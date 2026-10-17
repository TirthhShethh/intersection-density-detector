# Intelligent Polygon-based Intersection Density Detection

A computer-vision prototype that counts vehicles per lane at a road intersection using
YOLO object detection, and computes adaptive signal timing from live density data.
Users draw polygon detection zones over an intersection camera feed; the backend
tracks vehicles inside each zone and feeds per-lane counts into a decision engine.

## How It Works

1. **Zone definition** — the user draws polygon regions over the intersection view in the frontend (`polygon.js`)
2. **Detection** — each video frame is passed through a YOLO model (`detector/yolo_detector.py`, weights in `best.pt`)
3. **Tracking** — detected boxes are assigned stable IDs per zone using an IOU-based tracker (`detector/tracker.py`)
4. **Density decision** — per-lane vehicle counts are converted into signal-phase durations (`detector/decision.py`)
5. **API + UI** — Django REST views (`traffic_api/views.py`) serve counts and decisions to the dashboard in real time

## Tech Stack

| Layer    | Technology                          |
|----------|-------------------------------------|
| Detection| YOLO (Ultralytics), OpenCV          |
| Backend  | Django, Django REST, SQLite         |
| Frontend | Vanilla HTML/CSS/JS                 |
| Model    | Custom-trained YOLO (`backend/best.pt`) |

## Project Structure

```
backend/
├── detector/          # core CV pipeline
│   ├── yolo_detector.py   # YOLO inference
│   ├── tracker.py         # object ID tracking per polygon zone
│   └── decision.py        # density → signal timing logic
├── traffic_api/       # Django project (settings, urls, views)
├── data/              # sample videos / frames
├── docs/              # design notes
├── best.pt            # trained YOLO weights
└── manage.py
frontend/
├── index.html         # dashboard
├── polygon.js         # polygon zone drawing
├── script.js          # live count / signal UI
└── style.css
```

## Getting Started

```bash
cd backend
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

Then open `frontend/index.html` in a browser, draw zones over the intersection feed,
and start detection.

&gt; Note: `Py12requirements.txt` exists alongside `requirements.txt` — merge/delete one before publishing.

## Model

- Base model: YOLO [v8n / v8s — fill in which one]
- Dataset: [dataset name, e.g., annotated frames from X videos / public dataset]
- Classes: [e.g., car, bike, bus, truck, auto]
- Metrics: mAP@[0.5] = [X.XX], precision = [X.XX], recall = [X.XX]  ← from your training run

## Results

[Add: a screenshot or GIF of the dashboard with polygons + live counts,
and 1–2 sentences, e.g. "Counts from the CV pipeline matched manual counts on
N minutes of footage with ~X% average error."]

## Limitations & Next Steps

- [e.g., tracking drifts under heavy occlusion]
- [e.g., no persistence of historical density data]
- Planned: [retrian on more data / move to DeepSORT / deploy API to cloud]

## Author

Tirth Sheth — [LinkedIn] · [portfolio]
