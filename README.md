# Connect4 Vision Robot

A modular Connect 4 demonstration that combines computer vision, game logic, a minimax AI, and a robot manipulator to play discs on a physical board.

Features
- Real-time detection of red/yellow discs from a camera feed.
- Mapping of detected discs to a 6×7 logical Connect 4 board.
- Gravity-aware move detection to reduce false positives.
- Configurable AI difficulty (Easy / Medium / Hard) using minimax with alpha–beta pruning.
- Robot control for physical pick-and-place using `pyniryo` (mock fallback available).
- Simple fullscreen dashboard for configuration.

Repository structure (key folders)
- `main.py` — Orchestrator: camera loop, move detection, AI integration, robot calls.
- `vision/` — Camera, color detection, ROI selection, mapping and move detection.
- `game_logic/` — `Connect4Game` logic and `ai_strategy` (minimax & heuristic).
- `robot_control/` — Robot wrapper, pick/place sequences, pose tables and coin management.
- `ui/` — Tkinter dashboard for settings.
- `audio/` — Optional music/SFX and TTS helpers.

Requirements
- Python 3.8+ recommended
- Core (required): `numpy`, `opencv-python`, `pillow`
- Optional (for full experience): `pygame` (audio), `pyttsx3` (TTS), `pyniryo` (Niryo robot API)

Install (example)

```bash
python -m pip install --upgrade pip
pip install numpy opencv-python pillow
# Optional extras
pip install pygame pyttsx3 pyniryo
```

Setup
1. Camera & ROI (board grid):
   - Before running the full system, create and save the board ROI once so the pipeline knows where to crop frames.
   - Run the ROI selector:

```bash
python -m vision.detect_board
```

   - Use the interactive selector to draw the board rectangle and press a key to confirm. This saves `board_grid.npy` in the project folder.

2. Adjust camera index if needed: `main.py` uses `camera_id = 1` by default (external webcam). Change to `0` for built-in cameras.

Run

```bash
python main.py
```

Notes
- The UI dashboard will appear first (fullscreen). Choose player name, color, difficulty, and who starts.
- The code attempts to connect to a Niryo robot via `pyniryo`. If not available, a mock robot is used and actions are printed to the console so you can develop without hardware.
- If robot is physically configured to place a particular color (e.g., Red), the system prints a warning when human selects the opposite color. Physical robot behavior is determined by `robot_control/robot_positions.py` and may require per-setup calibration.

Tips for more robust detection
- If you see noisy masks, enable morphological cleaning in `vision/color_detection.py` (commented alternative) or tune HSV thresholds.
- For lighting variation, add an auto white-balance step or calibrate color thresholds per session.

Troubleshooting
- Camera not opening: verify camera index and that no other app is using it.
- `board_grid.npy` missing: re-run `python -m vision.detect_board` to recreate it.
- Robot connection fails: confirm network and `pyniryo` installation; the code falls back to a mock robot when the real robot is unreachable.

Development / Extending
- Replace the color-thresholding detector with a small object detector (MobileNet/YOLO) for more robust performance under varying light.
- Add temporal voting in move detection to tolerate partial occlusions.
- Add an automated calibration routine that lets the robot tune `DROP_POSES` for precise placement.

Acknowledgements
- Built as a modular demonstration combining OpenCV vision, classical game AI, and robot control sequences.

License
- (Add your preferred license here)
"# Connect4-Niryo-AI-Robot" 
