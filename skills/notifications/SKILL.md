---
name: notifications
description: Send transactional notifications reliably — pick channels, template safely, send async with retries, respect user preferences and opt-out, and avoid duplicates
---

# Notifications

Deliver transactional notifications (email, SMS, push, in-app) reliably and respectfully, using the project's existing provider (e.g. SES/SendGrid, Twilio, FCM/APNs) and job infrastructure, so users get the right message once — not zero times, not five.

## Delivery

1. **Send asynchronously via a job/queue** — never block a request on a third-party send; enqueue it and process in the background. Pair with the `background-jobs` skill
2. **Retry transient failures with capped backoff; don't retry permanent ones** — a 5xx/timeout is retryable, a hard bounce or invalid number is not. Give up and record after the cap
3. **Dedupe so a user isn't notified twice** — key each notification by (user, event, channel) and check before sending; at-least-once job delivery means a naive send fires duplicates. Pair with the `idempotency` skill
4. **Handle provider webhooks** — process delivery/bounce/complaint callbacks to update status and suppress bad addresses. Pair with the `webhooks` skill

## Content & channels

1. **Use templates, and escape by channel** — separate content from code; HTML-escape email, avoid injection in SMS. Never build messages by naive string concatenation of user data
2. **Localize and format for the recipient** — respect locale, timezone, and formatting; pair with the `i18n` skill
3. **Pick the channel by urgency and consent** — transactional (receipts, password resets) vs. marketing have different rules; don't send marketing without opt-in

## Respect & safety

1. **Honor user preferences and opt-out** — check per-channel, per-category preferences before sending, and make unsubscribe work (and legally required for bulk email). Suppress addresses that bounce or complain
2. **Rate-limit and batch** — cap how often a user can be notified, and digest/coalesce bursts so one noisy event doesn't spam them. Pair with the `rate-limiting` skill
3. **Never put secrets or sensitive data in messages** — no passwords, tokens, or full PII in the body; send a link to authenticate instead. Pair with `secrets-scan`
4. **Make delivery observable** — log sends, provider ids, status, and failures; alert on elevated bounce/failure rates. Pair with `observability`

## Rules

- **Send async through a queue with capped-backoff retries** — never block the request; don't retry permanent failures. Pair with `background-jobs`
- **Dedupe on (user, event, channel)** so retries and duplicate events don't double-send; pair with `idempotency`
- **Check user preferences and honor opt-out/unsubscribe** before every send; suppress bounced/complained addresses
- **Template content and escape per channel**; never string-concat user data into messages
- **Rate-limit and digest** notifications so users aren't spammed; pair with `rate-limiting`
- **Never include secrets or sensitive data** in a notification body — link to an authenticated action instead; pair with `secrets-scan`
- **Process provider delivery/bounce webhooks** to track status and clean your list; pair with `webhooks`
- **Localize** to the recipient's locale and timezone; pair with `i18n`
- Make sends **observable** and **alert on bounce/failure spikes**; pair with `observability`
- **Test failure paths** — provider errors, bounces, opted-out users, and duplicate events; pair with `test-gen`
