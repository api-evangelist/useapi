---
name: Operate useapi.net — link accounts, set webhooks, watch usage
description: >-
  The cross-cutting operating skill for useapi.net: authenticate, link and health-check the
  bring-your-own upstream accounts every generation API depends on, configure the account-wide
  replyUrl webhook, and audit usage and failures with the account statistics API.
api: openapi/useapi-google-flow-v1-openapi.yml
base_url: https://api.useapi.net
operations:
  - getAccounts
  - postAccounts
  - deleteAccountsByEmail
  - getJobs
generated: '2026-07-27'
method: generated
---

# Operating useapi.net

This skill covers what is true across **all ten** useapi.net APIs. Everything else is per-service —
the vendor states outright that identifier names, job lifecycle, response shapes, webhook payload
shape, status-code semantics and sync-vs-async behavior all vary by API.

## 1. Authenticate

```
Authorization: Bearer user:<number>-<unique-string>
```

- Use the complete token: keep the `user:` prefix **and** the alphanumeric suffix.
- Do not truncate to the number. Do not URL-encode.
- One token authorizes every API on the subscription. There is no OAuth, no scopes, no per-API key,
  and no key rotation endpoint.
- The token arrives by email on subscription: https://useapi.net/docs/start-here/setup-useapi

## 2. Understand the access model before writing any code

useapi.net is a fronting layer, not a model host. Every generation API requires you to register
**your own** account on the wrapped service (Google, Runway, Kling, Mureka, …) via that service's
`POST /accounts` endpoint, supplying browser cookies or a session token. An active subscription on
the underlying service is required in addition to the $15/month useapi.net subscription, and
generation costs land on the upstream account.

Link **more than one** account per service if you generate at volume. useapi.net picks an account
automatically whenever the per-request account selector is omitted, using a weighted health score
over the last 15 minutes (executing / completed / failed / rateLimited), and quarantines accounts
that return upstream 429s. With exactly one account configured the quarantine pre-filter is skipped
entirely — there is nothing to fail over to.

Note the account selector is named differently per service: `email` (google-flow, flowmusic),
`account` (dreamina, kling, minimax, mureka), `channel_id` (faceswap).

## 3. Set the account-wide webhook once

```
POST https://api.useapi.net/v2/account
{"replyUrl": "https://your-domain.example/useapi-callback"}
```

This becomes the default for **every** API call on the subscription; any individual request can
override it with its own `replyUrl`. useapi.net POSTs to it once a job completes or fails. Pass
`replyRef` on a request to carry your own correlation value through to the callback.

Read the current setting back with `GET https://api.useapi.net/v2/account`. An over-long value is
rejected with `{"error": "replyUrl is too long"}`.

**Treat the callback as untrusted.** No signature header, shared secret or verification scheme is
documented. Use the callback as a *trigger* to re-read authoritative job state from the service's own
job endpoint with your bearer token — do not act on the posted body alone.

## 4. Watch usage and failures

```
GET https://api.useapi.net/v2/account/stats?bot=<service>[&date=YYYY-MM-DD][&limit=N][&config=<account>]
```

Returns per-bot request counts broken down by config, endpoint, model, HTTP status, status text and
plan tier, with average latency and a success rate. `limit` maxes at 50000 and, when supplied,
overrides the `date` filter. Data lags 5–15 minutes and is retained for **3 months**.

Per-service, `GET /v1/<service>/jobs` gives near-real-time health: executing / completed / failed /
rateLimited counts for the last 15 minutes plus current quarantine entries.

There is **no public status page** — `status.useapi.net` merely redirects to the marketing homepage.
These two endpoints are the entire operational-visibility surface.

## 5. Non-negotiable rules

- **No idempotency anywhere.** No endpoint accepts an idempotency key. A retried generation POST
  creates a second job and spends upstream credits. Always reconcile against the job/task list
  before resubmitting.
- **7-day result retention.** Expired jobs return `410 Gone`. Download media promptly.
- **No prompt pre-validation.** Moderation verdicts come back as `400` (with an upstream reason
  code) or `422`. Screen prompts caller-side; the vendor offers a free `POST /v1/minimax/llm`
  MiniMax-Text-01 endpoint for exactly this.
- **Distinguish the money errors.** `402` = useapi.net subscription expired. `412` = the *upstream*
  account is out of credits. Different owners, different fixes.
- **`596` is a real, non-standard status** meaning the linked upstream account's session is broken.
  Delete and re-add that account; retrying will never clear it.
- **Always honor `Retry-After`** and the `retryAfter` ISO-8601 body field on `429`.
- **Never assume a convention carries across services.** Confirm identifier names, paging style
  (limit/offset vs cursor vs last_id vs page/pageSize all appear) and status semantics against the
  specific service's own documentation.

## Reference artifacts

- Conventions: `conventions/useapi-conventions.yml`
- Errors: `errors/useapi-problem-types.yml`
- Webhooks: `asyncapi/useapi-jobs-webhooks.yml`
- Lifecycle and retirements: `lifecycle/useapi-lifecycle.yml`
- Data model: `data-model/useapi-data-model.yml`
- Which model lives behind which API: https://useapi.net/model-matrix
