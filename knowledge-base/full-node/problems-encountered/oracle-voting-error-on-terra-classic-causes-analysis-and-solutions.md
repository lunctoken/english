---

title: "Oracle Voting Error on Terra Classic: Causes, Analysis, and Solutions"
menu_order: 1
post_status: publish
post_excerpt: 
featured_image: 
taxonomy:
    epkb_post_type_1_category:
        - problems-encountered
    epkb_post_type_1_tag:
        - Oracle
        - Oracle Issue
        - Oracle Vote
---

<p>[giw_edit_link]</p>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In this article, we analyze a recent Oracle voting error experienced by some validators on the Terra Classic blockchain. We'll explore the potential causes, key theories discussed within the community, and solutions to avoid such issues in the future.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Problem Encountered</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Validators on the Terra Classic network reported a failure in retrieving the LUNC price data due to Oracle voting errors. This error caused approximately 4 minutes of downtime for some validators, leading to concerns about the dependency on external data sources like CoinGecko and the lack of redundancy in their setups.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Key error in oracle example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Sep 19 08:43:46 validator-c start.sh&#91;657931]: &#91;ERROR]&#91;Thu, 19 Sep 2024 08:43:46 GMT] Error: cannot find LUNC price
Sep 19 08:43:46 validator-c start.sh&#91;657931]: at preparePrices (/home/terra/oracle-feeder/feeder/src/vote.ts:114:11)</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Additionally, two CoinGecko outages were recorded in feeder:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;INFO]&#91;Thu, 19 Sep 2024 06:35:07 GMT] CoinGecko is now alive. (downtime 5.7 minutes)
&#91;INFO]&#91;Thu, 19 Sep 2024 08:46:13 GMT] CoinGecko is now alive. (downtime 5.9 minutes)</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-causes-and-theories">Causes and Theories</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Several potential causes for the issue were discussed:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Rate Limiting by CoinGecko</strong><br><br>CoinGecko is known to impose aggressive rate limits, which may have blocked validators that exceeded their API limits during the critical period. Validators affected by these limits might not have fallback options in place, which exacerbated the problem.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Dependency on External Price Sources</strong><br><br>Validators who rely solely on external price providers like CoinGecko might experience more frequent outages. Validators without multiple data sources configured were particularly affected by this issue, highlighting the risk of such dependencies.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Geo-blocking or Regional Access Restrictions</strong><br><br>Some validators suggested that geo-blocking might prevent certain regions from accessing specific LUNC price sources, which could explain why only some validators encountered the issue.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Lack of Redundancy in Price Data Feeds</strong><br><br>Validators who didn’t configure multiple price providers encountered issues when their primary price source failed. The absence of a secondary provider for LUNC prices left validators without crucial data.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading"><strong>Log Analysis</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Upon reviewing the logs, it became clear that affected validators should analyze their price server logs rather than their feeder logs to identify the root cause. In this case, the CoinGecko downtime directly aligned with the occurrence of the Oracle voting error.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading"><strong>Solutions</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To prevent similar issues in the future, validators are encouraged to adopt the following best practices:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Implement Redundancy in Price Data Feeds</strong><br>Validators should integrate multiple price providers (e.g., CoinGecko, Binance, KuCoin) to avoid relying on a single source. This will ensure that if one provider faces downtime or rate-limits, another provider can supply the necessary data.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Monitor Rate Limits</strong><br>Regularly monitor the rate limits of external services like CoinGecko and configure appropriate intervals for data requests to avoid exceeding these limits.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Self-hosted LCD Nodes</strong><br>Where possible, validators should consider hosting their own LCD (Light Client Daemon) nodes to reduce dependence on third-party endpoints. This allows for more control over the node's configuration and improves reliability.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Improved Error Handling in Feeder Scripts</strong><br>Validators should review and optimize their Oracle feeder scripts to ensure that errors, such as missing price data, are handled more gracefully and do not lead to prolonged downtimes.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading"><strong>Conclusion</strong></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Oracle voting error on Terra Classic highlights the importance of building redundancy into critical infrastructure. Validators should implement multiple price data sources, monitor rate limits, and adopt robust error-handling mechanisms to ensure uninterrupted service. By following these practices, validators can protect themselves from similar incidents in the future.</p>
<!-- /wp:paragraph -->
