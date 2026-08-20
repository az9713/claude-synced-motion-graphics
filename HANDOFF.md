# HANDOFF — resume point for claude_design_part_N_zinho

**Read this first each new session.** State lives in this
folder on disk. Global conventions live in the user agent config (not in this repo).

## What this folder is
Study notes + experiments around the Claude Design video workflow
(YouTube: https://www.youtube.com/watch?v=gEPfF6BFAB4). `README.txt` holds
The user's raw notes on the 6 techniques (design systems, motion from prompts,
ref images, SRT-synced animation, "tweak don't restart").

## Current state (2026-08-19) — all done, nothing in progress
- **Face-swap variant shipped (2026-08-19 evening):**
  `videos/stock_report/output_professor.mp4` — same 5 overlay cards on a
  swapped base clip where the talking head is a fictional retired professor
  (AI face, no real person). Pipeline: nano-banana face gen ($0.04) →
  fal storage upload → Wan 2.2 Animate replace 720p ($1.50, ~25 min) →
  audio survived intact → HyperFrames re-render in `public_professor/`
  (a copy; original `public/` + `output.mp4` untouched). Plan of record:
  `faceswap-plan.html`. Assets + sidecars in `generations/`. FAL_API_KEY
  lives in project `.env`. Verified recipes cached in genmedia `models.md`.
- `stock_report.srt` — 3-cue SRT of `stock_report.MOV` (10s selfie clip),
  transcribed locally with faster-whisper, split at speech pauses.
- `videos/stock_report/output.mp4` — the main deliverable: the selfie clip
  with 5 transcript-synced motion-graphic overlay cards (title badge, GOOGL
  +30% / MSFT −20% / TSLA +5% count-up callouts, closing summary strip).
  Built with the HyperFrames `/talking-head-recut` workflow, 1080×1920,
  30fps, verified frame-by-frame and by audio loudness.
- `videos/stock_report/` also holds every intermediate: `transcript.json`
  (word-level timings), `storyboard.json`, `public/index.html` (the editable
  composition), `public/cards/*.html`, snapshots + proof frames.
- `dev-journey-stock-report.html` — full technical write-up of the build
  (pipeline, decisions, failures, verification). Images reference
  `videos/stock_report/` by relative path — keep them side by side.
- Removed by the user outside the session (noticed 2026-08-19): `narration.srt`
  (413-cue SRT of the YouTube video) and `IMG_2371.MOV`. Both are gone from
  disk; do not look for them. `narration.srt` is regenerable via the yt-dlp +
  dedup pattern below.

## Next task
- **None queued.** The folder is at a natural rest point. One open option
  named but not requested: the true-B-roll variant of the stock video
  (cards full-screen, face in a corner pip — small edit to
  `videos/stock_report/public/index.html` GSAP timeline + re-render). Do it
  only if asked.
- To re-render after any edit:
  `cd videos/stock_report && npx hyperframes render public --skill=talking-head-recut -o output.mp4 --fps 30`

## Session-transient scratch (regenerate if needed; durable outputs committed)
- `clean_srt.py` (scratchpad, gone after clear) — parsed YouTube rolling
  auto-captions: split blocks on `^\d+\ntimestamp` headers (NOT blank lines —
  9/825 blocks had internal empty lines), took last non-blank line per block,
  merged consecutive duplicates. Its output `narration.srt` was later deleted;
  this pattern regenerates it from
  https://www.youtube.com/watch?v=gEPfF6BFAB4 if ever needed again.
- `whisper_srt.py` (scratchpad) — faster-whisper `base`/`small.en` with
  `word_timestamps=True`, CUDA-first with CPU fallback, cues split at clause
  punctuation. Durable records: `stock_report.srt`,
  `videos/stock_report/transcript.json`.

## Gotchas already pushed to MMS (don't re-derive)
- hyperframes: transcribe needs whisper-cpp (absent) → substitute
  faster-whisper; snapshot wipes its output dir each run; iPhone MOV rotation
  lies in ffprobe — probe a real frame. See MMS index "HyperFrames on this
  machine" entry.
