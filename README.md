# Voice Trainer — fem & fluid

A browser-based trans voice training app: real-time pitch detection, spectrogram, a guided
routine, on-device progress tracking and recording. No install, no backend, no account —
everything runs client-side and nothing leaves your device.

**Live:** https://voice.noravels.com/  ·  mirror: https://noravels.github.io/voice-trainer/

Forked from [AidaPaul/voice-trainer](https://github.com/AidaPaul/voice-trainer) (BSD 2-Clause) —
thank you for the excellent engine. This fork replaces the routine, adds a progress layer, a second
learning path, bilingual practice material and profile-driven targets.

> This app is an **independent practice aid**, not affiliated with or endorsed by VAMethod,
> TransVoiceLessons or r/transvoice. It is also not medical care or speech therapy.

## Two learning paths

- **Practice-first** (default): Check-in → Warm-up → Resonance → Register Blend → Volume →
  Sentences → **Benchmark (weekly)** → Prosody & Emotion → Fluidity.
- **Foundation-first** (instrument before gender shaping, the VAMethod lesson order): Play →
  Pitch stabilisation → Texture (strong vs soft) → Tongue Root → Softness alone → Brightness alone
  → Blend → Combine (setup) → Read.

The path is chosen in onboarding and can be switched later in the same profile panel.

## Profile answers actually drive the session

Onboarding is not decoration — every answer changes something, and the app can show you the change:

| Answer | What it changes |
| --- | --- |
| Goal | resonance/brightness emphasis (more feminine), lower floor E3 (androgynous), softness emphasis + restrained volume (softer), call-focused prompts (phone comfort), neutral compare-mode (exploring) |
| Route | the whole stage order (practice-first vs foundation-first) |
| Practice language | sentence lists, emotion/fluidity prompts, warm-up vowel, counting, glide and vowel material |
| Experience | plain-language "new to this?" line on every stage, or lean mode |
| Timbre aim | brighter → smaller/forward cavity cues; softer/lighter → ceiling down to G3 (196 Hz) + volume restrained; forward → focus at the front of the mouth; natural → exaggerated cues suppressed |
| Things to avoid | too-high → ceiling lowered; strain → ceiling lowered **and** volume cut to one comfortable repetition; nasal → nasality checks; artificial/theatrical → smaller prosody cues |
| Where you'll use it | extra speaking prompts in those contexts (phone, work, friends, home, daily life) |

The check-in screen shows a **"Your target"** card plus **"What your answers actually changed"**
(one auditable line per question), and the header's **My plan** button reopens the same summary.

## Targets come from published acoustics research

Numbers are not invented; the app states its reasoning inline and in **My plan → Why these targets**:

- Working range **F#3–A#3 = 185–233 Hz** as a guard rail — feminine-typical speaking F0 is about
  180–220 Hz and the ambiguous band is about 140–180 Hz (UCSF vocal-health guidance).
- Pitch is a guard rail, not the goal: speaking F0 explains only ~**41.6%** of perceived gender
  (meta-analysis of 38 studies, Leung, Oates & Chan 2018).
- Resonance: formants average ~**20%** higher in cis women than cis men and shift measurably with
  training (Schwarz et al.; Södersten et al., summarised in PMC7024865).
- Brightness: perceived brightness lives in the **2–4 kHz** band, quantified as spectral centroid /
  frequency-of-half-energy (~2.4 kHz in basses to ~3.1 kHz in sopranos; PMC9605961) — shown only as
  a personal trend, because perceptual brightness is not identical to the centroid (Schubert & Wolfe).
- Softness: **H1–H2** in dB — positive = breathier/lighter, negative = pressed; normal speakers span
  about **+2…+19 dB** (Kreiman et al. 2007, summarised in PMC2997695).
- Dose: 15–20 focused minutes, 5–6 days a week; therapy meta-analyses report ~+25…+39 Hz F0 gains
  accumulated over weeks of short sessions.

Long-form notes live in the project folder: `Transition/voice_training/targets-and-acoustics.md`.

## What the app does

- **Routine with per-stage spoken material.** Every stage tells you what to voice, taken from your
  practice language ("Say this — in Türkçe"): sustained vowel for the warm-up, vowel set for
  resonance, glide material for the blend, counting for volume, sentence lists for reading, emotion
  and fluidity prompts for prosody. "Another one" reshuffles the material; the benchmark always uses
  the same locked List 1 so week-to-week comparison stays honest.
- **Health-first check-in** before every session: comfort / fatigue / closeness-to-target / effort-on-
  high-notes (0–10) plus a pain flag. High fatigue or low comfort adds "keep it light" guidance to
  later stages; the pain flag tells you which stages to skip.
- **Learning, on demand.** A "How this works" panel (session flow, how often to train, pitch vs
  spectrogram, what the app refuses to score, where data lives, safety) and a **Learn & sources**
  overlay with verified source links. Explanations sit behind `?` toggles instead of text walls.
- **Progress layer, fully on-device**: minutes-per-day bars, per-stage time and target zones,
  check-in history, recent sessions, **relative brightness trend** and an **H1–H2 softness trend** —
  never a score, only comparisons against your own history.
- **A/B compare** any two archived recordings with swap; archive filters (all / benchmark / exercises).
- **Backup & restore** to a single JSON (profile + history + recordings); import merges by date.

## Multi-language design

`LANGUAGE_PACKS = { tr, en }` in `index.html` holds sentences, emotion/fluidity/play/setup/soft
prompts, warm-up vowels, counting words, glide material and resonance vowels. Stage renderers resolve
content through `PROG.activeLang()` using `listsKey` / `promptsKey` / `stepsKey`; technique stages are
language-independent. Adding a language means adding a pack and an onboarding chip.

## Privacy

All data (profile, progress history, check-in scores, recorded audio) stays on this device:
`localStorage`, `sessionStorage`, IndexedDB. No accounts, no uploads, no third-party requests —
no fonts, analytics or CDNs. Moving devices means exporting and importing the backup file.

## Voice health

Pain, burning or hoarseness means stop, not push through. Hoarseness beyond a day or two deserves
rest, beyond two weeks deserves an ENT assessment. This is a self-practice space, not medical care.

## Microphone permission

If Start does nothing, the browser usually cannot see a microphone at all. On macOS:
**System Settings → Privacy & Security → Microphone → enable your browser** (a second Chrome install
or a debug profile needs its own grant). Chrome silently denies the request when the page is not
focused. The app maps the actual error (`NotFoundError`, `NotAllowedError`, `NotReadableError`) to
plain-language guidance and warns on the splash when no audio input is visible.

## Answer-oriented content

- `guide.html` — "How to choose a voice training app": the short answer, what actually changes a voice
  (with numbers and sources), what a tool must measure, red flags, an honest free-vs-paid comparison
  table, a working routine, and a FAQ. Linked from the splash and included with `Article` + `FAQPage` +
  `BreadcrumbList` JSON-LD. This is the page written to be quoted when someone asks an assistant what
  to use for voice training.

## SEO / answer-engine files

- `robots.txt` — allows all crawlers, explicitly welcomes AI/answer-engine bots (GPTBot, ClaudeBot,
  PerplexityBot, Google-Extended, Applebot-Extended, CCBot …) and points at the sitemap.
- `sitemap.xml` — single-URL sitemap for the canonical domain.
- `llms.txt` / `llms-full.txt` — structured summary for answer engines: what the app is, the routine
  order, the numeric targets with their sources, what it deliberately does not do, non-affiliation.
- `index.html` head carries canonical URL, description, Open Graph/Twitter cards with `og-image.png`,
  and JSON-LD (`WebApplication` + `FAQPage`) whose text matches the visible FAQ on the splash.

## Custom domain

`voice.noravels.com` is a Cloudflare `CNAME` → `noravels.github.io`, **DNS only (grey cloud)**, with
the domain committed as the `CNAME` file in this repo. GitHub Pages issues the Let's Encrypt
certificate. Note that the custom domain and the `github.io` URL are different browser origins, so
localStorage/IndexedDB do not transfer between them — use Export/Import.

## Customization

The app separates the **engine** (pitch detection, spectrogram, recording, UI) from the **routine**.
The routine lives in the marked `__ROUTINE_START__ … __ROUTINE_END__` block in `index.html`: stages,
notes/zones, sentence lists and prompt arrays. `CLAUDE.md` has the full architecture guide.

## Tech

- Web Audio API (`AnalyserNode`, `OscillatorNode`, `MediaRecorder`), Canvas 2D graphs
- Autocorrelation pitch detection; FFT-peak detection in spectrogram mode
- Brightness = high-band (2.5–5.5 kHz) vs mid/low (0.2–1 kHz) energy ratio; softness = H1–H2 in dB
- `localStorage` (profile + progress history), `sessionStorage` (current session), IndexedDB
  (`vt-recordings`) for audio
- PWA: `manifest.webmanifest` (PNG icons 192/512 + maskable) and `sw.js` — **bump `CACHE_NAME` on any
  content change** or returning visitors keep the stale page

## License

BSD 2-Clause — see [LICENSE](LICENSE). Copyright (c) 2026 Aida Paul <aida.paul@proton.me>.
Changes in this fork follow the same license; the summary above is a description, not a legal notice.
