<div align="center">

# LoopSmith

**A zero-dependency procedural music generator for small games.**

Compose, audition, edit, export, and hand off code-generated game loops from one standalone HTML file.

[![HTML5](https://img.shields.io/badge/HTML5-standalone-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![Web Audio API](https://img.shields.io/badge/Web%20Audio%20API-realtime%20synthesis-7B6CFF)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
[![Godot 4](https://img.shields.io/badge/Godot%204-handoff-478CBF?logo=godotengine&logoColor=white)](https://godotengine.org/)
[![Dependencies](https://img.shields.io/badge/dependencies-0-D8FF62?labelColor=202126)](#requirements)
[![Offline](https://img.shields.io/badge/offline-ready-D8FF62?labelColor=202126)](#privacy-and-offline-use)
[![Loop length](https://img.shields.io/badge/sequence-36%20steps-FF8A65?labelColor=202126)](#how-the-generator-works)

[Quick start](#quick-start) · [How it works](#how-the-generator-works) · [Godot handoff](#godot-handoff) · [Troubleshooting](#troubleshooting)

</div>

---

## Overview

LoopSmith is a compact browser-based music laboratory designed for procedural game soundtracks. It does not play pre-recorded MP3, WAV, or OGG assets during composition. Instead, it creates notes with the Web Audio API, schedules them in real time, and combines melody, bass, optional pads, and soft percussion into a looping track.

The project is intentionally small: the entire application lives in [`index.html`](./index.html), has no package manager, requires no server, and can be opened directly in a modern browser.

LoopSmith is especially useful for code-first games where music is generated at runtime—for example, a Godot project using `AudioStreamGenerator` and PCM sample buffers.

## Features

- **One-click full regeneration** — tempo, energy, density, brightness, space, swing, key, scale, harmony, melody, pad, drums, and timbre can all change together.
- **Musical phrase generation** — loops are built from motifs, related variations, answer phrases, and a closing cadence instead of unrelated random notes.
- **Dense 36-step melodies** — every generated loop contains 28–36 active notes, with rests restricted to weak beats.
- **Editable sequencer** — click any cell in the melody grid to add, move, or remove a note.
- **Real-time synthesis** — hear every loop immediately through the Web Audio API.
- **Live waveform** — inspect the current signal while the loop plays.
- **Multiple sound characters** — bell, warm, glass, crystal, pluck, and soft synthesis styles.
- **Layered arrangement** — sustained bass, optional ambient pad, and optional lightweight drums.
- **Built-in game SFX preview** — audition Tap, Move, Deny, Coin, and Win sounds.
- **WAV export** — render two repetitions of the current loop as a standard WAV file.
- **Structured JSON export** — save all musical and integration parameters for tools or agents.
- **Godot constants** — copy the generated scale, roots, pattern, timing, and timbre values.
- **Agent-ready handoff** — produce a detailed implementation prompt for a coding agent working on the game.
- **Offline and private** — synthesis and rendering happen locally on the device.
- **Zero dependencies** — no build tools, libraries, frameworks, accounts, or API keys.

## Quick start

1. Download or clone the repository.
2. Open [`index.html`](./index.html) in Chrome, Edge, or Firefox.
3. Press **Play** to hear the current loop.
4. Press **New loop · change everything** to generate a substantially different variation.
5. Adjust the controls or edit individual notes in the melody grid.
6. Use **Export WAV** to save an audio preview, or expand **Game handoff** to copy implementation data.

No installation command is required.

## Requirements

- A modern desktop browser with Web Audio API support.
- JavaScript enabled.
- Audio output for live playback.

Recommended browsers:

| Browser | Recommendation |
| --- | --- |
| Chrome | Recommended |
| Microsoft Edge | Recommended |
| Firefox | Supported |
| Safari | Expected to work, but browser autoplay rules may require an extra click |

## Interface

### Live Signal

The top panel contains the waveform and playback controls:

- **Play / Stop** starts or stops real-time synthesis.
- **New loop · change everything** creates a new musical identity.
- **Export WAV** renders two complete loop repetitions.
- The session summary shows the key, loop duration, and selected character.

Pressing the space bar also toggles playback when focus is not inside an input or button.

### Melody

The sequencer contains 36 steps arranged as nine visual bars of four steps. Rows represent scale degrees rather than fixed chromatic notes, which keeps manual edits inside the selected musical scale.

Clicking a cell moves the note for that step to the chosen degree. Clicking the currently active cell removes the note and creates a rest.

The caption above the grid reports:

- the source motif;
- the active-note count;
- the phrase structure used to develop the loop.

### Sound Settings

| Control | What it changes |
| --- | --- |
| Starting point | Loads a known musical setup or returns to generated variations |
| Seed | Reproduces the deterministic melody choices for that value |
| Root note | Transposes the complete composition |
| Scale | Selects the pitch collection used by melody and harmony |
| Tempo | Changes playback speed and total loop duration |
| Energy | Changes note length and layer intensity |
| Note density | Controls a limited number of intentional rests |
| Timbre brightness | Changes filtering and harmonic strength |
| Space | Controls the wet reverb contribution |
| Swing | Delays alternating steps for a looser rhythmic feel |
| Ambient pad | Adds quiet sustained chord tones |
| Soft drums | Adds a restrained kick-and-hat layer |

The density range is deliberately limited to `68–100%`. This prevents accidental near-empty melodies while still allowing rhythmic breathing room.

### Game Handoff

The collapsed panel at the bottom contains three representations of the same loop:

- **Agent** — a complete natural-language implementation request.
- **JSON** — structured data for tools, scripts, or storage.
- **Godot** — compact constants and implementation notes for `Audio.gd`.

## How the generator works

LoopSmith uses seeded pseudo-random choices inside musical constraints. A seed can therefore produce variety without making the result arbitrary or impossible to reproduce.

### 1. Character selection

The full-regeneration action first selects one of 16 musical characters. Each character defines safe ranges for:

- tempo;
- energy;
- density;
- brightness;
- space;
- swing;
- scale choices;
- pad and drum availability;
- synthesis voice styles.

The same character is not intentionally selected twice in a row.

### 2. Motif selection

One of 16 handcrafted eight-step motifs becomes the musical identity of the loop. Motifs favor the smooth, bright, bell-like movement common in lightweight puzzle games.

The motif may be transposed, gently inverted, or smoothed to avoid awkward jumps while staying inside the selected scale.

### 3. Phrase development

The motif is expanded into related phrases rather than copied blindly. LoopSmith can create:

- an opening theme (`A`);
- a close variation (`A′`);
- a contrasting or answering phrase (`B`);
- a further variation (`A″` or `B′`);
- a four-step cadence that resolves to the tonic.

The resulting form is one of four phrase structures, such as `A A′ B A″ + coda`.

### 4. Controlled rests

Rests are placed only after the complete phrase exists. The generator enforces these rules:

- at least 28 of 36 steps contain notes;
- the first step of every four-step visual bar always contains a note;
- a visual bar contains at most one rest;
- adjacent rests are avoided;
- the final four-step cadence contains no rests.

This prevents the sparse, disconnected patterns produced by independent per-step randomization.

### 5. Harmony and bass

Harmony comes from one of 24 root progressions. A root lasts for eight sequence steps, giving the melody time to establish a phrase before the harmony moves. Roots cycle when the 36-step loop extends beyond the four-value progression.

The bass follows the same eight-step harmonic rhythm and uses long, softly decaying notes similar to the original code-generated game soundtrack.

### 6. Synthesis

The main voice combines a primary oscillator with a quieter harmonic oscillator:

```text
signal = primary(phase) + harmonic_mix × sin(phase × harmonic_ratio)
```

Each note uses a short attack, exponential decay, and smooth release to avoid clicks. Brightness adjusts both filtering and harmonic presence. Space routes part of the signal through a locally generated convolution impulse.

## Variation model

The generator currently combines:

- 16 musical characters;
- 16 source motifs;
- 4 phrase structures;
- 24 harmonic progressions;
- 12 root notes;
- 4 scales;
- 6 voice styles;
- continuous ranges for tempo, energy, density, brightness, space, and swing;
- deterministic melodic transformations and controlled rest placement.

The practical number of possible loops is therefore far larger than the raw preset count. The constraints are designed to keep those combinations recognizable as music from the same game family.

## Exporting audio

Press **Export WAV** to render the current arrangement with `OfflineAudioContext`.

The exported file:

- contains two full repetitions of the loop;
- uses a 44.1 kHz render sample rate;
- includes melody, bass, enabled pad, enabled drums, filtering, and space;
- can be used for review, sharing, or comparison.

The WAV is a preview artifact. The recommended in-game workflow remains procedural synthesis from the exported parameters.

## Godot handoff

LoopSmith is designed to describe a procedural implementation rather than merely provide an audio file.

Recommended workflow:

1. Finish the loop in LoopSmith.
2. Expand **Game handoff**.
3. Open the **Agent** tab.
4. Press **Copy handoff**.
5. Paste the result into the coding agent that has access to the Godot project.
6. Ask the agent to inspect the existing audio generator before changing it.
7. Audition the result in the game and keep the LoopSmith seed with the project notes.

The handoff includes:

- sample rate and bus expectations;
- scale semitones;
- harmonic roots;
- the complete 36-step pattern;
- step and note durations;
- voice style and harmonic ratio;
- brightness, space, and swing;
- pad and drum flags;
- reference frequencies for every melodic step;
- instructions to preserve existing SFX and volume controls.

### JSON structure

The JSON export is organized into these top-level sections:

```json
{
  "format": "loopsmith-game-music-v1",
  "seed": "example-seed",
  "variation_profile": "Classic Dots",
  "tempo": {},
  "harmony": {},
  "composition": {},
  "melody": {},
  "sound": {},
  "arrangement": {},
  "integration": {}
}
```

## Game sound effects

The SFX buttons provide simple procedural reference sounds:

| Event | Reference behavior |
| --- | --- |
| Tap | Short high ping |
| Move | Slightly lower confirmation ping |
| Deny | Low negative tone |
| Coin | Two-note ascending sparkle |
| Win | Four-note rising phrase |

They are previews and remain separate from the music arrangement.

## Privacy and offline use

LoopSmith does not upload the melody, seed, exports, or settings. It does not require an account, analytics service, external API, or network connection after the file has been downloaded.

The only external requests made by the document are optional badge images when viewing this README on GitHub. The application itself is self-contained.

## GitHub repository link

The header includes a GitHub button. Until a repository URL is configured, it opens the GitHub home page.

After creating the repository, find this constant near the beginning of the script in [`index.html`](./index.html):

```js
const GITHUB_REPOSITORY_URL = 'https://github.com/';
```

Replace it with the final repository URL:

```js
const GITHUB_REPOSITORY_URL = 'https://github.com/YOUR_USERNAME/YOUR_REPOSITORY';
```

## Project structure

```text
LoopSmith/
├── index.html   # Interface, sequencer, synthesis, export, and handoff logic
└── README.md    # Project documentation
```

## Troubleshooting

### No sound after pressing Play

- Check that the browser tab and operating system are not muted.
- Press **Play** directly once; browsers require a user gesture before creating audio.
- Try Chrome or Edge if the current browser blocks local-file audio behavior.

### The waveform is flat

The waveform remains nearly flat while stopped. Start playback and confirm that the status in the header changes to **Playing**.

### Copy Handoff does not use the clipboard

Some browsers restrict the Clipboard API on `file://` pages. LoopSmith includes a legacy selection-and-copy fallback, but browser policy can still intervene. If necessary, select the text inside the handoff field manually and copy it.

### WAV export takes a moment

The browser renders two complete loops before creating the download. Longer, slower-tempo loops require more samples and therefore take longer to export.

### A manually edited melody contains too many rests

The density guarantees apply to generated melodies. Manual editing is intentionally unrestricted, so clicking active cells can create additional rests. Press **New loop · change everything** to return to a validated generated pattern.

## Design goals

- Keep the project understandable and portable.
- Prefer musical constraints over uncontrolled randomness.
- Make every generated loop reproducible from a seed.
- Produce instructions suitable for runtime synthesis in a game engine.
- Avoid audio assets when a lightweight procedural implementation is enough.
- Preserve direct manual control after generation.

## Contributing

Contributions are welcome after the project is published. Useful areas include:

- additional motif libraries;
- new scale families;
- more synthesis voices;
- improved stereo movement;
- additional engine handoff formats;
- browser compatibility testing;
- accessibility improvements for the sequencer.

Please keep new features dependency-free unless a dependency provides a clear and substantial benefit.

## License

This project is distributed under the **Apache-2.0 license**.

See [`LICENSE`](./LICENSE) for full legal text.

## ❤️ Support the Project

If you find this tool useful, consider leaving a ⭐ on GitHub