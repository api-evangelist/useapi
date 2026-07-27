---
name: Generate and download a song with Mureka via useapi.net
description: >-
  Link a Mureka account, generate a song from a prompt or from custom lyrics, extend or regenerate
  it, and download the finished audio through the useapi.net Mureka API.
api: openapi/useapi-mureka-v1-openapi.yml
base_url: https://api.useapi.net/v1/mureka
operations:
  - postAccounts
  - getAccounts
  - getMusicMoodsAndGenres
  - postMusicLyricsGenerate
  - postMusicCreate
  - postMusicCreateAdvanced
  - postMusicCreateInstrumental
  - getJobsByJobid
  - getMusicBySongid
  - postMusicExtend
  - postMusicRegenerate
  - postMusicDownload
generated: '2026-07-27'
method: generated
---

# Generate a song with Mureka via useapi.net

## Before you start

useapi.net fronts your own Mureka AI account. Link it first; generations consume that account's
credits. Send `Authorization: Bearer user:<number>-<unique-string>` on every request — the complete
token, unencoded.

## Steps

1. **Link the Mureka account** — `postAccounts` (`POST /accounts`), then verify with `getAccounts`
   (`GET /accounts`).
2. **Optionally look up the vocabulary first** — `getMusicMoodsAndGenres`
   (`GET /music/moods-and-genres/`) returns the moods and genres Mureka accepts, and
   `getMusicVocals` / `getMusicRefs` list available vocal and reference material. Use these instead
   of guessing style strings.
3. **Get lyrics if you don't have them** — `postMusicLyricsGenerate`
   (`POST /music/lyrics-generate`) writes lyrics from a brief.
4. **Create the song.** Pick the right entry point:
   - `postMusicCreate` (`POST /music/create`) — the standard path from a prompt or lyrics.
   - `postMusicCreateAdvanced` (`POST /music/create-advanced`) — when you need finer control.
   - `postMusicCreateInstrumental` (`POST /music/create-instrumental`) — no vocals.
   Set `replyUrl` (and `replyRef` for correlation) to get a callback rather than polling.
5. **Wait for the job** — poll `getJobsByJobid` (`GET /jobs/{jobId}`) or handle the webhook. Mureka's
   own identifier for a finished track is `musicId` / `song_id`, not the job id — read the finished
   track with `getMusicBySongid` (`GET /music/{song_id}`).
6. **Iterate if needed** — `postMusicRegenerate` (`POST /music/regenerate`) for a different take,
   `postMusicExtend` (`POST /music/extend`) to lengthen, `postMusicVideoGenerate`
   (`POST /music/video-generate`) to pair it with visuals.
7. **Download** — `postMusicDownload` (`POST /music/download`) returns the audio. Do this within the
   **7-day** job retention window.

## Voice cloning

`postFilesVocal` (`POST /files/vocal/`) uploads a vocal reference and `postSpeechVoice`
(`POST /speech/voice/`) registers a cloned voice; `getSpeechVoices` (`GET /speech/voices/`) lists
them and `postSpeech` (`POST /speech`) synthesizes with one. Only upload voices you have the right
to clone.

## Rules an agent must not get wrong

- **No idempotency key exists.** A retried create makes a second song and spends credits again.
  Reconcile with `getJobs` (`GET /jobs/`) before resubmitting anything.
- **`412 Insufficient credits`** (`{"error": "Not enough credits: requested 3, available 0", "code": 412}`)
  means the *Mureka* account is out, not useapi.net. `402` means the useapi.net subscription lapsed.
  These need different fixes — do not conflate them.
- **`596`** means the linked Mureka account's session is broken. Re-add the account; retrying will
  not help.
- **Mureka's identifiers are its own.** `musicId` / `song_id` here, `jobid` on Google Flow, `taskId`
  on Runway. Do not carry an id field name across useapi.net services.
- Honor `Retry-After` on every `429`.
