# touch

A browser-based particle field controlled by hand gestures through your webcam.

🔗 **Live:** [touchfield.space](https://touchfield.space/)

## Interaction

| Gesture                                  | Effect                                       |
| ---------------------------------------- | -------------------------------------------- |
| Raise one hand                           | Fingertips push and scatter nearby particles |
| Raise both hands                         | Two sets of force points at once             |
| Touch fingertips together (left + right) | Generates a new field                        |

A 5-step onboarding walks first-time users through camera permission, model loading, and each gesture.

## How it works

- **Hand tracking:** TensorFlow.js hand-pose detection (MediaPipe Hands, up to 2 hands) returns 21 keypoints per hand each frame. Fingertip velocity is computed from the previous frame.
- **Flow field:** each particle's direction is a sum of layered sine/cosine waves over position, time, and a per-particle seed. Each new field randomizes wave scale, speed, and strength.
- **Physics:** per frame, each particle is moved by the flow field, pulled toward a home position, and repelled by moving fingertips. Force scales with fingertip speed and falls off with distance; damping and a speed cap keep motion stable.
- **Rendering:** ~4,800 sprites in PixiJS (WebGL) with additive blending. Particles brighten based on how much they've been disturbed.
- **Gesture detection:** a new field triggers when any left-hand fingertip comes within a set distance of a right-hand fingertip, with a cooldown.
- **Sound:** Web Audio API grains (filtered noise or decaying sine tones) with volume tied to hand speed.

## Tech stack

PixiJS v7 · TensorFlow.js · MediaPipe Hands · Web Audio API · HTML/CSS/JavaScript

## Run locally

```bash
git clone https://github.com/auueliu/touch.git
cd touch
python3 -m http.server 8000
# open http://localhost:8000
```

Camera access requires `localhost` or HTTPS. Best in Chrome on a laptop or desktop.

## Privacy

All camera processing runs locally in the browser. No video or tracking data is stored or sent anywhere.
