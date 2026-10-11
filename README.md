# TomorrowLifeReport Web + API

Responsive Flask dashboard for daily time tracking, a 7-day report, and a JSON API.

## Deploy to Render using the existing RAM repository
Upload these files/folders to the **repository root** (same level as the existing `build.gradle` and `settings.gradle`):
- `app.py`
- `requirements.txt`
- `render.yaml`
- `templates/` folder
- `static/` folder

Keep the existing Android `app/` folder and Gradle files; `app.py` is a separate file and does not replace the Android module.

In Render, configure the service's **Root Directory** as blank or `.` (repository root), runtime Python, build command `pip install -r requirements.txt`, and start command `gunicorn app:app`. Set a strong `APP_PASSWORD`; `SECRET_KEY` should be a long random secret. `render.yaml` provides the same settings if deploying as a Blueprint.

## API
- `GET /api/health`
- `GET /api/entries?date=YYYY-MM-DD` (login required)
- `POST /api/entries` (login required; JSON body with `entry_date`, `category`, `title`, `planned_minutes`, `actual_minutes`, optional `notes`)
- `DELETE /api/entries/<id>` (login required)
- `GET /api/reports/weekly?end=YYYY-MM-DD` (login required)

## Important data persistence note
SQLite is used locally. For production, configure a managed PostgreSQL database and set `DATABASE_URL` to its internal connection URL; Render web service filesystems are ephemeral, so SQLite data may be lost across deploys/restarts. Check current database plan/expiry terms before storing important records. This starter uses one shared password, not individual user accounts.
