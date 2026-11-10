# The 20 required checks

Use the original numbering in the report. For every applicable item, inspect the enforcing code/configuration and exercise a bounded negative case plus a legitimate case where feasible. Mark unavailable deployment evidence as unverified, even if repository settings look correct.

## Contents

1. Next.js header
2. .git folder
3. Supabase storage
4. Price from the browser
5. Credential stuffing
6. Open database
7. Polyfill.io
8. SMS pumping
9. Prompt injection
10. Weak JWT secret
11. Race condition
12. Account pre-hijacking
13. Subdomain takeover
14. SSRF
15. Path traversal
16. Reset poisoning
17. Sourcemaps
18. Clickjacking
19. Open redirect
20. User enumeration

## 1. Next.js header

Inspect framework disclosure headers and actual responses, including edge/platform-added headers. Treat X-Powered-By as fingerprinting, usually informational by itself. Separately inspect the exact Next.js version and current official advisories, middleware/proxy authorization and configured image/cache surfaces; hiding a header does not patch vulnerabilities. Preserve framework functionality and legitimate assets.

## 2. .git folder

Inspect deployment roots, static copies, archive/backup artifacts and ignore rules. If live checks are authorized, make a small request for a known metadata path such as /.git/HEAD and inspect content/type as well as status: a generic 200 fallback page is not exposed Git. Do not recursively download an exposed repository or secrets to prove the point. Scope “not exposed” to the tested paths and deployment.

## 3. Supabase storage

Inventory actual buckets and object policies through the relevant connector, distinguishing browser localStorage from Supabase Storage. Check public flags, SELECT/INSERT/UPDATE/DELETE policies, path/owner scoping, signed URL lifetime and server-only service credentials. Test anonymous, user A and user B access with disposable objects where authorized. A public bucket intended for public assets and a public anon/publishable key are not vulnerabilities by themselves. Never tighten public access before checking actual sharing/download requirements.

## 4. Price from the browser

Trace checkout/order/mint inputs to the final charged or transferred amount. Verify trusted server/contract lookup or computation of price, currency, quantity, discounts, tax and entitlement; check webhook authenticity and idempotency. Mutate client values against fixtures without charging money. Do not flag a UI price display when the authoritative operation already verifies it. Mark absent commerce/financial actions not applicable; assess browser-controlled analytics separately.

## 5. Credential stuffing

Inspect login/reset/MFA/session endpoints, password handling, authentication error behavior and trusted client-IP derivation. Evaluate shared rate limits across instances/restarts, expiry, outages and legitimate shared-IP traffic; distinguish stuffing from single-account guessing. Check whether hosting actually overwrites forwarded headers before declaring them spoofable. Use synthetic requests/local counters, never stolen credential lists or production password spraying. Preserve normal login and avoid attacker-triggered global/account lockout.

## 6. Open database

Inspect actual exposed APIs/network paths, credentials, schema grants, RLS/policies, table ownership, SECURITY DEFINER functions/search paths, connection role and TLS verification. Test anonymous and cross-user authorization without extracting real rows. A migration enabling RLS is not proof of deployed enforcement; a service role or table owner may bypass it. Conversely, a publicly reachable authenticated endpoint is not an open database. Verify both encryption and certificate/hostname checks; provider/driver SSL mode names alone can be misleading. Inspect only in-scope schemas on shared databases.

## 7. Polyfill.io

Search source, generated pages and relevant deployed script requests for Polyfill.io and redirects/aliases that load it. Check exact third-party script origins and current official security information before recommending a replacement. Do not confuse locally served Next.js polyfill bundles with the external service. Remove or replace a reachable unsafe dependency only after checking required browser support and running the affected pages.

## 8. SMS pumping

Identify any SMS/voice OTP send path and who can invoke it. Inspect per-phone, per-IP/device and aggregate abuse controls, resend intervals, verification requirements, provider spend caps and intended destination coverage. Use a mocked provider; do not trigger billable sends. Avoid blanket country blocks or new verification friction without a product basis. A server-side API key alone does not prevent abuse of a public sending endpoint. Mark absent SMS/voice integrations not applicable.

## 9. Prompt injection

Inventory LLM inputs, retrieved pages/documents, tool calls, data access and consequential actions. Trace whether untrusted content can override instructions, obtain secrets, cross tenants or authorize a tool action. Test inert adversarial content with mocked tools and fake secrets. Require authorization and data boundaries outside model output; prompt wording alone is not a security boundary. Distinguish a static copied prompt or plain text renderer from an in-app model/agent execution surface.

## 10. Weak JWT secret

Inspect signing algorithms, key source, generation/entropy requirements, client exposure, accepted algorithms, expiry and application-required issuer/audience/type claims. Test tampering, wrong token purpose, expired tokens and key/credential rotation using fixture keys. Verify server and proxy/edge verifiers agree. Use algorithm-appropriate key strength; a character count does not prove entropy, and an asymmetric public key is not a secret. Report actual deployed key strength as unknown unless authorized evidence establishes it. Check that admin credential rotation can invalidate admin access without unnecessarily invalidating unrelated visitors.

## 11. Race condition

Identify read-check-write operations affecting money, inventory, coupons, quota, authorization, one-time tokens, session recovery and analytics. Inspect transactions, constraints, conditional updates, locking and idempotency at the authoritative store. Use bounded concurrent requests against isolated data; assert the invariant, not just successful responses. In-process locks do not coordinate multiple workers. Separate replay, lost updates and isolation failures. Do not claim multi-backend race safety from a single-connection emulator or a mocked database; preserve normal retries and partial failures.

## 12. Account pre-hijacking

Inspect registration, email verification, account linking/merging, OAuth, password reset and session invalidation. With synthetic identities, examine an attacker-created unverified account followed by the rightful mailbox owner's registration/login, including whether attacker sessions survive ownership verification or credential recovery. Require verified ownership for identity/account linking. An intentional unverified-email analytics gate is not a traditional account; state identity-attribution limits without silently converting it to verified membership.

## 13. Subdomain takeover

Inspect in-scope DNS records and the corresponding hosting/service bindings using authorized metadata. Correlate dangling aliases, removed resources and provider-specific claimability requirements; a 404 alone does not prove takeover. Do not claim/register a target resource as a test. A working primary hostname does not clear sibling or stale records. If DNS/binding access is absent, mark this unverified. Do not remove a record without checking current ownership and legitimate use.

## 14. SSRF

Find server-side fetch/proxy, image, webhook-test, import, URL-preview and document-rendering paths. Trace URL parsing, allowed protocols/destinations/ports, redirects, DNS resolution/rebinding, IPv4/IPv6/private/loopback/link-local destinations and network egress controls. Test only local mock destinations; do not probe cloud metadata or internal services in production. Check each redirect/resolution boundary, not only the original URL string. Distinguish browser navigation and static links from server-side requests, and inspect framework URL-fetch facilities even without an obvious application fetch call.

## 15. Path traversal

Inspect upload/download/archive/static handlers and path construction after actual framework decoding. Test traversal, encoded separators, absolute paths, NULs, symlinks and allowlist boundaries with disposable files; account for archive extraction and platform path rules where relevant. Check normalized/real path containment and authorization on the resolved resource. Preserve exact public-file exceptions, legitimate filenames and downloads. Do not read real system files or secrets as proof.

## 16. Reset poisoning

Identify password-recovery links and trusted origin construction. Inspect Host/X-Forwarded-Host trust, request-origin reuse, URL allowlists, reset-token binding/expiry/single use and redirects after reset. Use a mocked mailer to verify attacker-supplied headers cannot send a usable token to an attacker origin. Do not send real reset emails in an ordinary audit. An analytics-reset endpoint is a different function: inspect its authorization and CSRF controls separately rather than labeling it password-reset poisoning.

## 17. Sourcemaps

Inspect production build settings, served JS references, accessible .map files and private error-monitoring uploads. Probe a map path derived from an actual bundle when live checks are authorized; a guessed URL or proxy-generated denial is inconclusive. Assess the exposed content and impact before assigning severity. Public frontend code is expected; maps are not automatically credential exposure. Preserve private debugging uploads and avoid disabling observability unnecessarily.

## 18. Clickjacking

Inspect effective CSP frame-ancestors and/or X-Frame-Options on relevant HTML and sensitive UI responses, including overrides/redirects. Consider all ancestors and explicitly intended cross-origin embeds. Use a harmless local framing test if appropriate. Preserve legitimate same-origin PDF frames; do not indiscriminately set DENY or an incompatible policy. Treat source configuration separately from observed deployment behavior.

## 19. Open redirect

Trace returnTo/next/callback destinations, auth flows and server/client navigation. Validate parsed destinations against intended exact origins or internal-route allowlists, accounting for scheme-relative URLs, credentials, backslashes, encoding and framework normalization. Test malicious variants alongside every legitimate destination class. Avoid fragile string-prefix checks and do not break previews, deep links or intended external OAuth/payment callbacks. Report a code-driven fixed redirect separately from user-controlled redirection.

## 20. User enumeration

Compare synthetic existing/non-existing identities across signup, login, reset and lookup for status/body/content and meaningful timing differences. Account for rate limits, cache effects and whether identity exposure is already intentional/public. Use generic unauthenticated responses where appropriate without degrading authenticated/admin feedback. Do not enumerate real people or claim timing leakage from a single sample. If no account lookup or identity-dependent response exists, explain why the classic oracle is absent.
