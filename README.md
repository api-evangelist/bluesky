# Bluesky (bluesky)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

API for the Bluesky decentralized social network built on the AT Protocol.

**APIs.json:** [https://raw.githubusercontent.com/api-search/bluesky/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-search/bluesky/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- At-Protocol
- Decentralized
- Federated
- Open-Source
- Social Networks
- Social-Media

## Timestamps

- **Created:** 2024-11-16
- **Modified:** 2026-05-29

## APIs

### Bluesky API

The Bluesky API allows you to work programmatically with actors, feeds, graph, conversations, and other resources available for the the Bluesky app and social network.

- **Human URL:** [https://docs.bsky.app/](https://docs.bsky.app/)

#### Tags

- Social Networks

#### Properties

- [Documentation](https://docs.bsky.app/)
- [OpenAPI](openapi/bluesky-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/bluesky.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bluesky.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](https://www.postman.com/api-evangelist/bluesky/collection/ubo2xuv/bluesky-api) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [API Reference](https://docs.bsky.app/docs/api/at-protocol-xrpc-api)
- [H T T P Reference](https://docs.bsky.app/docs/category/http-reference)
- [A P I Directory](https://docs.bsky.app/docs/advanced-guides/api-directory)
- [Authentication](https://docs.bsky.app/docs/advanced-guides/oauth-client)
- [Rate Limits](https://docs.bsky.app/docs/advanced-guides/rate-limits)
- [Firehose](https://docs.bsky.app/docs/advanced-guides/firehose)
- [AsyncAPI](asyncapi/bluesky-asyncapi.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/latest)
- [Moderation](https://docs.bsky.app/docs/advanced-guides/moderation)
- [Identity](https://docs.bsky.app/docs/advanced-guides/resolving-identities)
- [Summary](undefined)

### Bluesky Jetstream

Jetstream is a simplified JSON event stream for the AT Protocol that converts CBOR-encoded MST blocks from the firehose into JSON objects over WebSocket connections, making it easier to consume real-time Bluesky network events for building feed generators, bots, and search engines.

- **Human URL:** [https://docs.bsky.app/blog/jetstream](https://docs.bsky.app/blog/jetstream)

#### Tags

- Events
- Social Networks
- Streaming

#### Properties

- [Documentation](https://docs.bsky.app/blog/jetstream)
- [GitHub Repository](https://github.com/bluesky-social/jetstream)
- [AsyncAPI](asyncapi/bluesky-asyncapi.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/latest)
- [Postman Collection](collections/bluesky.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/bluesky.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/bluesky-pbc)
- [Bots](https://docs.bsky.app/docs/starter-templates/bots)
- [Support](https://docs.bsky.app/docs/category/support)
- [Blog](https://docs.bsky.app/blog)
- [Getting Started](https://docs.bsky.app/docs/get-started)
- [Templates](https://docs.bsky.app/docs/category/starter-templates)
- [Tutorials](https://docs.bsky.app/docs/category/tutorials)
- [Newsletter](https://docs.bsky.app/docs/support/mailing-list)
- [Postman Workspace](https://www.postman.com/api-evangelist/bluesky/overview)
- [Guidelines](https://docs.bsky.app/docs/support/developer-guidelines)
- [Custom Feeds](https://docs.bsky.app/docs/starter-templates/custom-feeds)
- [Protocol](https://docs.bsky.app/docs/advanced-guides/atproto)
- [Advanced Guides](https://docs.bsky.app/docs/category/advanced-guides)
- [Terms of Service](https://bsky.social/about/support/tos)
- [Privacy Policy](https://bsky.social/about/support/privacy-policy)
- [Community Guidelines](https://bsky.social/about/support/community-guidelines)
- [S D Ks](https://atproto.com/sdks)
- [Protocol Overview](https://atproto.com/guides/overview)
- [Specifications](https://atproto.com/)
- [Git Hub  Org](https://github.com/bluesky-social)
- [GitHub Repository](https://github.com/bluesky-social/atproto)
- [GitHub Repository](https://github.com/bluesky-social/feed-generator)
- [GitHub Repository](https://github.com/bluesky-social/ozone)
- [Forum](https://github.com/bluesky-social/atproto/discussions)
- [Summary](undefined)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
**Email:** support@bsky.app
**URL:** https://bsky.social
