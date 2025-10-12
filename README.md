Virtual Keyboard — 3 Octaves, Modes, Instruments

Overview
- Three-octave, clickable and keyboard‑driven piano implemented in a single HTML file: `virtual_keyboard_two_octaves_modes_instruments.html`.
- Two play modes: Normal and Latch sustain.
- Multiple sound engines: SoundFont, WebAudioFont, and Pure Tones (oscillator) with graceful fallbacks.
- Scale guide with unobtrusive key markers and scale‑aware QWERTY mappings.
- Built‑in diagnostics: self‑test suite and a fallback banner for troubleshooting.

Quick Start
- Open `virtual_keyboard_two_octaves_modes_instruments.html` in a modern browser (Chrome, Edge, Firefox, Safari).
- Click anywhere in the page to allow audio (browsers require a user gesture to start Web Audio).
- Play notes by clicking the keys or using your computer keyboard.

Features
- Three Octaves: C3 through B5 with labeled white/black keys.
- Play Modes:
  - Normal: click rings for a configurable length; holding sustains while held.
  - Latch Sustain: click a key to start, click again to stop. Monophonic by design (new note replaces the old one).
- Sound Sources:
  - Sampled instruments via SoundFont (MusyngKite) with multi‑CDN loading.
  - Sampled instruments via WebAudioFont (Surikov presets) loaded on demand.
  - Pure tones (Web Audio oscillator): sine, triangle, square, sawtooth.
- Fallback Strategy:
  - Prefer SoundFont → if unavailable, try WebAudioFont → otherwise fall back to Pure Tones.
  - UI banner clarifies the current engine and offers temporary engine toggle buttons.
- Scale Guide:
  - Select Scale Type (None, Major, Minor) and a key (C…B).
  - Keys in the selected scale display a small blue dot (styled appropriately for white/black keys).
- QWERTY Keyboard Input:
  - Home row maps to scale degrees 1–7; accidentals on the row above; mirrored degrees on the bottom row.
  - Scale Lock (on by default) aligns letter keys to the current scale key/type.
  - Quick octave shifts: adjust the home-row octave/bottom-row offset in the panel or tap `[` / `]` / `\` while playing.
- Built‑in Diagnostics:
  - Self‑test validates library loading, engine fallbacks, range, scale markers, latch logic, and Stop All behavior.
  - Log output appears in the Diagnostics panel.

Controls
- Sound Source: choose sampled instruments (SoundFont / WebAudioFont) or Pure Tones.
- Volume: master output level.
- Stop All: immediately silences all active/latched notes and clears keep‑alives.
- Mode: Normal or Latch sustain.
- Default Length (Normal): ring time for a short click when not latched or held.
- Home Row Octave: choose the octave for `A S D F G H J` (defaults to 4; quick adjust with `[` and `]`).
- Bottom Row Offset: pick how far below the home row the `Z X C V B N M` letters sit (same octave, -1, or -2; press `\` to reset both rows).
- Self-Test: runs a quick diagnostic; results shown in the log panel.
- Scale Type: None, Major, Minor.
- Scale Key: C, C#, …, B.
- Scale Lock: when enabled, letter keys map to scale degrees for the selected scale.

QWERTY Mapping
- Home row (`A S D F G H J`) plays scale degrees 1–7 starting at your chosen Home Row Octave (defaults to C4); tweak it in the control panel or tap `[` / `]` to shift down/up quickly.
- Accidentals (`W E T Y U`) stay tied to the home row root, adding +1, +3, +6, +8, +10 semitones for chromatic fills.
- Bottom row (`Z X C V B N M`) mirrors the degrees in a lower register; use Bottom Row Offset (same octave, -1, -2) to decide how far below the home row it sits.
- Keyboard shortcuts: `[` and `]` shift both rows by ±1 octave; `\` snaps them back to the defaults.
- Notes sustain while the key is held in Normal mode; in Latch mode, each key press toggles the note on/off.

Audio Engines and Fallbacks
- SoundFont (preferred): loaded via `soundfont-player` from multiple CDNs. If an instrument fails to load, the app tries WebAudioFont next.
- WebAudioFont: loads a matching preset (when available). If that also fails, it falls back to Pure Tones.
- Pure Tones: always available; uses the browser’s Web Audio oscillator.
- Banner: on any failure/switch, a banner appears with temporary buttons to try an engine or select pure tones; the state is session‑only.

Latch Behavior Details
- Sampled engines (SoundFont/WebAudioFont) often play one‑shot samples that decay. To make latch reliable, the app:
  - Plays a short chunk (~3s) for latched notes and schedules a keep‑alive retrigger before decay.
  - Stops the previous voice on retrigger to avoid piling up voices.
  - Clears keep‑alive intervals on Stop, Stop All, engine change, or toggling the same key.
- Oscillator engine sustains indefinitely until stopped—no keep‑alive needed.
- Latch is monophonic by default: starting another note in latch mode replaces the current one.

How to Use Locally
- No build step required. Just open `virtual_keyboard_two_octaves_modes_instruments.html`.
- Network access:
  - Sampled engines load libraries/presets from CDNs. If offline or blocked, the app automatically falls back to Pure Tones and shows a banner.
  - You can still use the on‑page engine buttons to retry SoundFont or WebAudioFont when network conditions improve.

Troubleshooting
- No sound: ensure you’ve clicked the page or a control first (required to start audio on many browsers). Increase Volume.
- Stuck/noisy notes: click `Stop All`. Changing Sound Source also stops all notes first to prevent overlap.
- “Engine” banner keeps appearing: a library or preset failed to load from the network; try the other engine via the banner, or continue with Pure Tones.
- Latch fades after a few seconds: for sampled engines, ensure the app is in Latch mode; keep‑alive retrigger is automatic. For realistic percussive timbres (e.g., pianos), micro cross‑fade can be added later if desired.
- Keyboard letters not working: make sure the browser’s focus is on the page (click the background or a control). Scale Lock maps letters to the current scale.

Accessibility
- Keyboard supports pointer events and computer keys.
- Clear focus indicators on interactive controls.
- Descriptive `aria-label` on the keyboard region.

File Layout
- Main app: `virtual_keyboard_two_octaves_modes_instruments.html`
- Notes and rationale: `dev-notes.md`

Implementation Notes
- Uses the Web Audio API for pure tones and as the audio destination for sampled engines.
- Dynamically loads `soundfont-player` and `WebAudioFontPlayer` with readiness checks and multiple CDNs.
- Instruments/presets are fetched lazily; the UI remains responsive with fallbacks.
- Scale markers compute pitch classes from the selected key/type and render unobtrusive dots on each matching key.
- QWERTY mapping respects Scale Lock, with octave anchoring and clamping to the C2–B5 range.

Ideas and Next Steps
- Optional polyphonic latch with a “Mono” toggle.
- Additional scales/modes (harmonic/melodic minor, church modes, pentatonic) and custom user scales.
- Velocity sensitivity from keyboard (e.g., map key press duration or modifiers to velocity) and touch pressure where supported.
- Simple effects chain for sampled engines (reverb/chorus) and a per‑instrument preset browser.
- Offline support (self‑host or service‑worker caching of libraries/presets) to reduce reliance on CDNs.

Credits
- SoundFont playback via `soundfont-player`.
- WebAudioFont presets and loader via `WebAudioFont`.
- Built on standard Web Audio API.
