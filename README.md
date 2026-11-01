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

useapi.net is a third-party, unified REST API platform, in production since 2023, that fronts a set of consumer AI content-generation services — video, image, music, speech and face-swap models — behind one flat $15/month subscription and a single bearer-token API surface, so developers do not have to hold a separate developer account with each underlying provider. Thirteen services are exposed today (Google Flow, Flow Music, Google Vids, Gemini Notebook (NotebookLM), Meta Muse, Dreamina, Kling, MiniMax/Hailuo, Runway, PixVerse, Mureka, TemPolor and InsightFaceSwap), each as its own versioned REST API under api.useapi.net with its own native request/response shapes, and seven of them (Google Flow, Google Vids, Muse, Gemini Notebook, Kling, Mureka, Flow Music) also as hosted MCP servers (Streamable HTTP, Bearer token) listed in the official MCP Registry under net.useapi/*. useapi.net is an independent third-party service, not affiliated with Google, Meta or the other model vendors. The platform is built around bring-your-own-account access — the caller links one or more of their existing accounts on the underlying AI service and useapi.net performs automated multi-account load balancing, quarantine of rate-limited accounts, asynchronous job handling and replyUrl webhook callbacks on job completion or failure. It is positioned as substantially cheaper than the official first-party APIs of the services it wraps.

**Areas:** Company · Artificial Intelligence · Generative AI · Video Generation · Image Generation · Music Generation · Text-to-Speech · Face Swap · API Aggregator · Machine Learning · Media · Webhook

## useapi.net on APIs.io

Everything profiled for useapi.net is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/useapi/](https://apis.io/providers/useapi/)** — or go straight to one of the APIs or properties below.

### APIs (13)

- [useapi.net Account Management API v2](https://apis.io/apis/useapi/account-v2/) — Cross-cutting account API for the useapi.net subscription itself — retrieve account details and the configured service accounts, set the default replyUrl…
- [useapi.net Account API](https://apis.io/apis/useapi/useapi-account-api/) — The Account API from useapi.net — 4 operation(s) for account.
- [useapi.net Dreamina API](https://apis.io/apis/useapi/useapi-dreamina-api/) — Dreamina API v1 by useapi.net
- [useapi.net Flowmusic API](https://apis.io/apis/useapi/useapi-flowmusic-api/) — FlowMusic API v1 by useapi.net
- [useapi.net Google Flow API](https://apis.io/apis/useapi/useapi-google-flow-api/) — Google Flow API v1 by useapi.net
- [useapi.net Jobs API](https://apis.io/apis/useapi/useapi-jobs-api/) — The Jobs API from useapi.net — 12 operation(s) for jobs.
- [useapi.net Kling API](https://apis.io/apis/useapi/useapi-kling-api/) — Kling API v1 by useapi.net
- [useapi.net Mureka API](https://apis.io/apis/useapi/useapi-mureka-api/) — Mureka API v1 by useapi.net
- [useapi.net Pixverse API](https://apis.io/apis/useapi/useapi-pixverse-api/) — PixVerse API v2 (web) by useapi.net
- [useapi.net Runwayml API](https://apis.io/apis/useapi/useapi-runwayml-api/) — Runway API v1 by useapi.net
- [useapi.net Tempolor API](https://apis.io/apis/useapi/useapi-tempolor-api/) — TemPolor API v1 by useapi.net
- [useapi.net Face Swap API](https://apis.io/apis/useapi/useapi-face-swap-api/) — InsightFaceSwap API v1 by useapi.net
- [useapi.net Mini Max API](https://apis.io/apis/useapi/useapi-mini-max-api/) — MiniMax API v1 by useapi.net

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Useapi Jobs Webhooks](https://apis.io/asyncapis/useapi/useapi-jobs-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [useapi](https://apis.io/servers/useapi/useapi/)

#### Collections (12)

Postman and Open Collection request collections.

- [Dreamina API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-dreamina-v1/)
- [InsightFaceSwap API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-faceswap-v1/)
- [FlowMusic API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-flowmusic-v1/)
- [Google Flow API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-google-flow-v1/)
- [Kling API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-kling-v1/)
- [Midjourney REST API by useapi.net](https://apis.io/collections/useapi/open-useapi-midjourney-v1/)
- [Midjourney REST API v2 by useapi.net](https://apis.io/collections/useapi/open-useapi-midjourney-v2/)
- [MiniMax API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-minimax-v1/)
- [Mureka API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-mureka-v1/)
- [PixVerse API v2 (web) by useapi.net](https://apis.io/collections/useapi/open-useapi-pixverse-v2/)
- [Runway API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-runwayml-v1/)
- [TemPolor API v1 by useapi.net](https://apis.io/collections/useapi/open-useapi-tempolor-v1/)

#### Overlays (10)

OpenAPI overlays, including conversational phrasing for each operation.

- [Useapi Dreamina V1 Overlay](https://apis.io/overlays/useapi/useapi-dreamina-v1-overlay/)
- [Useapi Faceswap V1 Overlay](https://apis.io/overlays/useapi/useapi-faceswap-v1-overlay/)
- [Useapi Flowmusic V1 Overlay](https://apis.io/overlays/useapi/useapi-flowmusic-v1-overlay/)
- [Useapi Google Flow V1 Overlay](https://apis.io/overlays/useapi/useapi-google-flow-v1-overlay/)
- [Useapi Kling V1 Overlay](https://apis.io/overlays/useapi/useapi-kling-v1-overlay/)
- [Useapi Minimax V1 Overlay](https://apis.io/overlays/useapi/useapi-minimax-v1-overlay/)
- [Useapi Mureka V1 Overlay](https://apis.io/overlays/useapi/useapi-mureka-v1-overlay/)
- [Useapi Pixverse V2 Overlay](https://apis.io/overlays/useapi/useapi-pixverse-v2-overlay/)
- [Useapi Runwayml V1 Overlay](https://apis.io/overlays/useapi/useapi-runwayml-v1-overlay/)
- [Useapi Tempolor V1 Overlay](https://apis.io/overlays/useapi/useapi-tempolor-v1-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Useapi Authentication](https://apis.io/security/useapi/useapi-authentication/)
- [Useapi Domain Security](https://apis.io/security/useapi/useapi-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Useapi Plans Pricing](https://apis.io/plans/useapi/useapi-plans-pricing/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://useapi.net/)
- [Documentation](https://useapi.net/docs/subscription)
- [Getting started](https://useapi.net/docs/start-here/setup-useapi)
- [API reference](https://useapi.net/docs/api-google-flow-v1)
- [Pricing](https://useapi.net/docs/subscription)
- [Sign up](https://buy.stripe.com/8x2aEX4Bd8Vh9PMg4qeUU03)
- [Support](https://useapi.net/docs/support)
- [Blog](https://useapi.net/blog)
- [Terms of service](https://useapi.net/docs/legal)
- [Privacy policy](https://useapi.net/docs/legal)
- [GitHub](https://github.com/useapi)

## API Evangelist rating

**Kin Score 45.9** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 21.8 |
| Contract governance | 4.5 |
| Operational transparency | 34.2 |
| Developer ergonomics | 75.6 |
| Access clarity | 65.8 |

**Agent readiness 36.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | yes |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | — |

**Regulatory alignment 21.9** — AI & Biometrics

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/useapi/](https://apis.io/providers/useapi/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for useapi.net: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/useapi/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
