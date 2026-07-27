---
name: Generate a video with Google Flow (Veo) via useapi.net
description: >-
  Link a Google account, submit a Veo 3.1 text-to-video or image-to-video generation to the
  useapi.net Google Flow API, and collect the result by polling or by replyUrl webhook — including
  correct handling of the four distinct Google 429 quota reasons.
api: openapi/useapi-google-flow-v1-openapi.yml
base_url: https://api.useapi.net/v1/google-flow
operations:
  - postAccounts
  - getAccounts
  - postAssetsByEmail
  - postVideos
  - getJobsByJobid
  - getJobs
  - postVideosExtend
generated: '2026-07-27'
method: generated
---

# Generate a video with Google Flow via useapi.net

## Before you start

useapi.net does **not** host Veo. It drives a Google account you own. You need both an active
useapi.net subscription and at least one Google account with Flow access. Costs land on the Google
account, not on useapi.net.

## Authentication

Every call carries the same header:

```
Authorization: Bearer user:<number>-<unique-string>
```

Send the **complete** token — keep the `user:` prefix and the alphanumeric suffix, do not truncate
to the number, do not URL-encode. One token authorizes every useapi.net API on the subscription.
There are no scopes and no OAuth.

## Steps

1. **Link at least one Google account** — `postAccounts` (`POST /accounts`). The body carries
   cookies copied from Google in the format described at
   https://useapi.net/docs/start-here/setup-google-flow. The email is extracted from the cookies.
   Link **two or more** accounts if you intend to generate at volume: the load balancer can only
   route around a quota-limited account when there is another one to route to, and the quarantine
   pre-filter is skipped entirely when exactly one account is configured.
2. **Confirm the link** — `getAccounts` (`GET /accounts`) returns each configured email with its
   health status. Do this before your first generation; a bad cookie surfaces here rather than
   halfway through a batch.
3. **Upload reference images if you need them** — `postAssetsByEmail`
   (`POST /assets/{email}`) accepts PNG or JPEG. Note this endpoint is account-scoped: the asset
   belongs to the account you upload it to.
4. **Submit the generation** — `postVideos` (`POST /videos`). Include your prompt and model. Two
   choices matter here:
   - **Omit `email`** to let the load balancer pick the healthiest account and route around
     quarantined ones. Pass `email` only when you deliberately want a specific account.
   - Set **`replyUrl`** to receive a callback instead of polling, and **`replyRef`** to carry your
     own correlation id through to that callback. The vendor explicitly recommends webhooks over
     polling — it is both cheaper and less likely to look like automation to Google.
5. **Collect the result** — either wait for the callback, or poll `getJobsByJobid`
   (`GET /jobs/{jobId}`) with the returned job id. Do not poll tightly; the documentation asks for
   roughly once-a-minute pacing.
6. **Optionally continue the shot** — `postVideosExtend` (`POST /videos/extend`) extends an existing
   video with a new prompt.

## Rules an agent must not get wrong

- **There is no idempotency key.** A retried `postVideos` creates a *second* job and burns credits
  on the Google account. Never blind-retry a generation. If a call times out, find the job with
  `getJobs` before resubmitting.
- **Download within 7 days.** Jobs expire; `GET /jobs/{jobId}` then returns `410 Gone` with
  `{"error": "Job has expired", "code": 410}`.
- **Handle each 429 reason differently.** Every 429 carries a `Retry-After` header and a
  `retryAfter` ISO-8601 body field — honor it. Then branch on the reason:
  | reason | what it means | what to do |
  |---|---|---|
  | `PUBLIC_ERROR_UNUSUAL_ACTIVITY_TOO_MUCH_TRAFFIC` | captcha token scored too low | configure a second captcha provider (`postAccountsCaptchaProviders`), retry in ~60s |
  | `PUBLIC_ERROR_USER_REQUESTS_THROTTLED` | per-account concurrency cap | reduce parallelism; the account is quarantined ~30 min |
  | `PUBLIC_ERROR_PER_MODEL_DAILY_QUOTA_REACHED` | that model is capped on that account | switch model; resets at UTC midnight |
  | `PUBLIC_ERROR_USER_QUOTA_REACHED` | whole account capped | link more accounts; quarantined ~30 min |
  | `no_eligible_account` | every account is quarantined | wait for the earliest cooldown in `Retry-After` |
- **Prompts are not pre-screened.** useapi.net forwards Google's moderation verdict as a `400` with
  a `PUBLIC_ERROR_*` reason, or a `422`. Screen prompts yourself before spending a generation.
- **`596` is a real status code here.** It is non-standard and means the linked Google account's
  session is broken. Delete the account (`DELETE /accounts/{email}`) and re-add it; do not retry.
- **Model aliases have shifted.** `nano-banana` now maps to `nano-banana-2` and `imagen-4` maps to
  `nano-banana-2-lite` (Google removed Imagen from Flow in July 2026). Check
  https://useapi.net/model-matrix before hardcoding a model name.

## Where to check state

`getJobs` (`GET /jobs/`) returns per-account executing / completed / failed / rateLimited counts over
the last 15 minutes plus current quarantine entries. There is no public status page — this endpoint
and `GET /v2/account/stats` are the operational visibility surface.
