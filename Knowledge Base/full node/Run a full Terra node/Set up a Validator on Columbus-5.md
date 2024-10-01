---

title: Setup a Validator on Columbus-5
menu_order: 1
post_status: publish
post_excerpt: 
featured_image: 
taxonomy:
    category:
        - full-node
        - run-a-full-terra-node
    post_tag:
        - setup validator
        - Terra Classic
        - Columbus-5

---


### Discussed in https://github.com/orgs/classic-terra/discussions/521

<div type='discussions-op-text'>

<sup>Originally posted by **StrathCole** September 24, 2024</sup>

This is the short description of how to create a validator for the Terra Classic columbus-5 network.

## Current versions
`v3.1.5` (core)  - a non-state-breaking `v3.1.5-sim.2` is available for endpoints that rely on more accurate simulation
`v3.1.5` (oracle feeder)

## Endpoints
`RPC: https://terra-classic-rpc.publicnode.com`  
`LCD: https://terra-classic-lcd.publicnode.com`  

## Prerequisites
Linux Operating System (e.g. Ubuntu/Debian)
Quad-Core (preferably at least Hexa-Core) CPU, 64GB RAM and 1TB NVMe (preferably 2TB)

## Installing

1. Update system  
```bash
sudo apt update
sudo apt upgrade
```

2. Install packages
```bash
sudo apt install build-essential git
```

3. Clone the repository (if not done already)
```bash
git clone https://github.com/classic-terra/core
```

4. Make sure you have all tags available
```bash
cd ~/core
git fetch --all
git fetch --tags
```

5. Checkout the current testnet version tag unless advised differently
```bash
git checkout v3.1.5 #Adjust according to the testnet release at the top of this page
```

6. Install go if not yet done. Current go version used is `1.21`
```bash
wget -O go.tar.gz "https://dl.google.com/go/go1.21.13.linux-amd64.tar.gz"
# or if you want to use curl
curl -o go.tar.gz "https://dl.google.com/go/go1.21.13.linux-amd64.tar.gz"

sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go.tar.gz

sudo nano /etc/profile # or use your favorite editor
# add this line at the end
export PATH="$PATH:/usr/local/go/bin"

nano ~/.profile # or use your favorite editor
# add this line at the end
export PATH="$PATH:$HOME/go/bin"

# relogin or run
. /etc/profile
. ~/.profile

# verify go version
go version
```

7. Build and install
```bash
cd ~/core
make build && make install

# verify version
terrad version
```

8. Download addressbook and genesis file (if you are setting the node up for the first time)
```bash
terrad init <yourtestnetnodename>
cd ~/.terra/config
wget -O addrbook.json "https://dl2.quicksync.io/json/addrbook.terra.json" # or use curl
wget -O genesis.json "https://snapshots.hexxagon.io/cosmos/terra-classic/columbus-5/genesis.json" # or use curl
```

9. Download a snapshot to join the current testnet state
```bash
cd ~/.terra
sed -i -r 's/iavl-disable-fastnode = false/iavl-disable-fastnode = true/' config/app.toml
mv data data~backup
# find the latest snapshot on https://publicnode.com/snapshots#terra-classic and use that in the command below
wget -O snapshot.tar.lz4 "https://snapshots.publicnode.com/terra-classic-pruned-19928974-19928984.tar.lz4" # or use curl
lz4cat snapshot.tar.lz4 | tar xvf -
```

10. Start the node and catch up to latest height
```bash
terrad start # or use system service

# watch for status
terrad status # until catching up is false
```

## Joining the validator set

1. create a new wallet
```bash
terrad keys add valwallet

terrad keys list # remember/copy your wallet address
```

2. fund the wallet with some LUNC tokens

3. register the validator, adjust the settings for commission etc. accordingly  
**ATTENTION**  
Some settings cannot be changed, e.g. the maximum commission and maximum change rate

```bash
terrad tx staking create-validator --amount "10000000uluna" --commission-max-change-rate "0.01" --commission-max-rate "0.2" --commission-rate "0.1" --min-self-delegation 1 --pubkey $(terrad tendermint show-validator) --moniker myvalidator  --chain-id columbus-5 --gas auto --gas-prices 28.325uluna --gas-adjustment 2.5 --from valwallet
# change settings and --from according to your needs and the wallet you created in step 1.
```

4. check your validator is now in the active set via https://wallet.terra-classic.io (staking subsection) or
```bash
terrad q staking validators
```
It might be that you need more delegations to reach the active set.

## Set up Oracle Price Server and Feeder

1. If you want to use the dockerized version, make sure you have docker and docker compose installed. See official docs for how to do that. If you want to use the manual deployment, make sure you have nodejs and npm installed.

2. Create a new wallet which serves as the authorized feeder wallet for your validator. Feeding prices to the chain does not cost gas fees, but you need to send some minor funds to the feeder wallet nonetheless.

```bash
terrad keys add feederwallet
```

3. Delegate feeder privileges to the feeder wallet.

```bash
terrad tx oracle set-feeder <yourfeederwallet> --from <yourvalidatorkey> --chain-id columbus-5 --gas auto --gas-prices 28.325uluna --gas-adjustment 2
```

4. Follow the instructions in the oracle feeder repository to set the service up:
https://github.com/classic-terra/oracle-feeder/blob/main/README.md#using-docker-compose-recommended-experimental
or
https://github.com/classic-terra/oracle-feeder/blob/main/README.md#manual-deployment-instructions

**Remember to use the right network settings!** For example:
```bash
npm start vote -- \
   -d http://localhost:8532/latest \
   --lcd-url http://127.0.0.1:1317 \
   --chain-id columbus-5 \
   --validators <yourvaloperaddress> \
   --password <password>
```
You should use localhost for the oracle lcd, but you need to make sure you have the LCD enabled in your node config.  
To do that, edit `~/.terra/config/app.toml` and find `enabled = false` in the `[api]` section. Change it to `true` and restart the node.  
![grafik](https://github.com/user-attachments/assets/48b765af-151c-4245-ac51-6a69d18b65cb)

5. By checking the oracle logs, make sure you have the correct version of the oracle price server and feeder (see top of this page for the currently used version).

## Shutting down the validator permanently

If you decide to no longer take part in the network, please do **NOT** simply shut down your validator. Make sure that you properly undelegate all self-bonded tokens to unbond your validator. This can be done similar to this:

```bash
terrad tx staking unbond <youroperatoraddress> 10000000uluna --from <yourvalidatorkey> --chain-id columbus-5 --gas auto --gas-prices 28.325uluna --gas-adjustment 2
```

Make sure to check `terrad q staking validators` for the state of your validator. It should say `status: BOND_STATUS_UNBONDING`.

## Troubleshooting

### Feeder complains about `account terra***************vq7 not found`
Make sure you have sent funds to the feeder wallet. Otherwise the chain does not know it exists.

### Feeder does not report prices
Make sure the local LCD is running and you have enabled the API. Double-check you are using the correct version of both the core and the oracle.</div>
