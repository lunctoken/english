---

title: Setup a Validator on Columbus-5
menu_order: 1
post_status: publish
post_excerpt: 
featured_image: 
taxonomy:
    category:
        - run-a-full-terra-node
    post_tag:
        - setup validator
        - Terra Classic
        - Columbus-5

---
<!-- wp:heading {"level":5} -->
<h5 class="wp-block-heading" id="h-discussed-in-https-github-com-orgs-classic-terra-discussions-521">Discussed in <a href="https://github.com/orgs/classic-terra/discussions/521" target="_blank" rel="noopener">https://github.com/orgs/classic-terra/discussions/521</a></h5>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><sup>Originally posted by **StrathCole** September 24, 2024</sup></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is the short description of how to create a validator for the Terra Classic columbus-5 network.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-current-versions"><strong>Current versions</strong></h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>`v3.1.5` (core) - a non-state-breaking `v3.1.5-sim.2` is available for endpoints that rely on more accurate simulation<br>`v3.1.5` (oracle feeder)</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-endpoints"><strong>Endpoints</strong></h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>RPC: https://terra-classic-rpc.publicnode.com</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>LCD: https://terra-classic-lcd.publicnode.com</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-prerequisites"><strong>Prerequisites</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Linux Operating System (e.g. Ubuntu/Debian)<br>Quad-Core (preferably at least Hexa-Core) CPU, 64GB RAM and 1TB NVMe (preferably 2TB)</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-installing"><strong>Installing</strong></h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-update-system">1. Update system</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
sudo apt update
sudo apt upgrade</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-install-packages">2. Install packages</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
sudo apt install build-essential git</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-clone-the-repository-if-not-done-already">3. Clone the repository (if not done already)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
git clone https://github.com/classic-terra/core</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-make-sure-you-have-all-tags-available">4. Make sure you have all tags available</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
cd ~/core
git fetch --all
git fetch --tags</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-5-checkout-the-current-testnet-version-tag-unless-advised-differently">5. Checkout the current testnet version tag unless advised differently</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
git checkout v3.1.5 #Adjust according to the testnet release at the top of this page</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-6-install-go-if-not-yet-done-current-go-version-used-is-1-21">6. Install go if not yet done. Current go version used is 1.21</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
wget -O go.tar.gz "https://dl.google.com/go/go1.21.13.linux-amd64.tar.gz"
# or if you want to use curl
curl -o go.tar.gz "https://dl.google.com/go/go1.21.13.linux-amd64.tar.gz"

sudo rm -rf /usr/local/go &amp;&amp; sudo tar -C /usr/local -xzf go.tar.gz

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
go version</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-7-build-and-install">7. Build and install</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
cd ~/core
make build &amp;&amp; make install

# verify version
terrad version</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-8-download-addressbook-and-genesis-file-if-you-are-setting-the-node-up-for-the-first-time">8. Download addressbook and genesis file (if you are setting the node up for the first time)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
terrad init 
cd ~/.terra/config
wget -O addrbook.json "https://dl2.quicksync.io/json/addrbook.terra.json" # or use curl
wget -O genesis.json "https://snapshots.hexxagon.io/cosmos/terra-classic/columbus-5/genesis.json" # or use curl</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-9-download-a-snapshot-to-join-the-current-testnet-state">9. Download a snapshot to join the current testnet state</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
cd ~/.terra
sed -i -r 's/iavl-disable-fastnode = false/iavl-disable-fastnode = true/' config/app.toml
mv data data~backup
# find the latest snapshot on https://publicnode.com/snapshots#terra-classic and use that in the command below
wget -O snapshot.tar.lz4 "https://snapshots.publicnode.com/terra-classic-pruned-19928974-19928984.tar.lz4" # or use curl
lz4cat snapshot.tar.lz4 | tar xvf -</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-10-start-the-node-and-catch-up-to-latest-height">10. Start the node and catch up to latest height</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
terrad start # or use system service

# watch for status
terrad status # until catching up is false</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-joining-the-validator-set">Joining the validator set</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-create-a-new-wallet">1. create a new wallet</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
terrad keys add valwallet

terrad keys list # remember/copy your wallet address</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-fund-the-wallet-with-some-lunc-tokens">2. fund the wallet with some LUNC tokens</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>3. register the validator, adjust the settings for commission etc. accordingly<br>**ATTENTION**<br>Some settings cannot be changed, e.g. the maximum commission and maximum change rate</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>```bash<br>terrad tx staking create-validator --amount "10000000uluna" --commission-max-change-rate "0.01" --commission-max-rate "0.2" --commission-rate "0.1" --min-self-delegation 1 --pubkey $(terrad tendermint show-validator) --moniker myvalidator --chain-id columbus-5 --gas auto --gas-prices 28.325uluna --gas-adjustment 2.5 --from valwallet<br># change settings and --from according to your needs and the wallet you created in step 1.<br>```</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>4. check your validator is now in the active set via https://wallet.terra-classic.io (staking subsection) or<br>```bash<br>terrad q staking validators<br>```<br>It might be that you need more delegations to reach the active set.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>## Set up Oracle Price Server and Feeder</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>1. If you want to use the dockerized version, make sure you have docker and docker compose installed. See official docs for how to do that. If you want to use the manual deployment, make sure you have nodejs and npm installed.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>2. Create a new wallet which serves as the authorized feeder wallet for your validator. Feeding prices to the chain does not cost gas fees, but you need to send some minor funds to the feeder wallet nonetheless.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>```bash<br>terrad keys add feederwallet<br>```</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>3. Delegate feeder privileges to the feeder wallet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>```bash<br>terrad tx oracle set-feeder --from --chain-id columbus-5 --gas auto --gas-prices 28.325uluna --gas-adjustment 2<br>```</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>4. Follow the instructions in the oracle feeder repository to set the service up:<br>https://github.com/classic-terra/oracle-feeder/blob/main/README.md#using-docker-compose-recommended-experimental<br>or<br>https://github.com/classic-terra/oracle-feeder/blob/main/README.md#manual-deployment-instructions</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>**Remember to use the right network settings!** For example:<br>```bash<br>npm start vote --<br>-d http://localhost:8532/latest<br>--lcd-url http://127.0.0.1:1317<br>--chain-id columbus-5<br>--validators <br>--password ```<br>You should use localhost for the oracle lcd, but you need to make sure you have the LCD enabled in your node config.<br>To do that, edit `~/.terra/config/app.toml` and find `enabled = false` in the `[api]` section. Change it to `true` and restart the node.<br>![grafik](https://github.com/user-attachments/assets/48b765af-151c-4245-ac51-6a69d18b65cb)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>5. By checking the oracle logs, make sure you have the correct version of the oracle price server and feeder (see top of this page for the currently used version).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>## Shutting down the validator permanently</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you decide to no longer take part in the network, please do **NOT** simply shut down your validator. Make sure that you properly undelegate all self-bonded tokens to unbond your validator. This can be done similar to this:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>```bash<br>terrad tx staking unbond 10000000uluna --from --chain-id columbus-5 --gas auto --gas-prices 28.325uluna --gas-adjustment 2<br>```</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Make sure to check `terrad q staking validators` for the state of your validator. It should say `status: BOND_STATUS_UNBONDING`.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>## Troubleshooting</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>### Feeder complains about `account terra***************vq7 not found`<br>Make sure you have sent funds to the feeder wallet. Otherwise the chain does not know it exists.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>### Feeder does not report prices<br>Make sure the local LCD is running and you have enabled the API. Double-check you are using the correct version of both the core and the oracle.</p>
<!-- /wp:paragraph -->
