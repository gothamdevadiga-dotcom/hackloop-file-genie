# File Genie Backend

This folder contains the Python API used by the File Genie desktop application. The current backend only provides a health check; document processing, file organization, and search are not implemented yet.

## Run on Windows

Open PowerShell in the repository folder, then run:

```powershell
cd backend
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

If PowerShell blocks activation, you can skip activation and run the environment's Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

The API health check is at <http://127.0.0.1:8000/api/health>. Interactive API documentation is at <http://127.0.0.1:8000/docs>.

## Frontend connection

The frontend can send HTTP requests to `http://127.0.0.1:8000`. CORS allows the common local development origins on ports 3000 and 5173. To use other origins, set `FILE_GENIE_FRONTEND_ORIGINS` to a comma-separated list before starting the backend. For example:

```powershell
$env:FILE_GENIE_FRONTEND_ORIGINS = "http://localhost:3001"
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Electron must launch the backend process when the desktop app starts and stop it when the app exits. A packaged app will also need a deliberate plan for shipping and launching Python. Prefer serving the Electron UI from a dedicated app protocol and allowing that exact origin; do not broadly allow arbitrary origins just to make CORS work.