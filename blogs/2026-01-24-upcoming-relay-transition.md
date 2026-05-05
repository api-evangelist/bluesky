---
title: "Upcoming Relay Transition"
url: "https://docs.bsky.app/blog/relay-rollout"
date: "Sat, 24 Jan 2026 00:00:00 GMT"
author: ""
feed_url: "https://docs.bsky.app/blog/rss.xml"
---
<p>In coming days we are finally going to transition the <code>bsky.network</code> firehose endpoint to a newer relay implementation. This should not have a significant impact on most consumers.</p>
<p>More specifically, we are tagetting the morning <em>Tuesday, January 27th</em> (US/Pacific timezone). The <code>bsky.network</code> firehose will be switched from the <code>narelay.pop2.bsky.network</code> relay instance to the <code>relay1.us-west.bsky.network</code> relay instance.</p>
<p>This transition was <a href="https://docs.bsky.app/blog/relay-sync-updates">previously announced</a> but postponed until now. The majority of Bluesky internal services transitioned to the new relay instances in the past week.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="how-will-this-impact-relay-consumers">How will this impact relay consumers?<a class="hash-link" href="https://docs.bsky.app/blog/relay-rollout#how-will-this-impact-relay-consumers" title="Direct link to How will this impact relay consumers?">​</a></h2>
<p>If you are connected to the <code>bsky.network</code> hostname, your websocket connection may drop, and the cursor sequence will jump forward by a significant delta. If your client implements auto-reconnection, no manual intervention should be necessary, and impact should be minimal.</p>
<p>It is likely that some events will be duplicated with new (higher) sequence numbers. The per-account revision (<code>rev</code>) field can be used to de-duplicate events at the account level. For many use-cases (such as bots, feed generators, etc), re-processing a small number of events should not be problematic.</p>
<p>The backfill window of the <code>bsky.network</code> instance should reflect what is seen on the firehose. That is, reconnecting with an "old" (lower) cursor value should not result in replaying the full "new relay" 72 hour backfill window. This is because an intermediate service (<code>rainbow</code>) is inserted between the relay backend and the public endpoint.</p>
<p>Operators can implement the transition on their own schedule by connecting directly to specific relay instances:</p>
<ul>
<li><code>narelay.pop2.bsky.network</code> is the current relay behind <code>bsky.network</code>. It has been in operation since Fall 2024 and runs the older "bigsky" implementation. The current sequence is around <code>17502400000</code> (aka, 17 billion). This instance will be shut down a week or two after the transition.</li>
<li><code>relay1.us-west.bsky.network</code> will be the new relay behind <code>bsky.network</code>. The current sequence is around <code>26653242499</code> (aka, 26 billion).</li>
<li><code>relay1.us-east.bsky.network</code> is an alternative relay running in a separate data center. The current sequence is around <code>26653242087</code> (aka, 26 billion).</li>
</ul>
<p>Note that the east/west sequence numbers are similar (because they started at the same time), but are not identical. The content is very similar (they are both full-network relays), but are not seamlessly interchangeable.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="what-about-jetstream-consumers">What about Jetstream Consumers?<a class="hash-link" href="https://docs.bsky.app/blog/relay-rollout#what-about-jetstream-consumers" title="Direct link to What about Jetstream Consumers?">​</a></h2>
<p>Jetstream uses timestamp cursors, so no observable cursor jump will happen. Consumers may see a small number of duplicate/replay events around the transition.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="how-will-this-impact-pds-hosts">How will this impact PDS hosts?<a class="hash-link" href="https://docs.bsky.app/blog/relay-rollout#how-will-this-impact-pds-hosts" title="Direct link to How will this impact PDS hosts?">​</a></h2>
<p>There should not be any noticeable impact from this transition. The new relay instances have already been consuming from all PDS hosts for many months, "request crawl" messages are mirrored, and rate limits have been synchronized between old and new relay instances.</p>
<p>The new relay implementation does include support for string verification of "MST inversion proofs", part of the <a href="https://github.com/bluesky-social/proposals/tree/main/0006-sync-iteration" rel="noopener noreferrer" target="_blank">Sync 1.1</a> protocol changes. However, strict verification will still be disabled for now.</p>
<p>We do intent to enable strict MST verification in the near future. The reference PDS implementation has included the required changes for some time. We will monitor which PDS instances and implementations might be impacted and work with developers and operators before making that change, and will make public announcements ahead of time.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="why-are-we-doing-this">Why are we doing this?<a class="hash-link" href="https://docs.bsky.app/blog/relay-rollout#why-are-we-doing-this" title="Direct link to Why are we doing this?">​</a></h2>
<p>The new relay includes support for Sync 1.1 protocol features, such as the <code>#sync</code> message type and additional metadata supporting "MST inversion". It also fixes a handful of bugs that have caused operational problems with individual PDS implementations.</p>
<p>The new implementation also removes a huge amount of deprecated code related to "archival" relay operation and indexing of record data in the relay itself. It should be easier to maintain and develop new features for the relay going forward, including smarter rate-limiting and resolving connection issues with upstream PDS instances.</p>
