---
name: Swap and enhance faces with InsightFaceSwap via useapi.net
description: >-
  Register a Picsi.Ai / InsightFaceSwap Discord channel, save a source face as a reusable identity,
  swap it onto target images, and run background change or headshot generation through the
  useapi.net InsightFaceSwap API.
api: openapi/useapi-faceswap-v1-openapi.yml
base_url: https://api.useapi.net/v1/faceswap
operations:
  - postAccountByChannelid
  - getAccount
  - postSaveid
  - postListid
  - postSetid
  - postSwapid
  - postSwap
  - postInswapper
  - postPicsi
  - postChangebg
  - postHeadshot
  - getJobs
  - getJobsCancel
  - deleteDelid
generated: '2026-07-27'
method: generated
---

# Face swap with InsightFaceSwap via useapi.net

## Before you start

This API drives the **InsightFaceSwap Discord bot** by Picsi.Ai, so the unit of configuration is a
Discord channel, not an email. You supply a Discord token, server id and channel id. Free and paid
Picsi.Ai tiers both work.

Auth header on every call: `Authorization: Bearer user:<number>-<unique-string>` (complete token).

## Ethical and legal gate — check this first

Face swapping produces synthetic likenesses of real people. Before running any of these operations,
confirm you have the subject's consent for the source face and rights to the target image. Do not
use this skill to produce material depicting a real person in a context they have not agreed to.
useapi.net performs no such check for you.

## Steps

1. **Register the channel** — `postAccountByChannelid` (`POST /account/{channel_id}`) with the
   Discord token and server id. Confirm with `getAccount` (`GET /account`) or
   `getAccountByChannelid` (`GET /account/{channel_id}`).
2. **Save a source face as a named identity** — `postSaveid` (`POST /saveid`) stores a face under a
   name you choose. `postListid` (`POST /listid`) lists what you have saved, and `postSetid`
   (`POST /setid`) selects the active identity.
3. **Swap.** Pick the operation that matches the job:
   - `postSwapid` (`POST /swapid`) — swap a *saved* identity onto a target image.
   - `postSwap` (`POST /swap`) — swap directly from a supplied source image.
   - `postInswapper` (`POST /inswapper`) — the inswapper pipeline.
   - `postPicsi` (`POST /picsi`) — Picsi.Ai HiFidelity / ARTIFY effects, age transformation and
     multi-face morphing.
4. **Other transforms** — `postChangebg` (`POST /changebg`) replaces the background;
   `postHeadshot` (`POST /headshot`) generates a headshot.
5. **Collect the result** — `getJobs` (`GET /jobs/?jobid=<jobid>`) returns a single job;
   `getJobs2` (`GET /jobs`) lists them. Cancel a running job with `getJobsCancel`
   (`GET /jobs/cancel/?jobid=<jobid>`). Or set `replyUrl` on the request and take the callback.
6. **Clean up saved identities** — `deleteDelid` (`DELETE /delid`) removes one,
   `deleteDelall` (`DELETE /delall`) removes all. Delete stored faces you no longer need rather than
   leaving biometric-adjacent data sitting in a third-party bot.

## Rules an agent must not get wrong

- **No idempotency key** — a retried swap is a second job.
- **7-day retention** on results (`410 Gone` afterwards).
- **`596`** means the linked Discord/Picsi session is broken; re-register the channel.
- `422` is a moderation rejection from upstream, not a transport failure — do not retry it
  unchanged.
- Honor `Retry-After` on `429`.
