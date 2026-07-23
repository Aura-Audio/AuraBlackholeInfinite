# AuraBlackholeInfinite

### Blackhole / Infinite Reverb

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status: Stable](https://img.shields.io/badge/status-stable-success)
![Web Audio API](https://img.shields.io/badge/Web%20Audio-AudioWorklet-8b5cf6)
![Input: Live Microphone](https://img.shields.io/badge/input-live%20microphone-22d3ee)
![Build: None](https://img.shields.io/badge/build-none-lightgrey)

A single-page Web Audio application that transforms live microphone input into an evolving, infinite reverb cloud. The processor is designed for ambient drones, sci-fi scoring, meditation textures, experimental vocal processing, and self-sustaining sound design.

The effect is implemented entirely in the browser using the Web Audio API and an `AudioWorklet` feedback-delay network. No audio is uploaded to a server.

---

## Table of Contents

- [Overview](#overview)
- [Live Demo](#live-demo)
- [Features](#features)
- [Quick Start](#quick-start)
- [Running Locally](#running-locally)
- [Deploying to GitHub Pages](#deploying-to-github-pages)
- [Usage Notes](#usage-notes)
- [Controls](#controls)
- [Presets](#presets)
- [Technical Architecture](#technical-architecture)
- [Browser Compatibility](#browser-compatibility)
- [Performance Notes](#performance-notes)
- [Troubleshooting](#troubleshooting)
- [Project Structure](#project-structure)
- [Privacy](#privacy)
- [Limitations](#limitations)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)

---

## Overview

Blackhole / Infinite Reverb is a browser-based real-time audio processor inspired by classic infinite reverb and shimmer devices. It submerges the input signal in a dense, modulated feedback network where transients dissolve into a long, evolving wash.

The application is suitable for:

- Ambient drone generation
- Experimental vocal processing
- Sci-fi sound design
- Meditation and texture generation
- Film and game scoring
- Live performance sound processing
- Browser-based audio research

The processor emphasizes extremely long decay, high feedback, modulation, pitch shifting, and filtering to prevent static resonances.

---

## Live Demo

Replace the following link with your deployed GitHub Pages URL:

[https://your-username.github.io/blackhole-infinite-reverb/](https://your-username.github.io/blackhole-infinite-reverb/)

> Headphones are strongly recommended. Using loudspeakers may cause acoustic feedback.

---

## Features

- Real-time live microphone processing
- Single-file HTML, CSS, and JavaScript implementation
- No build step required
- `AudioWorklet`-based DSP engine
- Eight-line feedback-delay network
- Infinite or near-infinite decay modes
- High-feedback reverb cloud generation
- Modulated delay lines to reduce static resonances
- Granular pitch-shifting stage
- High-pass and low-pass filtering
- Dry/wet mix control
- Freeze / infinite hold mode
- Soft saturation and output limiting
- Stereo widening from distributed delay-line panning
- Real-time spectrum visualizer
- Preset system
- Secure-context microphone access
- Fully client-side audio processing

---

## Quick Start

1. Save the application as `index.html`.
2. Serve the file from a local web server.
3. Open the page in a supported browser.
4. Click **Enable Microphone**.
5. Grant microphone permission.
6. Adjust parameters or select a preset.

Example:

```bash
python3 -m http.server 8080
```

Then open:

```text
http://localhost:8080
```

---

## Running Locally

A local web server is required because microphone access and `AudioWorklet` generally require a secure context.

### Option 1: Python

```bash
python3 -m http.server 8080
```

Open:

```text
http://localhost:8080
```

### Option 2: Node.js

```bash
npx serve .
```

Open the URL printed in the terminal.

### Option 3: VS Code

Use a local server extension such as:

- Live Server
- Live Preview
- Python HTTP Server extension

---

## Deploying to GitHub Pages

This project is compatible with static hosting.

1. Push the repository to GitHub.
2. Open **Settings**.
3. Navigate to **Pages**.
4. Select **Deploy from a branch**.
5. Choose the `main` branch and the root directory.
6. Save the settings.
7. Open the generated GitHub Pages URL.

Because GitHub Pages uses HTTPS, microphone access should work in supported browsers.

Example repository structure:

```text
blackhole-infinite-reverb/
├── index.html
├── README.md
└── LICENSE
```

---

## Usage Notes

- Use headphones to reduce the risk of acoustic feedback.
- Start with a low output level.
- High feedback and infinite decay can produce sustained sound.
- Use the **Clear Reverb Buffer** control if low-frequency energy accumulates.
- The **Freeze / Infinite Hold** mode mutes new input and sustains the existing reverb tail.
- The **Infinite Decay** mode forces maximum feedback while still allowing new input.
- Pitch shifting is most effective with sustained input material.
- Percussive transients will dissolve rapidly into the reverb cloud.

---

## Controls

| Control | Description |
|---|---|
| Input Gain | Adjusts the level of the microphone signal entering the processor. |
| Mix | Balances the dry microphone signal and the processed reverb signal. |
| Output | Controls the final output level. |
| Decay / Infinite | Controls reverb tail length. At 100%, decay becomes infinite. |
| Feedback | Controls the amount of energy recirculated in the delay network. |
| Modulation Rate | Sets the speed of delay-line modulation. |
| Modulation Depth | Sets the amount of delay-line modulation in milliseconds. |
| Pitch Shift | Shifts the pitch of the signal entering the reverb network in semitones. |
| High-Pass | Removes low-frequency energy to prevent rumble and bass buildup. |
| Low-Pass | Darkens the reverb tail by reducing high-frequency content. |

### Transport and State Controls

| Control | Description |
|---|---|
| Power | Suspends or resumes the audio engine. |
| Freeze / Infinite Hold | Mutes input and sustains the current reverb cloud indefinitely. |
| Infinite Decay | Forces infinite feedback while continuing to accept input. |
| Mute Dry | Removes the dry microphone signal from the output. |
| Clear Reverb Buffer | Resets the delay network and removes accumulated energy. |

---

## Presets

| Preset | Character |
|---|---|
| Void Choir | Elevated pitch content, wide modulation, and a long spectral tail. |
| Sci-Fi Engine | High feedback, downward pitch bias, and dense modulation. |
| Meditation Sphere | Smooth, restrained, and slowly evolving ambient texture. |
| Pitch Abyss | Extreme downward pitch shift with maximum feedback. |
| Clean Infinite | Balanced infinite reverb with clearer high-frequency content. |

---

## Technical Architecture

The DSP engine is implemented inside an `AudioWorkletProcessor` to achieve low-latency sample-accurate processing.

High-level signal flow:

```text
Microphone Input
       |
       v
 Input Gain
       |
       +------------------+
       |                  |
       v                  v
 Dry Path          Pitch Shifter
                          |
                          v
              Feedback Delay Network
              |    |    |    |    |
              v    v    v    v    v
            Modulated Delay Lines
                          |
                          v
             High-Pass / Low-Pass Damping
                          |
                          v
                 Feedback Matrix
                          |
                          v
                Stereo Distribution
                          |
                          v
                  Soft Saturation
                          |
                          v
             Compressor / Limiter
                          |
                          v
                    Audio Output
```

### DSP Components

#### Feedback-Delay Network

The reverb core uses eight delay lines with different delay times. The delay outputs are processed through a feedback matrix and recirculated into the network.

This creates a dense, evolving reverberant field suitable for infinite or near-infinite decay.

#### Modulation

Each delay line is modulated independently. Modulation prevents metallic static resonances and produces a continuously shifting spectral texture.

#### Pitch Shifting

A granular pitch-shifting stage processes the input before it enters the reverb network. Pitch shifting contributes harmonic displacement and shimmer-like behavior.

#### Filtering

High-pass and low-pass filters are used inside the feedback path to control spectral buildup and damping.

- The high-pass filter reduces low-frequency accumulation.
- The low-pass filter controls the brightness of the reverb tail.

#### Saturation and Limiting

Soft saturation is used inside the feedback network to control extreme feedback behavior. A dynamics compressor is used at the output stage as a safety limiter.

---

## Browser Compatibility

| Browser | Support |
|---|---|
| Chrome | Supported |
| Edge | Supported |
| Firefox | Supported |
| Safari | Supported in recent versions |
| iOS Safari | Supported in recent versions |
| Android Chrome | Supported |

Required browser capabilities:

- `AudioContext`
- `AudioWorkletNode`
- `navigator.mediaDevices.getUserMedia`
- Secure context, such as `https://` or `http://localhost`

---

## Performance Notes

Performance depends on device capability, sample rate, buffer size, and browser implementation.

For best results:

- Close unnecessary browser tabs.
- Use a modern desktop browser.
- Avoid extremely high input gain.
- Reduce output level if saturation becomes excessive.
- Use the high-pass filter to reduce low-frequency energy.
- Clear the reverb buffer if the texture becomes overloaded.

High sample-rate audio interfaces may increase CPU and memory usage.

---

## Troubleshooting

| Issue | Possible Cause | Solution |
|---|---|---|
| No microphone prompt appears | Browser permission blocked or insecure context | Use HTTPS or localhost and allow microphone access. |
| No sound | Power suspended or output muted | Enable Power and increase Output. |
| Harsh feedback | Loudspeaker monitoring | Use headphones and reduce Input Gain. |
| Low-frequency rumble | Feedback buildup | Increase High-Pass frequency or clear the buffer. |
| Sound becomes too dense | Infinite feedback engaged | Reduce Feedback, Decay, or enable Clear Reverb Buffer. |
| Audio crackles | CPU overload or buffer underrun | Close other applications or use a desktop browser. |
| Safari does not start | Permissions or AudioWorklet issue | Use a recent Safari version and grant microphone permission. |
| Page fails when opened from disk | Secure context restriction | Serve the file through localhost or HTTPS. |

---

## Project Structure

```text
blackhole-infinite-reverb/
├── index.html      # Complete application
├── README.md       # Project documentation
└── LICENSE         # License text
```

The application is intentionally contained in a single HTML file for portability.

---

## Privacy

All audio processing occurs locally in the browser.

The application does not:

- Upload microphone audio to a server
- Store audio recordings by default
- Transmit user data to external services
- Require authentication
- Require network access after the page has loaded

---

## Limitations

- Pitch shifting is approximate and optimized for ambient texture rather than transparent pitch correction.
- Infinite feedback can generate sustained sound that requires manual clearing.
- Browser audio latency varies by platform and device.
- Mobile devices may have higher latency and reduced CPU headroom.
- Acoustic feedback may occur when using loudspeakers.

---

## Roadmap

Potential future enhancements:

- Preset persistence using `localStorage`
- MIDI control support
- WebM or WAV recording export
- Stereo input mode
- Additional pitch-shifting algorithms
- External parameter automation
- Resizable FFT visualizer
- Additional modulation shapes
- Multi-band damping controls
- Touch-optimized performance interface

---

## Contributing

Contributions are welcome.

Suggested contribution workflow:

1. Fork the repository.
2. Create a feature branch.
3. Make focused changes.
4. Test in at least one Chromium-based browser and one additional browser.
5. Submit a pull request with a clear description of the change.

Areas where contributions are especially welcome:

- DSP improvements
- Accessibility improvements
- Mobile interface refinements
- Preset design
- Documentation improvements
- Browser compatibility fixes

---

## License

Distributed under the MIT License. See `LICENSE` for more information.

If this project does not yet include a `LICENSE` file, add one before public distribution.

---

## Acknowledgements

This project is inspired by the general category of infinite reverb and shimmer processors. It is not affiliated with or endorsed by Eventide, Strymon, Valhalla DSP, or Zynaptiq. Product names mentioned for reference are trademarks of their respective owners.
