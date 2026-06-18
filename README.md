<div align="center">

  <h1>Gesture-Controlled Cigarette Simulator</h1>

  <p>
    <b>A camera-driven desktop simulation that connects real-time hand tracking to an SDL2 burn animation.</b>
  </p>

  <p>
    <img alt="C++" src="https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white" />
    <img alt="SDL2" src="https://img.shields.io/badge/SDL2-173B6B?style=for-the-badge&logo=libsdl&logoColor=white" />
    <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
    <img alt="OpenCV" src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" />
    <img alt="MediaPipe" src="https://img.shields.io/badge/MediaPipe-FF6F00?style=for-the-badge" />
  </p>

  <p>
    <a href="#features">
      <img alt="Features" src="https://img.shields.io/badge/Features-00C2A8?style=for-the-badge" />
    </a>
    <a href="#demo-video">
      <img alt="Demo video" src="https://img.shields.io/badge/Demo_Video-FF3864?style=for-the-badge" />
    </a>
    <a href="#how-it-works">
      <img alt="How it works" src="https://img.shields.io/badge/How_It_Works-7C3AED?style=for-the-badge" />
    </a>
    <a href="#why-i-built-this">
      <img alt="Why I built this" src="https://img.shields.io/badge/Why_I_Built_This-F59E0B?style=for-the-badge" />
    </a>
  </p>

</div>

---

## Overview

**Gesture-Controlled Cigarette Simulator** is a two-process desktop experiment:

| Side | Responsibility |
| --- | --- |
| `app.py` | Uses the webcam, OpenCV, and MediaPipe Hands to detect whether the hand is open or closed. |
| `main.cpp` / `headers.h` | Runs the SDL2 interface, cigarette selection screen, burn animation, flame sprite sheet, and texture clipping. |
| `state.txt` | Acts as a tiny bridge between Python and C++: `1` means open hand, `0` means closed hand. |

The result is an interactive visual prototype where opening your hand advances the burn animation, while the SDL window handles the cigarette selection, rendering, and reset flow.

> This is a computer-vision and animation experiment. It is not an endorsement of smoking.

## Demo Video

> i'll manage to find time to record a demo of me using it  :p


## Features

<table>
  <tr>
    <td><b>Camera gesture input</b></td>
    <td>Tracks hand landmarks through OpenCV and MediaPipe.</td>
  </tr>
  <tr>
    <td><b>Open-hand detection</b></td>
    <td>Classifies the hand using fingertip and thumb landmark positions.</td>
  </tr>
  <tr>
    <td><b>Python to C++ bridge</b></td>
    <td>Shares gesture state through <code>state.txt</code> for a lightweight prototype IPC path.</td>
  </tr>
  <tr>
    <td><b>SDL2 selection menu</b></td>
    <td>Lets the user choose between multiple cigarette/cigar assets before starting.</td>
  </tr>
  <tr>
    <td><b>Animated flame</b></td>
    <td>Uses a <code>6x6</code> flame sprite sheet rendered frame by frame.</td>
  </tr>
  <tr>
    <td><b>Progressive burn effect</b></td>
    <td>Reveals smoked/ash textures with SDL clipping rectangles as the burn advances.</td>
  </tr>
  <tr>
    <td><b>Manual fallback</b></td>
    <td>Press <code>I</code> to advance the burn without camera input.</td>
  </tr>
</table>

## Tech Stack

<p>
  <img alt="C++17" src="https://img.shields.io/badge/C++17-0B5FFF?style=flat-square&logo=cplusplus&logoColor=white" />
  <img alt="SDL2" src="https://img.shields.io/badge/SDL2-1F6FEB?style=flat-square&logo=libsdl&logoColor=white" />
  <img alt="SDL2 image" src="https://img.shields.io/badge/SDL2_image-FF7A18?style=flat-square&logo=image&logoColor=white" />
  <img alt="SDL2 ttf" src="https://img.shields.io/badge/SDL2_ttf-9B5DE5?style=flat-square" />
  <img alt="SDL2 gfx" src="https://img.shields.io/badge/SDL2_gfx-06B6D4?style=flat-square" />
  <img alt="Python" src="https://img.shields.io/badge/Python-2563EB?style=flat-square&logo=python&logoColor=white" />
  <img alt="OpenCV" src="https://img.shields.io/badge/OpenCV-22C55E?style=flat-square&logo=opencv&logoColor=white" />
  <img alt="MediaPipe" src="https://img.shields.io/badge/MediaPipe-F97316?style=flat-square" />
</p>

| Layer | Technology | Role |
| --- | --- | --- |
| Simulation | `C++17` | Main app loop, UI state, animation state, and rendering logic. |
| Rendering | `SDL2` | Window, renderer, input events, and frame presentation. |
| Assets | `SDL2_image` | Loads PNG cigarette, ash, and flame textures. |
| Text | `SDL2_ttf` | Loads local fonts and renders menu/status text. |
| Extra SDL drawing | `SDL2_gfx` | Linked as part of the SDL rendering stack. |
| Gesture detection | `Python`, `OpenCV`, `MediaPipe` | Captures webcam frames and detects hand landmarks. |
| IPC prototype | `state.txt` | Simple file polling between the Python detector and C++ renderer. |

## Project Structure

```text
.
|-- app.py              # Webcam hand detector, open/closed classifier, state writer
|-- main.cpp            # SDL app entry point and render loop
|-- headers.h           # SDL wrapper, UI, assets, gesture polling, burn animation
|-- state.txt           # Runtime bridge: 1=open hand, 0=closed hand
|-- font.ttf            # UI font
|-- font2.ttf           # UI font
|-- font.otf            # UI font
|-- assets/
|   |-- marl_show.png   # Menu preview asset
|   |-- oris_show.png   # Menu preview asset
|   |-- cuba_show.png   # Menu preview asset
|   |-- marlboro.png    # Simulation texture
|   |-- oris.png        # Simulation texture
|   |-- cuba.png        # Simulation texture
|   |-- marl_smo.png    # Smoked overlay texture
|   |-- cuba_smo.png    # Smoked overlay texture
|   `-- fire.png        # 6x6 flame sprite sheet
`-- output/main         # Existing compiled binary artifact
```

## Setup

### Python Gesture Detector

Create and activate a Python environment:

```bash
python3 -m venv venv311
source venv311/bin/activate
```

Install the detector dependencies:

```bash
pip install opencv-python mediapipe
```

### C++ SDL App

Install SDL dependencies on Ubuntu/Debian:

```bash
sudo apt install build-essential libsdl2-dev libsdl2-image-dev libsdl2-ttf-dev libsdl2-gfx-dev
```

Compile from the repository root:

```bash
g++ main.cpp -o cigarette-simulator \
  -lSDL2 -lSDL2_image -lSDL2_ttf -lSDL2_gfx -std=c++17
```

## Run

Start the gesture detector in one terminal:

```bash
python app.py
```

Start the SDL simulator in another terminal:

```bash
./cigarette-simulator
```

Run both commands from the repository root so `state.txt`, fonts, and `assets/` resolve correctly.

## Controls

| Input | Action |
| --- | --- |
| Mouse click on a menu option | Select the cigarette/cigar type. |
| `smoke !` button | Start the simulation. |
| Open hand | Advance the burn while the detector is running. |
| `I` | Manually advance the burn. |
| `Esc` | Return to the selection menu and reset the burn. |
| Window close | Quit the SDL app. |
| `q` in the camera window | Quit the Python detector. |

## Gesture State

`app.py` writes one of two values to `state.txt`:

| Value | Meaning |
| --- | --- |
| `1` | Open hand detected. |
| `0` | Closed hand or no open-hand state detected. |

The C++ app reads this file inside `uinter::update()`. When the simulation is burning and the state is `1`, the burn position advances every `150ms`.

## How It Works

### 1. Hand Tracking

The Python detector opens the webcam with OpenCV, converts frames from BGR to RGB, and sends them to MediaPipe Hands.

The open-hand classifier checks the finger landmarks:

| Landmark check | Purpose |
| --- | --- |
| Fingertips above lower finger joints | Counts extended fingers. |
| Thumb x-position against its previous joint | Estimates whether the thumb is open. |
| At least 3 extended fingers plus open thumb | Writes `1` to `state.txt`. |

### 2. SDL Menu

The C++ app starts in menu mode. It renders three visual choices from the `assets/` folder and a `smoke !` button. Clicking a choice stores the selected asset set, then the button switches the app into simulation mode.

### 3. Burn Animation

The simulation is layered from:

1. A base cigarette/cigar texture.
2. A smoked overlay texture.
3. A flame animation from `assets/fire.png`.

The variable `fy` controls the flame position and the clipping rectangle used to reveal more of the smoked overlay. As `fy` increases, the flame moves and more ash appears.

### 4. Flame Sprite Sheet

`assets/fire.png` is treated as a `6x6` grid. The renderer advances through `36` frames with a `100ms` delay, then loops back to the first frame.

## Why I Built This

> Write your personal note here.

Suggested direction:

```md
I built this to connect computer vision with a real-time SDL animation instead of keeping gesture
detection as a separate boring demo. The goal was to make the camera input feel physical: open the
hand, watch the simulation react, and learn how Python and C++ can talk in a simple prototype.
```

## Current Status

| Area | Status |
| --- | --- |
| SDL menu | Working |
| Asset selection | Working |
| Flame sprite animation | Working |
| Burn/ash reveal | Working |
| Manual keyboard fallback | Working |
| MediaPipe hand detector | Working, camera/lighting dependent |
| Python-to-C++ bridge | Working through `state.txt` polling |

## Known Limitations

| Limitation | Current state |
| --- | --- |
| IPC | File polling works for a prototype but is less robust than sockets, pipes, or shared memory. |
| Camera input | Recognition depends on lighting, camera quality, and hand position. |
| Launch flow | The detector and SDL app are started separately. |
| Layout | Window size and UI positions are fixed around `1280x720`. |
| Platform setup | SDL and MediaPipe installation may vary by operating system. |

## Roadmap Ideas

| Idea | Why it would help |
| --- | --- |
| Start detector from the SDL app | Make the app feel like one unified program. |
| Replace file polling | Use sockets, named pipes, or shared memory for cleaner communication. |
| Add calibration | Improve recognition for different hands, camera angles, and lighting. |
| Add visual camera status | Show whether the detector is connected and what state it is writing. |
| Add more animation states | Smoke particles, heat glow, or a stronger end-of-burn transition. |

---

<div align="center">
  <sub>Built with C++ SDL2 rendering, Python hand tracking, and a tiny file-based bridge.</sub>
</div>
