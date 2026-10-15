# Copilot instructions for this repository

## Project overview

This repository contains a small FastAPI app for Mergington High School. The app lets students browse extracurricular activities and sign up by email. The main implementation lives in `src/app.py`; the web UI is plain HTML/CSS/JavaScript served from `src/static/`.

At a high level:
- `src/app.py` defines the FastAPI app and the in-memory activity store.
- `src/static/` contains the browser UI (`index.html`, `app.js`, `styles.css`).
- The root route redirects to `/static/index.html` so the frontend is served by the API.
- Data is intentionally in-memory only; activity state resets when the process restarts.
- The core API contract is:
  - `GET /activities`
  - `POST /activities/{activity_name}/signup?email=user@mergington.edu`

## Build, test, and validation commands

### Setup

```bash
python -m pip install -r requirements.txt
```

### Run the app

From the repository root:

```bash
uvicorn src.app:app --reload
```

You can also run it directly from the source directory:

```bash
cd src && python app.py
```

Then browse to:
- `http://localhost:8000/` for the UI
- `http://localhost:8000/docs` for the FastAPI Swagger docs

### Test commands

This repository currently does not include a test suite in the repo root, but `pytest.ini` is configured and the project expects pytest-based tests to be added under a `tests/` directory. When tests are present, use:

```bash
python -m pytest -q
```

Run a single file:

```bash
python -m pytest tests/test_app.py -q
```

Run a single test by name:

```bash
python -m pytest tests/test_app.py -k signup -q
```

There is no dedicated lint configuration in this repo; the project relies on Python syntax and pytest validation unless a task introduces project-specific lint tooling.

## Key conventions and repository-specific patterns

- Keep the API implementation in `src/app.py`; do not move business logic into the static frontend.
- Use the in-memory `activities` dictionary as the source of truth; if you add or change data, keep the structure consistent with the existing schema:
  - `description`
  - `schedule`
  - `max_participants`
  - `participants`
- Preserve the existing FastAPI route style and response patterns. Request validation should remain explicit and local to the route handler.
- The frontend and backend are intentionally simple; avoid introducing a framework, database, or app structure unless the task explicitly requires it.
- `pytest.ini` sets `pythonpath = .`, so tests can import project modules from the repo root without additional installation steps.
- When changing signup behavior, keep the existing semantics in mind: activity lookup should fail with a 404 if the activity does not exist, and duplicate signups should fail with a 400.
- This repository is instructional and lightweight; prefer minimal, direct code changes over broad refactors.
