# useapi.net

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
