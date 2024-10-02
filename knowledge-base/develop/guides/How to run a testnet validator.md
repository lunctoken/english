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

<!-- wp:paragraph -->
<p>This is the short description of how to create a validator for the Terra Classic rebel-2 network.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-current-testnet-rebel-2-version">Current TESTNET (rebel-2) version</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>v3.2.3-testnet.3</code> (core)<br><code>v3.1.5</code> (oracle feeder)</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-endpoints">Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>RPC: https://rpc.luncblaze.com</code><br><code>LCD: https://lcd.luncblaze.com</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you intend to run intensive tests, it is advised to run your own testnet node for faster queries.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-prerequisites">Prerequisites</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Linux Operating System<br>Similar specs to the mainnet validators, but 32GB RAM and 1TB NVMe should be enough.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-installing">Installing</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-update-system">1. Update system</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code class="language-bash">sudo apt update<br>sudo apt upgrade</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-install-packages"><p>2. Install packages</p></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code class="language-bash">sudo apt install build-essential git</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-clone-the-repository-if-not-done-already">3. Clone the repository (if not done already)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code class="language-bash">git clone https://github.com/classic-terra/core</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-make-sure-you-have-all-tags-available">4. Make sure you have all tags available</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code class="language-bash">cd ~/core<br>git fetch --all<br>git fetch --tags</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-5-checkout-the-current-testnet-version-tag-unless-advised-differently">5. Checkout the current testnet version tag unless advised differently</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code class="language-bash">git checkout v3.2.2 #Adjust according to the testnet release at the top of this page</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-6-install-go-if-not-yet-done-current-go-version-used-is-1-21">6. Install go if not yet done. Current go version used is 1.21</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>wget -O go.tar.gz "https://dl.google.com/go/go1.21.13.linux-amd64.tar.gz"
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
<pre class="wp-block-code"><code>terrad init &lt;yourtestnetnodename&gt;
cd ~/.terra/config
wget -O addrbook.json "https://luncblaze.com/rebel-2/addrbook.json" # or use curl
wget -O genesis.json "https://luncblaze.com/rebel-2/genesis.json" # or use curl</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-9-download-a-snapshot-to-join-the-current-testnet-state">9. Download a snapshot to join the current testnet state</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>cd ~/.terra
sed -i -r 's/iavl-disable-fastnode = false/iavl-disable-fastnode = true/' config/app.toml
mv data data~backup
wget -O snapshot.tar.lz4 "https://luncblaze.com/rebel-2/rebel2-latest.tar.lz4" # or use curl
lz4cat snapshot.tar.lz4 | tar xvf -</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-10-start-the-node-and-catch-up-to-latest-height">10. Start the node and catch up to latest height</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>terrad start # or use system service

# watch for status
terrad status # until catching up is false</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-joining-the-validator-set">Joining the validator set</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-create-a-new-wallet">1. Create a new wallet</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>bash
terrad keys add valwallet

terrad keys list # remember/copy your wallet address</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-request-testnet-funds-to-the-wallet-faucet-available-soon-by-contacting-strathcole-or-fragwuerdig">2. Request testnet funds to the wallet (faucet available soon) by contacting StrathCole or Fragwuerdig</h3>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-register-the-validator">3. Register the validator</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>terrad tx staking create-validator --amount "10000000uluna" --commission-max-change-rate "0.01" --commission-max-rate "0.2" --commission-rate "0.1" --min-self-delegation 1 --pubkey $(terrad tendermint show-validator) --moniker myvalidator  --chain-id rebel-2 --gas auto --gas-prices 28.325uluna --gas-adjustment 2.5 --from valwallet
# change settings and --from according to your needs and the wallet you created in step 1.</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-check-your-validator-is-now-in-the-active-set-via-https-wallet-terra-classic-io-nbsp-staking-subsection-remember-to-switch-to-testnet-or">4. Check your validator is now in the active set via <a href="https://wallet.terra-classic.io/">https://wallet.terra-classic.io/</a>&nbsp;(staking subsection, remember to switch to testnet) or</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>terrad q staking validators</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-set-up-oracle-price-server-and-feeder">Set up Oracle Price Server and Feeder</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-dockerized-version">1. Dockerized version</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you want to use the dockerized version, make sure you have docker and docker compose installed. See official docs for how to do that.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-manual-deployment">2. Manual deployment</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you want to use the manual deployment, make sure you have nodejs and npm installed.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-create-a-new-wallet-which-serves-as-the-authorized-feeder-wallet-for-your-validator-feeding-prices-to-the-chain-does-not-cost-gas-fees-but-you-need-to-send-some-minor-funds-to-the-feeder-wallet-nonetheless">3. Create a new wallet which serves as the authorized feeder wallet for your validator. Feeding prices to the chain does not cost gas fees, but you need to send some minor funds to the feeder wallet nonetheless.</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>terrad keys add feederwallet</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-delegate-feeder-privileges-to-the-feeder-wallet">4. Delegate feeder privileges to the feeder wallet.</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>terrad tx oracle set-feeder &lt;yourfeederwallet&gt; --from &lt;yourvalidatorkey&gt; --chain-id rebel-2 --gas auto --gas-prices 28.325uluna --gas-adjustment 2</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-5-follow-the-instructions-in-the-oracle-feeder-repository-to-set-the-service-up">5.Follow the instructions in the oracle feeder repository to set the service up:</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><br><a href="https://github.com/classic-terra/oracle-feeder/blob/main/README.md#using-docker-compose-recommended-experimental" target="_blank" rel="noreferrer noopener">https://github.com/classic-terra/oracle-feeder/blob/main/README.md#using-docker-compose-recommended-experimental</a><br>or<br><a href="https://github.com/classic-terra/oracle-feeder/blob/main/README.md#manual-deployment-instructions" target="_blank" rel="noreferrer noopener">https://github.com/classic-terra/oracle-feeder/blob/main/README.md#manual-deployment-instructions</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Remember to use the right network settings!</strong> For example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>npm start vote -- 
   -d http://localhost:8532/latest 
   --lcd-url http://127.0.0.1:1317 
   --chain-id rebel-2 
   --validators &lt;yourvaloperaddress&gt; 
   --password &lt;password&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>You should use localhost for the oracle lcd, but you need to make sure you have the LCD enabled in your node config.<br>To do that, edit <code>~/.terra/config/app.toml</code> and find <code>enabled = false</code> in the <code>[api]</code> section. Change it to <code>true</code> and restart the node.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"className":"wp-block-image size-full"} -->
<figure class="wp-block-image size-full"><img src="https://github.com/user-attachments/assets/48b765af-151c-4245-ac51-6a69d18b65cb" alt="grafik"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-6-by-checking-the-oracle-logs-make-sure-you-have-the-correct-version-of-the-oracle-price-server-and-feeder-see-top-of-this-page-for-the-currently-used-version">6. By checking the oracle logs, make sure you have the correct version of the oracle price server and feeder (see top of this page for the currently used version).</h3>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-coordination">Coordination</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We have a dedicated telegram group for rebel-2 participants. Please reach out to <a href="https://github.com/StrathCole">@StrathCole</a> / <a href="https://github.com/fragwuerdig">@fragwuerdig</a> to be added. We'll also announce upgrade plans there for the testnet.<br>Keep in mind that the voting period on rebel-2 is set to 1 day to allow quicker testing.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-leaving-testnet">Leaving testnet</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you decide to no longer take part in the testnet, please do <strong>NOT</strong> simply shut down your validator. Make sure that you properly undelegate all self-bonded tokens to unbond your validator. This can be done similar to this:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>terrad tx staking unbond &lt;youroperatoraddress&gt; 10000000uluna --from &lt;yourvalidatorkey&gt; --chain-id rebel-2 --gas auto --gas-prices 28.325uluna --gas-adjustment 2</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Make sure to check <code>terrad q staking validators</code> for the state of your validator. It should say <code>status: BOND_STATUS_UNBONDING</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-faucet-get-coins">Faucet (get coins)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Request testnet tokens from Faucet-Bot <a href="https://t.me/tcrebelfaucet_bot">https://t.me/tcrebelfaucet_bot</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-troubleshooting">Troubleshooting</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-feeder-complains-about-account-terra-vq7-not-found">Feeder complains about <code>account terra***************vq7 not found</code></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Make sure you have sent funds to the feeder wallet. Otherwise the chain does not know it exists.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-feeder-does-not-report-prices">Feeder does not report prices</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Make sure the local LCD is running and you have enabled the API. Double-check you are using the correct version of both the core and the oracle.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>Originally posted by @StrathCole in <a href="https://github.com/classic-terra/core/discussions/516" target="_blank" rel="noreferrer noopener">https://github.com/classic-terra/core/discussions/516</a></em></p>
<!-- /wp:paragraph -->

