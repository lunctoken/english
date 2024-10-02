---

title: "How to run a testnet validator"
menu_order: 1
post_status: publish
post_excerpt: 
featured_image: 
taxonomy:
    category:
        - develop
        - guides
    post_tag:
        - Testnet
        - Validator
        - Run a Validator

---

<p>[giw_edit_link]</p>

<!-- wp:paragraph -->
<p>Running a testnet validator is a great way to participate in the Terra Classic ecosystem and contribute to its stability.<br>If you’re looking for a detailed guide on how to set up and run a testnet validator, check out this comprehensive walkthrough by StrathCole on GitHub. It covers all the necessary steps and configurations to get started.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>👉 <a href="https://github.com/orgs/classic-terra/discussions/516" target="_blank" rel="noreferrer noopener">Click here for the full guide</a></p>
<!-- /wp:paragraph -->

This is the short description of how to create a validator for the Terra Classic rebel-2 network.

## Current TESTNET (rebel-2) version
`v3.2.3-testnet.3` (core)  
`v3.1.5` (oracle feeder)

## Endpoints
`RPC: https://rpc.luncblaze.com`  
`LCD: https://lcd.luncblaze.com`  

If you intend to run intensive tests, it is advised to run your own testnet node for faster queries.

## Prerequisites
Linux Operating System
Similar specs to the mainnet validators, but 32GB RAM and 1TB NVMe should be enough.

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
git checkout v3.2.2 #Adjust according to the testnet release at the top of this page
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
wget -O addrbook.json "https://luncblaze.com/rebel-2/addrbook.json" # or use curl
wget -O genesis.json "https://luncblaze.com/rebel-2/genesis.json" # or use curl
```

9. Download a snapshot to join the current testnet state
```bash
cd ~/.terra
sed -i -r 's/iavl-disable-fastnode = false/iavl-disable-fastnode = true/' config/app.toml
mv data data~backup
wget -O snapshot.tar.lz4 "https://luncblaze.com/rebel-2/rebel2-latest.tar.lz4" # or use curl
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

2. request testnet funds to the wallet (faucet available soon) by contacting StrathCole or Fragwuerdig

3. register the validator
```bash
terrad tx staking create-validator --amount "10000000uluna" --commission-max-change-rate "0.01" --commission-max-rate "0.2" --commission-rate "0.1" --min-self-delegation 1 --pubkey $(terrad tendermint show-validator) --moniker myvalidator  --chain-id rebel-2 --gas auto --gas-prices 28.325uluna --gas-adjustment 2.5 --from valwallet
# change settings and --from according to your needs and the wallet you created in step 1.
```

4. check your validator is now in the active set via https://wallet.terra-classic.io (staking subsection, remember to switch to testnet) or
```bash
terrad q staking validators
```

## Set up Oracle Price Server and Feeder

1. If you want to use the dockerized version, make sure you have docker and docker compose installed. See official docs for how to do that. If you want to use the manual deployment, make sure you have nodejs and npm installed.

2. Create a new wallet which serves as the authorized feeder wallet for your validator. Feeding prices to the chain does not cost gas fees, but you need to send some minor funds to the feeder wallet nonetheless.

```bash
terrad keys add feederwallet
```

3. Delegate feeder privileges to the feeder wallet.

```bash
terrad tx oracle set-feeder <yourfeederwallet> --from <yourvalidatorkey> --chain-id rebel-2 --gas auto --gas-prices 28.325uluna --gas-adjustment 2
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
   --chain-id rebel-2 \
   --validators <yourvaloperaddress> \
   --password <password>
```
You should use localhost for the oracle lcd, but you need to make sure you have the LCD enabled in your node config.  
To do that, edit `~/.terra/config/app.toml` and find `enabled = false` in the `[api]` section. Change it to `true` and restart the node.  
![grafik](https://github.com/user-attachments/assets/48b765af-151c-4245-ac51-6a69d18b65cb)

5. By checking the oracle logs, make sure you have the correct version of the oracle price server and feeder (see top of this page for the currently used version).

## Coordination

We have a dedicated telegram group for rebel-2 participants. Please reach out to @StrathCole / @fragwuerdig to be added. We'll also announce upgrade plans there for the testnet.  
Keep in mind that the voting period on rebel-2 is set to 1 day to allow quicker testing.

## Leaving testnet

If you decide to no longer take part in the testnet, please do **NOT** simply shut down your validator. Make sure that you properly undelegate all self-bonded tokens to unbond your validator. This can be done similar to this:

```bash
terrad tx staking unbond <youroperatoraddress> 10000000uluna --from <yourvalidatorkey> --chain-id rebel-2 --gas auto --gas-prices 28.325uluna --gas-adjustment 2
```

Make sure to check `terrad q staking validators` for the state of your validator. It should say `status: BOND_STATUS_UNBONDING`.

## Faucet (get coins)
Request testnet tokens from Faucet-Bot https://t.me/tcrebelfaucet_bot

## Troubleshooting

### Feeder complains about `account terra***************vq7 not found`
Make sure you have sent funds to the feeder wallet. Otherwise the chain does not know it exists.

### Feeder does not report prices
Make sure the local LCD is running and you have enabled the API. Double-check you are using the correct version of both the core and the oracle.

_Originally posted by @StrathCole in https://github.com/classic-terra/core/discussions/516_
