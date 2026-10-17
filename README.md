# Bastet

> Named after Bastet, the Egyptian goddess of the home, usually depicted as a cat.

A personal desk assistant built on an ESP32 and a small OLED display. It started as a
Spotify "Now Playing" player with physical controls and is growing into a modular
assistant: music, timers, calendar and, eventually, voice.

![status](https://img.shields.io/badge/status-in%20development-yellow)
![platform](https://img.shields.io/badge/platform-ESP32-blue)
![license](https://img.shields.io/badge/license-MIT-green)

## Status

| Module | State |
|---|---|
| [Spotify player](https://github.com/im-katherine/esp32-spotify-oled-player) | Working (separate repository) |
| Menu navigation | Planned |
| Timer | Planned |
| Google Calendar | Planned (design still open) |
| Voice control | Planned |
| 3D-printed enclosure | Planned |

This repository is the home of the assistant itself. The modules were born as
independent projects, and each one is brought in as it is integrated.

## Motivation

I started by testing a module that could change the song playing on Spotify. Once the
ESP32 could read and control the current track, the next question was obvious: what else
can it reach? My calendar? A timer? Instead of building separate gadgets, I decided to
build a single assistant.

Voice interaction is the part I most want to learn: audio capture, speech recognition and
command parsing. The first versions are touch and button only, and voice comes later.

## Existing module: Spotify player

The first module is a standalone, working project: it shows the current track, artist and
progress, and controls playback with three buttons. Setup, wiring and code live in its own
repository:

**[esp32-spotify-oled-player](https://github.com/im-katherine/esp32-spotify-oled-player)**

## Planned architecture

```
ESP32 (OLED, buttons, microphone) ──WiFi──► Backend (Python)
  modules: spotify, timer, ...               ├─ Google Calendar (OAuth)
                                             └─ Speech-to-text and command parsing
```

- **On the device:** the menu, the timer and the Spotify module, so the basics work with
  nothing but WiFi.
- **On a small backend:** the parts that are heavy for a microcontroller, such as Google
  OAuth and speech recognition. The ESP32 only sends requests and receives compact JSON.
- **Voice:** push-to-talk first (hold a button and speak). A wake word is a later,
  hardware-dependent step.
- **Modules** share a small common interface, so a new one does not touch the others.

This is the intended design and may change as I learn what works on the hardware.

## Repository layout

```
bastet-personal-assistant/
├── firmware/   # ESP32 code (PlatformIO project)
├── backend/    # Python backend (planned)
├── hardware/   # wiring notes and enclosure files (planned)
└── docs/       # design notes
```

## Roadmap

**Phase 1: foundation**
- [x] Spotify now-playing display with playback controls (separate repository)
- [ ] Bring the Spotify code in as a module
- [ ] Menu navigation with the three buttons
- [ ] Timer module

**Phase 2: connected**
- [ ] Python backend
- [ ] Google Calendar: next events on the display

**Phase 3: voice**
- [ ] I2S microphone capture
- [ ] Push-to-talk streaming to the backend
- [ ] Voice commands for the existing modules
- [ ] Wake word (depends on hardware)

**Phase 4: hardware**
- [ ] Album art on the OLED (JPEG decoding and dithering to 1-bit)
- [ ] Deep sleep when playback is paused for a long time
- [ ] 3D-printed enclosure

## Hardware

| Component | Notes |
|---|---|
| ESP32 dev board | Tested on a Wemos Lolin32 |
| SSD1306 OLED | 128x32, I2C |
| 3 tactile buttons | Previous, Select, Next |
| Jumper wires and breadboard | For prototyping |

Pin assignments are documented in the Spotify player repository for now.

## License

MIT
