---
title: "Relay Updates for Sync v1.1"
url: "https://docs.bsky.app/blog/relay-sync-updates"
date: "Fri, 02 May 2025 00:00:00 GMT"
author: ""
feed_url: "https://docs.bsky.app/blog/rss.xml"
---
<p>We have an updated relay implementation which incorporates Sync v1.1 protocol changes, and are preparing to switch over the <code>bsky.network</code> relay operated by Bluesky. This post describes new infrastructure developers can start using today and describes our plan for completing the migration.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="updated-relay-implementation">Updated Relay Implementation<a class="hash-link" href="https://docs.bsky.app/blog/relay-sync-updates#updated-relay-implementation" title="Direct link to Updated Relay Implementation">​</a></h2>
<p>The new version of the relay is available now in the <a href="https://github.com/bluesky-social/indigo/tree/main/cmd/relay" rel="noopener noreferrer" target="_blank"><code>indigo</code> go git repository</a>, named simply <code>relay</code>. It is a fork and refactor of the <code>bigsky</code> relay implementation, and several components have stuck around. The biggest change is that the relay no longer mirrors full repository data for all accounts in the network: sync v1.1 relays are now "non-archival". We have actually been operating <code>bsky.network</code> with a patched non-archival version of <code>bigsky</code> for some time, but this update includes a number of other changes:</p>
<ul>
<li>the new <code>#sync</code> message type is supported</li>
<li>the deprecated <code>#handle</code>, <code>#migration</code>, and <code>#tombstone</code> message types are fully removed (they were replaced with <code>#identity</code> and <code>#account</code>)</li>
<li>the sync v1.1 changes to the <code>#commit</code> message schema are supported</li>
<li>message validation and data limits updated to align with written specification</li>
<li>account hosting status and lifecycle updated to match written specification</li>
<li><code>#commit</code> messages validated with MST inversion (controlled by "lenient mode" flag)</li>
<li>new <code>com.atproto.sync.listHosts</code> endpoint to enumerate subscribed PDS instances</li>
<li><code>com.atproto.sync.getRepo</code> endpoint implemented as HTTP redirect to PDS instance</li>
<li>simplified configuration, operation, and SQL database schemas</li>
</ul>
<p>The relay can aggregate firehose messages for the full atproto network on a relatively small and inexpensive server, even with signature validation and MST inversion of every message on the firehose. The relay can handle thousands of messages per second using on the order of 2 vCPU cores, 12 GByte of RAM, and 30 Mbps of inbound and outbound bandwidth. Disk storage depends on the length of the configurable "backfill window." (A 24-hour backfill currently requires a couple hundred GByte of disk.)</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="staged-rollout">Staged Rollout<a class="hash-link" href="https://docs.bsky.app/blog/relay-sync-updates#staged-rollout" title="Direct link to Staged Rollout">​</a></h2>
<p>The end goal is to upgrade the <code>bsky.network</code> relay instance and for all active PDS hosts in the network to emit valid Sync v1.1 messages. We are rolling this out in stages to ensure that no active services in the network break.</p>
<p>We have two new production relay instances:</p>
<ul>
<li><code>relay1.us-west.bsky.network</code></li>
<li><code>relay1.us-east.bsky.network</code></li>
</ul>
<p>These both run the new relay implementation and attempt to crawl the entire atproto network. The two instances are operationally isolated and subscribe to PDS instances separately, though configuration will be synchronized between them periodically. They have sequence numbers which started at <code>20000000000</code> (20 billion) to distinguish them from the current <code>bsky.network</code> relay, which has a sequence around <code>8400000000</code> (8.4 billion). Note that the two new relays are independent from each other, have different sequence numbers, and will have different ordering of messages. Clients subscribing to these public hostnames will technically connect to <code>rainbow</code> daemons which fan-out the firehose.</p>
<p>We encourage service operators and protocol developers to experiment with these new instances. They are running in "lenient" MST validation mode and should work with existing PDS instances and implementations. PDS hosts can direct requestCrawl API requests to them. If hosts don't show up in listHosts, try making some repo updates to emit new <code>#commit</code> messages. Bluesky intends to operate both of these relays, at these hostnames, for the foreseeable future.</p>
<p>For the second stage of the roll-out (in the near future), we will update the <code>bsky.network</code> domain name to point at one of these new relay instances. If all goes smoothly, firehose consumers will simply start receiving events with a higher sequence number.</p>
<p>As a final stage, we will re-configure both instances to enforce MST inversion more strictly and drop <code>#commit</code> messages which fail validation. To ensure this does not break active hosts and accounts in the network, the relays currently attempt strict validation on every message and log failures (even if they are passed through on the firehose in "lenient" mode). We will monitor the logs and work with PDS operators who have not upgraded. If necessary, we can "ratchet" validation by host, meaning that new hosts joining the network are required to validate but specific existing hosts are given more time to update.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="other-sync-v11-progress">Other Sync v1.1 Progress<a class="hash-link" href="https://docs.bsky.app/blog/relay-sync-updates#other-sync-v11-progress" title="Direct link to Other Sync v1.1 Progress">​</a></h2>
<p>We have a few resources for PDS developers implementing Sync v1.1:</p>
<ul>
<li>the <code>goat</code> CLI tool has several <code>--verify</code> flags on firehose consumer command, which can be pointed at a local PDS instance for debugging</li>
<li>likewise the relay implementation can be running locally (eg, using sqlite) and pointed at a local PDS instance</li>
<li>interoperability test data in the <a href="https://github.com/bluesky-social/atproto-interop-tests" rel="noopener noreferrer" target="_blank"><code>bluesky-social/atproto-interop-tests</code></a> git repository, both for MST implementation details, and for commit proofs</li>
<li>the written specifications have not been updated yet, but the <a href="https://github.com/bluesky-social/proposals/tree/main/0006-sync-iteration" rel="noopener noreferrer" target="_blank">proposal doc</a> is thorough and will be the basis for updated specs</li>
</ul>
<p>While not strictly required by the protocol, both of the new relay hostnames support the <code>com.atproto.sync.listReposByCollection</code> endpoint (technically via the <code>collectiondir</code> microservice, not part of the relay itself). This endpoint makes it much more efficient to discover which accounts in the network have records of a given type and is particularly helpful for developers building new Lexicons and working with data independent of the <code>app.bsky.*</code> namespace.</p>
<p>The need for ordered repository CAR file exports has become more clear, and an early implementation was completed for the PDS reference implementation. That implementation is not performant enough to merge yet, and it may be some time before ordered CAR files are a norm in the network. The exact ordering also needs to be described more formally to ensure interoperation. Work has not yet started on the "partial synchronization" variant of <code>getRepo</code>, which will allow fetching a subset of the repository.</p>
