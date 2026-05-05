---
title: "Taking AT to the IETF"
url: "https://docs.bsky.app/blog/taking-at-to-ietf"
date: "Tue, 23 Sep 2025 00:00:00 GMT"
author: ""
feed_url: "https://docs.bsky.app/blog/rss.xml"
---
<p>Last week we posted two drafts to the IETF Data Tracker. This is the first major step towards standardizing parts of AT in an effort to establish long-term governance for the protocol.</p>
<p>In particular, we’ve submitted two Internet-Drafts:</p>
<p><a href="https://datatracker.ietf.org/doc/draft-holmgren-at-repository/" rel="noopener noreferrer" target="_blank">Authenticated Transfer Repository and Synchronization</a>: a proposed standard that specifies the AT repository format and sync semantics</p>
<p><a href="https://datatracker.ietf.org/doc/draft-newbold-at-architecture/" rel="noopener noreferrer" target="_blank">Authenticated Transfer: Architecture Overview</a>: an informational draft that goes over the architecture of the broader network and describes how the repository fits into it.</p>
<p>Just today, we were approved for a Birds of a Feather (BOF) session at <a href="https://www.ietf.org/meeting/124/" rel="noopener noreferrer" target="_blank">IETF 124</a> in Montreal from November 1-7. Details on the BOF can be found <a href="https://datatracker.ietf.org/doc/bofreq-newbold-authenticated-transfer/" rel="noopener noreferrer" target="_blank">Here</a>.</p>
<p>A BOF is a part of the formal IETF process for forming a working group. It involves pulling together interested parties in order to determine if the IETF is a good fit for chartering a working group to work on a particular technology.</p>
<p>This is a “non-working group forming” BOF. The intention is to get feedback on both the charter for the WG and the drafts that we’ve submitted. If things go well, then we’d likely do an interim BOF between IETF 124 and 125 to actually form a working group.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="what-were-planning-to-bring-and-what-were-not">What We’re Planning to Bring (and What We’re Not)<a class="hash-link" href="https://docs.bsky.app/blog/taking-at-to-ietf#what-were-planning-to-bring-and-what-were-not" title="Direct link to What We’re Planning to Bring (and What We’re Not)">​</a></h2>
<p>We’re specifically focusing on the repository and sync protocol. We’re not planning to bring Lexicon, AT’s particular OAuth profile, Auth scopes, PLC, the handle system, or other AT components to the IETF right now.</p>
<p>A few reasons for the narrow scope:</p>
<ul>
<li>Working groups need focused charters, especially when bringing a new protocol to the IETF</li>
<li>The repo and sync protocol is the most foundational part of AT and is therefore the most impactful to have under neutral governance</li>
<li>The repo and sync protocol is the most “IETF-flavored” part of the stack, especially with its reliance on CBOR and WebSockets (both IETF specifications)</li>
</ul>
<p>If things go well for both sides, we may consider rechartering the working group later. Whether or not a working group forms will not impact how new AT features such as private state are designed or rolled out.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="why-ietf">Why IETF?<a class="hash-link" href="https://docs.bsky.app/blog/taking-at-to-ietf#why-ietf" title="Direct link to Why IETF?">​</a></h2>
<p>This is part of an ongoing effort to mature the governance of AT. (See also: the parallel work that we’re pushing forward on <a href="https://docs.bsky.app/blog/plc-directory-org" rel="noopener noreferrer" target="_blank">moving PLC to an independent organization</a>)</p>
<p>We want AT to have a neutral long-term home, and the IETF seems like a natural fit for several reasons. It’s the home of many internet protocols that you know and use every day: HTTP, TLS, SMTP, OAuth, WebSockets, and many others. The IETF has an open, consensus-driven process that anyone can participate in. And importantly, the IETF cares about both the decentralization of the internet while also keeping it functioning well in practice. This balance between idealism and pragmatism matches how we’ve approached the challenges of building a large-scale decentralized social networking protocol.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="what-you-can-do">What You Can Do<a class="hash-link" href="https://docs.bsky.app/blog/taking-at-to-ietf#what-you-can-do" title="Direct link to What You Can Do">​</a></h2>
<p>Read the drafts! Take a look at what we’ve submitted and see what you think. We have a <a href="https://github.com/bluesky-social/ietf-drafts" rel="noopener noreferrer" target="_blank">public GitHub repo</a> where you can comment on the drafts and provide feedback. We’re hoping to iterate on the drafts at least once before the BOF and already have a few issues noted that we need to address.</p>
<p>If you’re planning to attend IETF 124 in Montreal, let us know! We’d love to connect with folks who are interested in this work.</p>
