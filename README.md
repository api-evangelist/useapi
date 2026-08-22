# useapi.net

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

useapi.net is an experimental, unified REST API platform that fronts a set of consumer AI content-generation services — video, image, music, speech and face-swap models — behind one flat $15/month subscription and a single bearer-token API surface, so developers do not have to hold a separate developer account with each underlying provider.

- Website: https://useapi.net/
- Developer portal: https://useapi.net/docs/start-here
- API catalog: https://useapi.net/.well-known/api-catalog
- llms.txt: https://useapi.net/llms.txt
- Postman workspace: https://www.postman.com/useapinet/workspace/useapi-net
- GitHub: https://github.com/useapi

## APIs

Ten services are offered today, each as its own versioned REST API under `api.useapi.net` with its own native request and response shapes:

| API | Base URL | Contract |
|---|---|---|
| Google Flow API v1 | `https://api.useapi.net/v1/google-flow` | `openapi/useapi-google-flow-v1-openapi.yml` |
| Flow Music API v1 | `https://api.useapi.net/v1/flowmusic` | `openapi/useapi-flowmusic-v1-openapi.yml` |
| Dreamina API v1 | `https://api.useapi.net/v1/dreamina` | `openapi/useapi-dreamina-v1-openapi.yml` |
| Kling AI API v1 | `https://api.useapi.net/v1/kling` | `openapi/useapi-kling-v1-openapi.yml` |
| MiniMax / Hailuo AI API v1 | `https://api.useapi.net/v1/minimax` | `openapi/useapi-minimax-v1-openapi.yml` |
| Runway API v1 | `https://api.useapi.net/v1/runwayml` | `openapi/useapi-runwayml-v1-openapi.yml` |
| PixVerse API v2 | `https://api.useapi.net/v2/pixverse` | `openapi/useapi-pixverse-v2-openapi.yml` |
| Mureka API v1 | `https://api.useapi.net/v1/mureka` | `openapi/useapi-mureka-v1-openapi.yml` |
| TemPolor API v1 | `https://api.useapi.net/v1/tempolor` | `openapi/useapi-tempolor-v1-openapi.yml` |
| InsightFaceSwap API v1 | `https://api.useapi.net/v1/faceswap` | `openapi/useapi-faceswap-v1-openapi.yml` |

Plus a cross-cutting account API at `https://api.useapi.net/v2/account`, and the retired Midjourney API (discontinued 2026-06-24) whose vendor-published OpenAPI is archived here.

## Contract provenance

useapi.net does **not** publish OpenAPI for its ten current APIs. It publishes first-party **Postman v2.1 collections**, referenced as `service-desc` entries from an RFC 9727 `/.well-known/api-catalog` linkset. Those collections are saved verbatim under `postman/`, and the OpenAPI 3.1 documents under `openapi/` are faithful API Evangelist derivations of them — 298 operations, every path, parameter, body example and description carried over unchanged. The two `useapi-midjourney-v*-openapi.yml` files are the exception: they are vendor-published OpenAPI 3.0.2 harvested verbatim from SwaggerHub.

## Notes for integrators

- **One convention, then ten dialects.** Bearer auth (`Authorization: Bearer user:<number>-<unique-string>`) is the only platform-wide contract. The vendor states explicitly that identifier names, job lifecycle, response shapes, webhook payloads and status-code semantics all vary per API.
- **Bring your own account.** Every generation API requires the caller to register their own account on the wrapped service (browser cookies or a session token), and an active subscription there. Costs land on that upstream account.
- **No idempotency, anywhere.** A retried generation POST creates a second job and spends credits. See `conventions/useapi-conventions.yml`.
- **7-day result retention**, then `410 Gone`.
- **`596` is a real, non-standard status** meaning the linked upstream account's session is broken.
- **No status page, no SLA, no compliance program.** The service is explicitly experimental. See `conformance/useapi-conformance.yml`.

## Artifacts in this repo

`openapi/` · `postman/` · `overlays/` · `well-known/` · `llms/` · `mcp/` · `packages/` · `authentication/` · `conventions/` · `errors/` · `lifecycle/` · `changelog/` · `conformance/` · `data-model/` · `asyncapi/` · `sandbox/` · `security/` · `skills/`
