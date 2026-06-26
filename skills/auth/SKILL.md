---
name: auth
description: Implement authentication and authorization safely — verify identity, manage sessions and tokens, store credentials securely, and enforce least-privilege access checks
---

# Auth

Add or harden authentication (who is this?) and authorization (what are they allowed to do?) using the project's existing framework and session/token infrastructure, without rolling your own crypto.

## Authentication

1. **Never store passwords reversibly** — hash with a slow, salted algorithm (bcrypt/scrypt/argon2id) at a sane work factor; never plain text, MD5, or SHA-256 alone. Use the library, don't hand-roll it
2. **Use constant-time comparison** for secrets, tokens, and signatures so attackers can't time their way in
3. **Manage sessions safely** — issue a fresh session/token on login, **regenerate the id on privilege change** (login, role switch) to prevent fixation, and set cookies `HttpOnly`, `Secure`, and `SameSite`
4. **Expire and revoke** — give sessions/tokens a bounded lifetime, support logout and server-side revocation, and rotate refresh tokens. A stolen token must not be valid forever
5. **Verify tokens fully** — for JWTs, check signature, `exp`, issuer, and audience, and pin the algorithm (reject `alg: none` and algorithm-confusion). Don't trust unverified claims
6. **Rate-limit and lock out** auth endpoints to blunt credential stuffing and brute force; pair with the `rate-limiting` skill

## Authorization

1. **Check on every request, server-side** — enforce access at the point of action, never by hiding UI. The client is untrusted
2. **Deny by default** — start from no access and grant explicitly; an unmatched route or role should fail closed
3. **Enforce least privilege** — grant the narrowest scope/role that works; separate user from admin paths
4. **Check ownership, not just authentication** — confirm *this* user may act on *this* resource, keyed by the server-side identity. Looking up the id from the request body/param without an ownership check is the IDOR / broken-object-level-authorization bug
5. **Centralize the policy** — put access rules in one middleware/guard/policy layer so they're consistent and auditable, not scattered per-handler

## Rules

- **Hash passwords with bcrypt/scrypt/argon2id** — never store them reversibly, and never invent your own scheme
- **Compare secrets in constant time**; use vetted libraries for all crypto
- **Authorize on the server for every action**, deny by default, and verify resource ownership — most real breaches are broken access control, not broken crypto
- **Derive identity from the verified session/token**, never from a client-supplied user id, role, or `is_admin` field
- **Set cookies `HttpOnly` + `Secure` + `SameSite`** and regenerate the session id on login to prevent fixation
- **Verify JWTs fully** (signature, expiry, issuer, audience, pinned algorithm) and keep lifetimes short with revocation/rotation
- **Never log credentials, tokens, or session ids**; keep secrets out of code and pair with `secrets-scan`
- **Return generic auth failures** ("invalid credentials") — don't reveal whether the username or the password was wrong
- **Rate-limit authentication endpoints** to resist brute force and credential stuffing; pair with `rate-limiting`
- Make auth events **observable** — log logins, failures, and access denials (without secrets) for audit; pair with `observability`
