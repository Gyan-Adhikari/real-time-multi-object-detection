# Neural Vision — Multi-Object Detection

Django + Ultralytics YOLO project for image, video, and live-camera object detection.

## Added feature: object counting

- **Image mode:** shows the total number of detected objects and a count for each detected class (for example, person: 3, car: 2).
- **Video mode:** shows total detection instances across all processed frames, plus a per-class count across the video. These are frame-by-frame detections, **not unique tracked objects**; the same object can be counted in multiple frames.
- Existing bounding-box results and confidence values are retained.

## Setup on Ubuntu / Linux

Run these commands in a terminal. If you extracted the ZIP into `~/Downloads/multi-object-detection`, use that path; otherwise `cd` into the extracted `website` directory.

```bash
cd ~/Downloads/multi-object-detection/website
```

If your extracted folder has a different name, adjust the path. The `manage.py` file must be in the current directory.

### 1. Install system packages

```bash
sudo apt update
sudo apt install -y python3 python3-venv python3-pip ffmpeg libgl1 libglib2.0-0
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip setuptools wheel
```

### 3. Install Python dependencies

```bash
pip install -r requirements.txt
```

PyTorch/Ultralytics may take some time to download. CPU inference works without a CUDA GPU; a compatible NVIDIA/CUDA setup is optional.

### 4. Prepare the Django database

```bash
python manage.py migrate
```

### 5. Start the development server

```bash
python manage.py runserver
```

Open **http://127.0.0.1:8000/** in your browser. Use the site's navigation to open the detection page, then choose Photo, Video, or Live Camera.

## How to use object counting

1. Open the detection page.
2. In **Photo**, select a JPG/PNG/WEBP image and click **RUN YOLO**. The results show the total objects and counts by class.
3. In **Video**, select a supported video and run detection. The result shows the number of processed frames and total per-frame detections by class.
4. In **Live Camera**, allow camera access if your browser asks. The camera is accessed by the computer running Django; if using a remote server, its webcam is not automatically your laptop webcam.

## Troubleshooting

- **`No module named django` / `ultralytics`:** activate the environment (`source .venv/bin/activate`) and run `pip install -r requirements.txt`.
- **Model file error:** check that `yolo26n.pt` exists next to `manage.py`. This project includes that model file.
- **Camera does not open:** close other apps using the webcam and check Linux camera permissions. Live camera uses camera index `0`.
- **OpenCV shared-library errors:** run `sudo apt install -y libgl1 libglib2.0-0`.
- **Port 8000 is busy:** run `python manage.py runserver 8001` and open `http://127.0.0.1:8001/`.
- **Very slow detection:** CPU inference can be slow. Try a smaller test image/video or use a supported GPU environment.

## Important notes

- `runserver` is for local development, not production deployment.
- Do not expose Django's development server to the public internet. Before deployment, configure a secure `SECRET_KEY`, `DEBUG=False`, `ALLOWED_HOSTS`, HTTPS, and production static/media serving.
- Uploaded media and generated results are saved under the project's `media/` directory.
- The included model's classes and detection quality depend on the model itself and its confidence threshold.

## Project structure

```text
website/
├── manage.py
├── requirements.txt
├── yolo26n.pt
├── website/       # Django settings and URL configuration
└── detection/     # Detection logic, views, templates, static assets
```
