# Voice Trainer — Fem & Fluid

A browser-based trans voice training app with real-time pitch detection, spectrogram visualization, guided exercises, on-device progress tracking, and recording. No install, no backend — runs entirely client-side and nothing leaves your device.

**Live:** https://noravels.github.io/voice-trainer/

Forked from [AidaPaul/voice-trainer](https://github.com/AidaPaul/voice-trainer) (BSD 2-Clause) — thank you for the excellent engine. This fork replaces the routine and adds a progress layer.

## What changed from upstream

- **Fem & fluid routine (8 stages)** informed by [@vamethod_transvoice](https://www.instagram.com/vamethod_transvoice/) (VAMethod) — an independent practice aid, not affiliated with or endorsed by VAMethod. Pedagogical root: Cornelius Reid's functional voice training. Stages: safety check-in → "ew" warm-up → Turkish-vowel resonance (dark→bright) → volume without weight → Turkish sentence reading → weekly benchmark (locked text) → prosody/emotion → fluidity switching.
- **Health-first check-in** before every session: comfort / fatigue / closeness-to-target sliders (0–10) and a pain flag that shows a rest-first banner. Scores are tracked per session.
- **Progress layer, fully on-device**: day streak, minutes-per-day bars, per-stage time and target zones, "how practice has felt" table, recent sessions, and a **relative brightness trend** (2.5–5.5 kHz vs 0.2–1 kHz band energy ratio). Deliberately never a score — no "female/male" judgment, trends are only relative to your own history.
- **A/B compare in the dashboard**: pick any two archived recordings for side-by-side playback with swap \u2014 hear week-over-week change directly.
- **Recording archive filters**: all / benchmark / exercises, plus per-recording delete.
- **Daily nudge strip**: last-practice day + minutes today, gentle 15\u201320 min framing.
- **Prosody/emotion prompt pool** extended with real-life Turkish call patterns (delivery call, cafe chat, interview, tired one-liners).
- **Backup & restore**: export profile + history + recordings to a single JSON; import merges (dedupe by session date) \u2014 kept device-local, no cloud.
- **Voice profile & onboarding**: goal / experience / timbre aims / things to avoid / use contexts (chip selectors). Shapes framing, stays on-device, editable anytime. First-time baseline guidance (locked read + free talk) stamps the profile.
- **Weekly benchmark**: a locked sentence list so week-over-week recordings stay comparable.
- **Recording archive** in IndexedDB, week-indexed, with playback and save, surviving reloads.
- Session save/discard gate on Finish and New Session; sub-30s sessions are not recorded into history.
- Turkish reading material throughout (prosody drills target Turkish stress/intonation patterns).

## Features (inherited + extended)

- **Real-time pitch detection** (autocorrelation) and  FFT-peak spectrogram (log scale, 60 Hz–5 kHz)
- **Guided 8-stage routine** with timers, reference tones, and target zones
- **Custom reference lines** — right-click the graph to pin any frequency
- **Session & exercise recording** — exercise recordings also persist to IndexedDB
- **Progress dashboard** — streak, minutes/day, stage breakdown, check-in scores, recording archive, brightness trend
- **Single HTML file** — no build step, no dependencies; PWA envelope for home-screen install and offline use

## Voice health

Pain, burning, or hoarseness means stop — not push through. This is a self-practice space, not medical care or speech therapy. Details are on the app's splash screen.

## Privacy

All data (progress history, check-in scores, recordings) stays on your device: `localStorage`, `sessionStorage`, and IndexedDB. No accounts, no uploads, no network calls beyond loading the app itself.

## Customization

The app separates the **engine** (pitch detection, spectrogram, recording, UI) from the **routine**. The routine lives in the marked `__ROUTINE_START__ … __ROUTINE_END__` block in `index.html`: stages, notes/zones, sentences, and prompt arrays. See `CLAUDE.md` for the full architecture guide.

## Tech

- Web Audio API (`AnalyserNode`, `OscillatorNode`, `MediaRecorder`), Canvas 2D graphs
- Autocorrelation pitch detection; FFT-peak detection in spectrogram mode
- `localStorage` (progress history), `sessionStorage` (current session), IndexedDB (recording archive)
- PWA: `manifest.webmanifest` + `sw.js` (stale-while-revalidate; bump `CACHE_NAME` on any content change — see `CLAUDE.md`)

## License

BSD 2-Clause — see [LICENSE](LICENSE). Copyright (c) 2026 Aida Paul <aida.paul@proton.me>.

Changes in this fork follow the same license; the Upstream section above is a summary, not a legal notice.
