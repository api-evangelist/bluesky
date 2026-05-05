---
title: "OAuth Improvements"
url: "https://docs.bsky.app/blog/oauth-improvements"
date: "Thu, 12 Jun 2025 00:00:00 GMT"
author: ""
feed_url: "https://docs.bsky.app/blog/rss.xml"
---
<p>We've been making improvements to the end-user and developer experiences with atproto OAuth, and wanted to share some updates.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="email-transitional-scope">Email Transitional Scope<a class="hash-link" href="https://docs.bsky.app/blog/oauth-improvements#email-transitional-scope" title="Direct link to Email Transitional Scope">​</a></h2>
<p>One of the few gaps between password authentication and the initial ("transitional") set of OAuth scopes was access to an account's email address (and email confirmation status). We have implemented a new OAuth scope, <code>transition:email</code> that clients can request for access to the account's email. The email itself can be accessed via the <code>com.atproto.server.getSession</code> endpoint on the PDS, using an OAuth access token.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="public-versus-confidential-clients">Public versus Confidential Clients<a class="hash-link" href="https://docs.bsky.app/blog/oauth-improvements#public-versus-confidential-clients" title="Direct link to Public versus Confidential Clients">​</a></h2>
<p>The difference between "public" and "confidential" OAuth clients is one of the more complex tradeoffs for application developers. Tradeoffs in security, privacy, trust, application architecture, session lifetime, and user experience are all entangled.</p>
<p>We have a new essay that tries to untangle this knot, explaining the security concerns and trade-offs involved: <a href="https://github.com/bluesky-social/atproto/discussions/3950" rel="noopener noreferrer" target="_blank">"OAuth Client Security in the Atmosphere"</a>.</p>
<p>We also have a proposal to enable a new architecture: <a href="https://github.com/bluesky-social/proposals/tree/main/0010-client-assertion-backend" rel="noopener noreferrer" target="_blank">"Client Assertion Backend for Browser-based Applications"</a>. In this architecture, in-browser web apps (SPAs) would communicate with a simple and generic backend server to generate client attestations for token requests. The backend could "veto" client sessions (to address security incidents), but would not directly mediate token requests. This would be distinct from the Token Mediating Backend (TMB) architecture. Note that with both architectures, the server can not hijack the session or make "offline" authenticated requests on the user's behalf, assuming the DPoP keys are held securely on-device by the web app.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="session-lifetimes">Session Lifetimes<a class="hash-link" href="https://docs.bsky.app/blog/oauth-improvements#session-lifetimes" title="Direct link to Session Lifetimes">​</a></h2>
<p>Many developers are encountering friction with OAuth session lifetimes, especially using in-browser "public" clients. The existing lifetime limitations mean that un-refreshed sessions would time out after just two days.</p>
<p>There will always be shorter session lifetimes for public clients, but we think the session lifetimes can be increased without a significant security trade-off. We are planning to roll out the following session lifetimes:</p>
<ul>
<li>Public Clients<!-- -->
<ul>
<li>increase overall session lifetime (after which tokens can not be refreshed) from one week to two weeks</li>
<li>increase the lifetime of individual refresh tokens from two days to two weeks (the same as the overall session lifetime; refreshing tokens does not extend the overall session)</li>
</ul>
</li>
<li>Confidential Clients<!-- -->
<ul>
<li>increase the lifetime of individual refresh tokens from one month to three months (if tokens are not refreshed, the session ends before the overall session lifetime limit)</li>
<li>increase overall session lifetime (assuming tokens are refreshed) from one year to two years; including existing sessions</li>
</ul>
</li>
</ul>
<p>Remember that the maximum lifetime of OAuth <em>access tokens</em> is much shorter: the specification recommends a limit of 30 minutes, and the reference implementation uses 15 minutes.</p>
<p>There is a tracking issue here: <a href="https://github.com/bluesky-social/atproto/pull/3883" rel="noopener noreferrer" target="_blank">bluesky-social/atproto#3883</a></p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="auth-scopes">Auth Scopes<a class="hash-link" href="https://docs.bsky.app/blog/oauth-improvements#auth-scopes" title="Direct link to Auth Scopes">​</a></h2>
<p>Protocol design on Auth Scopes is wrapping up. This functionality will allow app developers to request granular permissions to specific atproto resources, like records (by NSID), remote API endpoints (using service proxying), account hosting status, and identity updates. The system will also allow Lexicon designers to define higher-level "bundles" of permissions for atproto resources in the same NSID namespace, to make end-user permission requests simpler.</p>
<p>You can learn more by reading an earlier <a href="https://github.com/bluesky-social/atproto/discussions/3655" rel="noopener noreferrer" target="_blank">draft of this protocol feature from March 2025</a>.</p>
<p>We will begin server-side implementation and integration work on this feature soon after a final proposal is published.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="other-changes-and-bug-fixes">Other Changes and Bug Fixes<a class="hash-link" href="https://docs.bsky.app/blog/oauth-improvements#other-changes-and-bug-fixes" title="Direct link to Other Changes and Bug Fixes">​</a></h2>
<p>The DPoP JWTs created by the <code>@atproto/oauth-client</code> TypeScript package incorrectly included query parameters as part of the <code>htu</code> request URL field, which goes against the DPoP specification. The client package has been fixed as of version <a href="https://www.npmjs.com/package/@atproto/oauth-client?activeTab=versions" rel="noopener noreferrer" target="_blank">0.3.18</a>.</p>
<p>The Authorization Server used to require a DPoP proof during Pushed Authorization Requests when the parameters contained a <code>dpop_jkt</code>. This was not valid per <a href="https://datatracker.ietf.org/doc/html/rfc9449#section-10.1-2.1" rel="noopener noreferrer" target="_blank">spec</a> and is thus no longer the case.</p>
<p>The OAuth Client Implementation Guide previously instructed developers to include the Auth Server Issuer (host URL) in DPoP JWTs included on authenticated requests to the PDS, in the <code>iss</code> field. The example Python code and TypeScript OAuth client implementation also included this field. This is not required by the DPoP specification, and should <em>not</em> be implemented by clients or SDKs. We have updated the guide and implementations to remove this field.</p>
<p>The TypeScript OAuth client SDK was incorrectly sending HTTP POST requests using JSON bodies, instead of form-encoding (the server was flexible to either encoding). This has been corrected, and requests that should use form-encoding (like PAR) now do.</p>
<p>A couple of subtle bugs with the TypeScript OAuth client SDK and DPoP nonce caching have been fixed.</p>
<p>If a client app redirects to the reference PDS without a <code>login_hint</code>, and the user was already logged in, the auth flow now proceeds directly to an account selector, instead of displaying a "Create Account or Log In" page. This makes the flow smoother and reduces a click.</p>
<p>The client app redirect URL no longer needs to be on the same host origin as the client metadata document.</p>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="deprecation-notice">Deprecation notice<a class="hash-link" href="https://docs.bsky.app/blog/oauth-improvements#deprecation-notice" title="Direct link to Deprecation notice">​</a></h2>
<p>The following invalid behaviors were detected in our reference implementation. As of now, these are considered as deprecated and might change in the future. Make sure you use the latest version of our SDKs, and if you implemented your own, please check that it is compliant.</p>
<ul>
<li>
<p>DPoP proofs that contain a query or fragment in the <code>htu</code> claim should be rejected.</p>
</li>
<li>
<p>JWT for the client attestation using the <code>private_key_jwt</code> authentication method <a href="https://www.rfc-editor.org/rfc/rfc7523.html#section-3" rel="noopener noreferrer" target="_blank">MUST</a> contain an <code>exp</code> claim. This is currently not enforced by the reference implementation.</p>
</li>
<li>
<p>When performing a Pushed Authorization Request, the client <a href="https://datatracker.ietf.org/doc/html/rfc9449#section-10.1-2.1" rel="noopener noreferrer" target="_blank">must</a> either provide a <code>dpop_jkt</code> authorization request parameter, or provide a DPoP proof header. The reference implementation does not enforce this, allowing the DPoP proof header to be provided only during the token exchange.</p>
</li>
</ul>
<h2 class="anchor anchorWithStickyNavbar_LWe7" id="remaining-limitations">Remaining Limitations<a class="hash-link" href="https://docs.bsky.app/blog/oauth-improvements#remaining-limitations" title="Direct link to Remaining Limitations">​</a></h2>
<p>The account management interface on the reference PDS implementation allows revoking OAuth client sessions. On a free-standing PDS, this has an immediate effect: both access and refresh tokens stop working immediately. But the Bluesky-hosted PDS instances ("mushroom servers") use an "entryway" service as an OAuth Auth Server. A side-effect of this is that revoking client sessions could take up to 15 minutes to take effect: refresh tokens immediately stop working, but access tokens do not. This is a relatively common design tradeoff in auth systems involving many servers but is not currently well communicated in the interface.</p>
<p>The TypeScript OAuth Authorization server implementation (`@atproto/oauth-provider`) currently does not correctly validate that the initial client attestation signing key is present in the client metadata document over the full lifetime of an OAuth session. Instead, it simply validates each attestation (eg, on token refreshes) against the current JWKs in the client metadata. In other words, if a client removes a public key from the JWK set in the client metadata document, the auth server is supposed to reject future token refresh requests for sessions that started by using that specific key; but the current implementation does not do this. Similarly, our OAuth client implementation did not enforce the use of the same key whenever refreshing sessions. The tracking issue for this is <a href="https://github.com/bluesky-social/atproto/pull/3847" rel="noopener noreferrer" target="_blank">https://github.com/bluesky-social/atproto/pull/3847</a>. Note that fixing this bug might result in sessions becoming invalid if clients do not adapt their implementation to use the same key over the session’s lifetime. Clients only using a single key should not be impacted.</p>
