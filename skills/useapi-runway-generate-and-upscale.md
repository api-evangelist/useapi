---
name: Generate and upscale a Runway video via useapi.net
description: >-
  Link a Runway account, upload assets, generate with Gen-4.5 / Gen-4 / Gen-4 Turbo, track the task,
  and upscale or lip-sync the result through the useapi.net Runway API.
api: openapi/useapi-runwayml-v1-openapi.yml
base_url: https://api.useapi.net/v1/runwayml
operations:
  - postAccountsByEmail
  - getAccounts
  - getFeatures
  - postAssets
  - getAssets
  - postGen45Create
  - postGen4Create
  - postGen4turboCreate
  - postVideosCreate
  - getTasksByTaskid
  - getTasks
  - postGen4Upscale
  - postVideosUpscale
  - postLipsyncCreate
  - getLipsyncVoices
generated: '2026-07-27'
method: generated
---

# Generate and upscale a Runway video via useapi.net

## Before you start

This wraps runwayml.com through **your** Runway account. The vendor's own Q&A is blunt that using
automation is against Runway's terms of service and recommends keeping dedicated stand-by accounts,
so treat account health as a first-class operational concern, not an edge case.

Auth header on every call: `Authorization: Bearer user:<number>-<unique-string>` (complete token,
not URL-encoded).

## Steps

1. **Link the Runway account** — `postAccountsByEmail` (`POST /accounts/{email}`); verify with
   `getAccounts` (`GET /accounts`).
2. **Check what the account can actually do** — `getFeatures` (`GET /features/`) reports the
   capabilities available on the linked plan. The Unlimited plan is what unlocks credit-free
   `exploreMode` on images and most video models; a free account will not have it.
3. **Upload source media** — `postAssets` (`POST /assets/`). This endpoint takes a **raw body** with
   the file's own `Content-Type` (not multipart), with the name in the query string. List with
   `getAssets` (`GET /assets/`).
4. **Generate.** Choose the model-specific entry point rather than a generic one:
   - `postGen45Create` (`POST /gen4_5/create`) — Gen-4.5
   - `postGen4Create` (`POST /gen4/create`) — Gen-4
   - `postGen4turboCreate` (`POST /gen4turbo/create`) — Gen-4 Turbo
   - `postVideosCreate` (`POST /videos/create`) — the general video entry point, including
     third-party models routed through Runway
   - `postGen4ActTwo` (`POST /gen4/act-two`) — character animation
   Set `replyUrl` + `replyRef` for callbacks.
5. **Track the task.** Runway's identifier here is **`taskId`**, not `jobid`. Poll
   `getTasksByTaskid` (`GET /tasks/{taskId}`), or list with `getTasks` (`GET /tasks/`). Cancel a
   queued task with `deleteSchedulerByTaskid` (`DELETE /scheduler/{taskId}`).
6. **Post-process** — `postGen4Upscale` (`POST /gen4/upscale`) or `postVideosUpscale`
   (`POST /videos/upscale`) for resolution; `postLipsyncCreate` (`POST /lipsync/create`) with a voice
   from `getLipsyncVoices` (`GET /lipsync/voices/`) for dialogue.

## Rules an agent must not get wrong

- **No idempotency key.** A retried create is a new task and new credits. Reconcile with
  `getTasks` first.
- **Pace yourself deliberately.** The vendor advises simulating real users: avoid 24/7 generation,
  rotate between accounts on a schedule, prefer `replyUrl` webhooks over tight polling. This is an
  account-preservation measure, not a rate limit.
- **Results expire after 7 days** (`410 Gone`). Pull the media down promptly.
- **`412`** = the Runway account is out of credits. **`402`** = the useapi.net subscription lapsed.
  **`596`** = the linked Runway session is broken, re-add the account.
- Honor `Retry-After` on every `429`; when `email` is omitted the load balancer will already have
  tried to route around limited accounts, so a `429` that still reaches you means every linked
  account is limited.
