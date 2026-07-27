# useapi.net

An experimental, unified REST API platform that fronts ten consumer AI content-generation services — video, image, music, speech and face-swap — behind one flat $15/month subscription and a single bearer token. Ten APIs, 298 operations, an RFC 9727 API catalog, no OpenAPI, no idempotency, no SLA.

- **Provider:** https://useapi.net
- **Developer portal:** https://useapi.net/docs/start-here
- **API catalog:** https://useapi.net/.well-known/api-catalog (RFC 9727)
- **Postman workspace:** https://www.postman.com/useapinet/workspace/useapi-net
- **GitHub:** https://github.com/useapi (22 public repos)
- **Community:** [Discord](https://discord.gg/w28uK3cnmF) · [Telegram](https://t.me/use_api)
- **Public API:** yes — self-service, one published price, no free tier
- **Agent-native:** partial — llms.txt yes, MCP no
- **Profiled:** 2026-07-27 · **Enriched:** 2026-07-27

## What it actually is

useapi.net is not a model host. It is a **fronting layer over consumer AI products that never
shipped an API** — Google Flow, Dreamina, Kling, Hailuo, Runway, PixVerse, Mureka, TemPolor,
InsightFaceSwap — reverse-engineered into REST and sold as one subscription. The vendor is candid
about this; "experimental" is their word, on their own docs.

The economic model is **bring-your-own-account**. You register your own credentials on the wrapped
service (`POST /<service>/accounts`, supplying browser cookies or a session token), and useapi.net
drives that account on your behalf. Generation cost lands on your upstream subscription; the $15
buys the automation layer, not the compute. What that layer adds is genuinely non-trivial: weighted
multi-account load balancing scored on executing/completed/failed/rate-limited counts over a rolling
15 minutes, automatic quarantine of accounts that return upstream 429s (~30 minutes on Google
`USER_QUOTA_REACHED`, until UTC midnight on `PER_MODEL_DAILY_QUOTA_REACHED`), captcha handling, and
async job orchestration with `replyUrl` webhook callbacks.

That framing matters for how you read everything below. The governance gaps here are not
oversights by a company that should know better — they are the honest shape of a wrapper business
whose contracts are downstream of ten vendors who never agreed to be wrapped.

## The find: an RFC 9727 API catalog

The single most interesting thing in this profile is that `https://useapi.net/.well-known/api-catalog`
returns **200 `application/linkset+json`** — a real RFC 9727 API catalog over an RFC 9264 linkset,
enumerating all ten current APIs, each with a `service-desc` pointing at a first-party Postman
collection and `service-doc` entries for the HTML docs.

RFC 9727 was published in 2025 and adoption across the 25,000-provider network is close to nothing.
Finding it on a $15/month unofficial-API shop rather than a bank or a cloud vendor is the sort of
inversion that makes the catalog worth keeping. It also means the whole surface is
machine-discoverable from one URL, which is more than most providers ten times its size can say.

## Contract provenance — read this before trusting `openapi/`

**useapi.net publishes no OpenAPI for any of its ten current APIs.** What it publishes is first-party
**Postman v2.1 collections**, linked as `service-desc` from the api-catalog.

So this repo separates the two cleanly:

| Directory | Provenance |
|---|---|
| `postman/` (10 files) | **Vendor-published, saved verbatim.** The authoritative contract. |
| `openapi/useapi-*-v*.yml` (10 files) | **API Evangelist derivations** of those collections — 298 operations, paths/parameters/bodies/descriptions carried over unchanged. Not vendor artifacts. |
| `openapi/useapi-midjourney-v1/v2` | **Vendor-published OpenAPI 3.0.2**, harvested verbatim from SwaggerHub — for the API they discontinued. |

The irony is on the record: the only OpenAPI useapi.net ever published describes the one API it
no longer runs.

## Ten APIs, one convention, ten dialects

| API | Version | Ops |
|---|---|---|
| Kling AI | v1 | 45 |
| Runway | v1 | 44 |
| MiniMax / Hailuo AI | v1 | 43 |
| PixVerse | v2 | 37 |
| Mureka | v1 | 34 |
| Google Flow (Veo, Imagen 4, Nano Banana) | v1 | 25 |
| TemPolor | v1 | 21 |
| InsightFaceSwap | v1 | 18 |
| Dreamina (Seedance / Seedream) | v1 | 16 |
| Flow Music (Lyria 3 Pro) | v1 | 15 |

Plus a cross-cutting account API at `v2`, and the retired Midjourney v1/v2 (8 + 19 ops) kept for
the record.

Bearer auth is the **only** platform-wide contract — one long-lived token,
`Authorization: Bearer user:<number>-<unique-string>`, no scopes, no OAuth, no rotation or
revocation endpoint, no per-API key. `/.well-known/openid-configuration` and
`/oauth-authorization-server` both 404.

Everything else is per-service, and the vendor says so in its own words: identifier names
(`jobid` / `taskId` / `musicId`), job lifecycle, response shapes, webhook payload shape,
status-code semantics, and synchronous-vs-asynchronous behavior all vary by API. Version skew is
visible in the path — PixVerse sits on v2 while every other generation API is on v1, and the
account API is on v2. **Treat each of the ten as a separate integration.** That is not a criticism
of the docs; it is the direct consequence of wrapping ten unrelated products.

## Integration hazards

- **No idempotency, anywhere.** No key, no header, no dedupe window — nothing in the docs, the
  collections or the derived specs. A retried generation POST creates a second job and spends
  upstream credits. This is the most expensive fact in the profile.
- **7-day result retention**, then `410 Gone`. Persist your own outputs.
- **`596` is a real status code here.** Non-standard, and it means the linked upstream account's
  session is broken — re-auth that account, don't retry the request. The error catalog carries 16
  codes including four distinct Google 429 quota reasons that map to different cooldowns.
- **Upstream volatility is the operating condition.** Midjourney discontinued 2026-06-24;
  LTX Studio retired 2025-06-18; v1 sunset 2024-03-01. The set of APIs you can call is a function
  of ten other companies' tolerance for being wrapped.

## Governance and agent readiness — honest ledger

**Present:** llms.txt (plus a ~3 MB `llms-full.txt` verbatim docs dump, deliberately not committed);
the RFC 9727 catalog; a 93-entry dated changelog; dated in-place retirement notices on every
withdrawn API's docs page, with the old docs left online; TLS 1.3 on both hosts; DNSSEC, SPF, and
DMARC at `p=reject`.

**Absent, and not papered over:**

- **No MCP server.** `mcp.useapi.net` is a Cloudflare wildcard that 302s to the marketing homepage —
  it is not an endpoint. The MCP registry, npm and the GitHub org return nothing. `mcp/` here holds
  a **derived candidate** manifest (180 tools from the 298 operations, with account/credential CRUD
  deliberately excluded because those take raw browser cookies), clearly marked `status: candidate`.
- **No status page.** `status.useapi.net` is the same wildcard redirect.
- **No security.txt, no VDP, no trust center, no certifications.** The ToS disclaims warranties.
- **No HSTS, no CAA.**
- **No SDK for any current API.** The only first-party client libraries — `@useapi/midjourney-api`
  (npm) and its Python sibling — were generated from the SwaggerHub specs for the discontinued
  Midjourney API and are unmaintained. Distribution for the live APIs is Postman collections plus
  eleven runnable example repos.
- **No formal deprecation policy, no `Sunset`/`Deprecation` headers** (RFC 8594). The practice is
  better than the policy: notices are dated and specific, they just aren't machine-readable.

Enrichment deliberately **withheld** the `Idempotency`, `Compliance` and `StatusPage` pointers
rather than claim credit for things that do not exist. The `SDKs` pointer *is* set, and should be
read narrowly — it resolves to two official-but-unmaintained clients for a retired API.

`skills/` holds five API Evangelist-generated Agent Skills grounded in verified operationIds; the
vendor publishes no AGENTS.md and no packaged skills.

## Why it's in the network

Three reasons. It is a **live RFC 9727 implementation** in a catalog that has almost none. It is a
clean specimen of the **Postman-collection-as-contract** pattern — a provider doing machine-readable
distribution without ever touching OpenAPI. And it is the clearest example in the catalog of the
**unofficial-API-as-a-product** category, where the entire value proposition is absorbing ten other
vendors' instability, and where the governance profile is legible only against that fact.
