---
title: "Enabling Account Migration Back to Bluesky’s PDS"
url: "https://docs.bsky.app/blog/incoming-migration"
date: "Fri, 26 Sep 2025 00:00:00 GMT"
author: ""
feed_url: "https://docs.bsky.app/blog/rss.xml"
---
<p>One of the core promises of AT is seamless account migration between PDS hosts. Since federation opened up in the AT network, it has been possible to migrate away from the Bluesky PDS and between non-Bluesky PDSs. However, once you left the Bluesky PDS, returning wasn’t an option.</p>
<p>Today, we’re removing that restriction and allowing returning users to migrate back to the Bluesky PDS. We hope this gives more users the confidence to explore other PDSs in the network, knowing they can return if needed. That being said, account migration remains a potentially destructive operation, so users should be aware of the risks before migrating between hosts.</p>
<p>This does not yet allow users who have never had a <code>bsky.social</code> account to migrate to our PDS. The migration flow works the same as described in the <a href="https://github.com/bluesky-social/pds/blob/main/ACCOUNT_MIGRATION.md" rel="noopener noreferrer" target="_blank">Account Migration</a> documentation, but instead of creating a new account, you’ll log into your existing <code>bsky.social</code> account, import your repo, and reactivate. The Bluesky PDS will automatically handle the diff and index any changes to the repository since you were last active on <code>bsky.social</code>.</p>
<p>We want to see more PDSs in the network and more users on non-Bluesky PDSs. Future work includes an account migration tool hosted at <code>bsky.social</code>, improvements to the PDS distribution - especially for mid-sized deployments, and auto-scaling rate limits at the Relay.</p>
