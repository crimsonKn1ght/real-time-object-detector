# Real-Time Object Detection

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat)](LICENSE)
[![YOLO11](https://img.shields.io/badge/Ultralytics-YOLO11n-blue.svg?style=flat)](https://docs.ultralytics.com/models/yolo11/)
[![TensorFlow.js](https://img.shields.io/badge/TensorFlow.js-COCO--SSD-ff6f00.svg?style=flat)](https://github.com/tensorflow/tfjs-models/tree/master/coco-ssd)

Two webcam detectors in one repository.

- **`rock-paper-scissors/`** is a custom-trained YOLO11n detector for rock-paper-scissors
  hand shapes, running locally through OpenCV. The trained weights are committed as
  `game_weights.pt`.
- **`standalone-detector/`** is a general-purpose detector that runs entirely in the
  browser with TensorFlow.js and the COCO-SSD model. No server, no install.

## Rock-paper-scissors detector

Install the dependencies:

```bash
pip install ultralytics opencv-python
```

`ultralytics` pulls in PyTorch. `config.py` selects CUDA automatically when it is
available and falls back to CPU otherwise.

Run it from inside the project directory, since the script imports `config.py` and
resolves the weights relative to the working directory:

```bash
cd rock-paper-scissors
python live-time-detector.py
```

A window opens on the default camera. The most confident detection is printed to stdout
whenever the predicted label changes. Press `q` or `Esc` to quit.

Detection thresholds live in `config.py`:

| Setting | Default | Meaning |
|---|---|---|
| `conf_thres` | `0.5` | Minimum confidence for a box to count |
| `imgsz` | `640` | Inference resolution |
| `device` | auto | CUDA device `0` when available, else CPU |

## Standalone browser detector

Open `standalone-detector/standalone-detector.html` in a modern browser. The page loads
COCO-SSD from a CDN and asks for camera permission, so it needs a network connection on
first load but no local setup.

COCO-SSD covers the 80 COCO classes: people, animals, vehicles, household objects, food
items and so on.

## Privacy

- All processing happens locally in the browser or on your own machine
- No frames are sent to a server
- Nothing is recorded or stored

## Technical details

**Rock-paper-scissors**
- Model: YOLO11n (nano variant)
- Framework: Ultralytics YOLO, PyTorch backend
- Weights: custom-trained, committed as `game_weights.pt`
- Capture: OpenCV `VideoCapture`

**Standalone detector**
- Model: COCO-SSD (Single Shot MultiBox Detector)
- Framework: TensorFlow.js
- Styling: Tailwind CSS
- Rendering: `requestAnimationFrame` draw loop

## Repository layout

```
├── rock-paper-scissors/
│   ├── live-time-detector.py     # Webcam loop and annotation
│   ├── config.py                 # Weights path, thresholds, device selection
│   └── game_weights.pt           # Custom-trained YOLO11n weights
├── standalone-detector/
│   └── standalone-detector.html  # Self-contained browser detector
├── README.md
└── LICENSE
```

## Dataset

**Dataset:** Rock-Paper-Scissors-SXSW
**Publisher:** Roboflow (via Universe)
**URL:** https://universe.roboflow.com/roboflow-58fyf/rock-paper-scissors-sxsw/dataset/14
**Accessed:** 10-08-2025

## Troubleshooting

**Camera not working**
- Check that camera permissions are granted
- Browsers require HTTPS or `localhost` for camera access
- Confirm no other application is holding the camera

**Poor detection quality**
- Improve lighting
- Keep the subject at a moderate distance from the camera
- Lower `conf_thres` in `config.py` to surface weaker detections

**Model slow to load**
- The browser detector downloads COCO-SSD on first load, which can take up to about
  30 seconds on a slow connection

## Contributing

Fork the project and open a pull request.

## License

MIT License. See [LICENSE](LICENSE) for details.
