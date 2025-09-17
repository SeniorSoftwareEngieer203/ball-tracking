# Simple Hybrid Ball Tracker

This repository contains a small hybrid ball-tracking demo that uses YOLO (Ultralytics) as the primary detector and a simple frame-difference fallback to continue tracking when YOLO misses the ball.

Files

- `main.ipynb` - Main Jupyter notebook that implements the tracker and a runnable `main()` entry at the bottom.

Quick overview

- YOLO detects the ball when confident.
- When YOLO fails, a frame-difference blob tracker follows the last known position until YOLO reacquires the ball.

## Setup (Windows, PowerShell)

These instructions assume you have Python 3.8+ installed and are on Windows using PowerShell. Adjust commands for your OS/shell if needed.

1. Create and activate a virtual environment

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Upgrade pip and install dependencies

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Notes on PyTorch/YOLO

- The project uses `ultralytics` which requires a working PyTorch installation. On Windows you should follow the official PyTorch installation page to pick the build that matches your CUDA (or CPU-only) setup. Example CPU-only installation:

```powershell
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
pip install ultralytics opencv-python-headless numpy
```

Or, if you have CUDA (replace cu117 with your CUDA version):

```powershell
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu117
pip install ultralytics opencv-python-headless numpy
```

Project configuration

- Edit the top of `main.ipynb` to point to your video and model files. Relevant variables in the notebook:

- `VIDEO_PATH` - path to the input video (e.g. `"./input.MOV"`)
- `OUTPUT_PATH` - where the output video will be written (e.g. `"./output/trajectory_ball_tracking.mp4"`)
- `YOLO_MODEL_PATH` - path to a YOLO weights file (for example `yolov8x.pt` or a custom model)

Place your files in the repository (or provide absolute paths). Example structure:

```
./input.MOV
./yolov8x.pt
```

## Running the app

There are two common ways to run the tracker:

1. Run the Jupyter notebook (recommended for interactive tweaking)

```powershell
# Start Jupyter in this folder
jupyter notebook
# open main.ipynb in the browser and run the cells (or run all)
```

2. Run the notebook as a script (quick, non-interactive)

The top of `main.ipynb` contains a `main()` entrypoint and a `if __name__ == "__main__": main()` guard. To run it as a script, export the notebook to a .py file or run it with `nbconvert`:

```powershell
# Convert to script
jupyter nbconvert --to script main.ipynb --output main
# Run the generated main.py
python main.py
```

What to expect

- Progress prints to the console while processing.
- Output video will be written to the path set in `OUTPUT_PATH`.
- At the end the notebook prints a small tracking summary (frames tracked by YOLO vs frame-diff, lost frames, etc.).

Troubleshooting

- If YOLO model load fails, ensure `YOLO_MODEL_PATH` points to a valid `.pt` file and you have a compatible PyTorch build.
- If OpenCV errors appear when installing `opencv-python`, try `pip install opencv-python-headless` (used in requirements) or a platform wheel that matches your Python version.
- If GPU is available but not used, verify your PyTorch installation with `python -c "import torch; print(torch.cuda.is_available())"`.

Minimal changes you may want to tweak

- `CONFIDENCE_THRESHOLD` - YOLO detection confidence threshold.
- Frame-diff parameters: `DIFF_THRESHOLD`, `BLUR_KERNEL`, `MIN_BALL_AREA`, `MAX_BALL_AREA`.
- Tracking tolerances: `SEARCH_RADIUS_MULTIPLIER`, `SIZE_TOLERANCE`, `MAX_MOVEMENT_PER_FRAME`.

License / Attribution

- This is sample/demo code. No license file is included; add one if you plan to reuse or redistribute the code.

Contact / Next steps

- If you want, I can convert `main.ipynb` to a standalone `main.py` script, add argument parsing, or create a small test video and unit tests. Tell me which you'd prefer.

# ball-tracking
