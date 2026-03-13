# VikWay Web MVP (FastAPI + React)

This MVP reuses the existing routing graph and weights from the current project (`vikway/export`).

## Structure

- `backend/` FastAPI API that loads `G.pkl` + `nodes.npy` and builds routes.
- `frontend/` React + Vite app with Leaflet map.

## Reused logic

- Nearest-node snapping from point to graph.
- Weighted shortest paths with `networkx.shortest_path`.
- Modes backed by existing graph weight keys (`w_short`, `w_quiet`, `w_green`, `w_accessible`).
- Polyline reconstruction from edge geometry.
- Route length and ETA calculation.

## Backend setup

1. Create and activate virtual environment.
2. Install dependencies:

```powershell
cd backend
pip install -r requirements.txt
```

3. Optional: set export directory explicitly (if autodetect fails):

```powershell
$env:VIKWAY_EXPORT_DIR = "C:\Users\chekalina\Documents\New project\vikway\export"
```

4. Run API:

```powershell
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

## Frontend setup

1. Install dependencies:

```powershell
cd frontend
npm install
```

2. Copy env file and run dev server:

```powershell
copy .env.example .env
npm run dev
```

By default frontend expects API on `http://127.0.0.1:8000`.

## API endpoints

- `GET /api/health`
- `GET /api/meta`
- `POST /api/routes`

Example request:

```json
{
  "start": { "lat": 59.41, "lon": 56.79 },
  "end": { "lat": 59.40, "lon": 56.83 },
  "mode": "green",
  "include_alternatives": true
}
```

## Notes

- `balanced` mode maps to available balanced-like weight key in this order: `w_accessible`, `w_balanced`, `weight`, `w_short`.
- Proxy metrics (`avg_noise`, `avg_green`) are best-effort and depend on available edge attributes in your graph.
- No auth, DB, Docker, or enterprise layers are added by design.
