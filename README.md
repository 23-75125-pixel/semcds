# SEMCDS

Smart Exam Management & Cheating Detection System prototype built with Flask.

## Run

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python main.py
```

To run it with the Flask CLI instead, use:

```powershell
flask --app wsgi run --debug
```

## Supabase Setup

1. Create a Supabase project.
2. Run [database/supabase_schema.sql](database/supabase_schema.sql) in the SQL editor.
3. Set `SUPABASE_URL` and `SUPABASE_SERVICE_ROLE_KEY` in your `.env` file.
4. Restart the Flask app.

## Render Deploy

1. Push the repo to GitHub.
2. In Render, create a new Blueprint service from this repo.
3. Render will pick up [render.yaml](render.yaml) and create a Python web service.
4. In Render, fill in the missing environment variables:
   - `SUPABASE_URL`
   - `SUPABASE_SERVICE_ROLE_KEY`
   - `GOOGLE_CLIENT_ID`
   - `GOOGLE_CLIENT_SECRET`
   - `GOOGLE_REDIRECT_URI`
   - `SMTP_HOST`
   - `SMTP_USER`
   - `SMTP_PASSWORD`
   - `SMTP_FROM`
   - `OPENAI_API_KEY` and/or `GEMINI_API_KEY` if you use AI quiz generation
5. Restart the service after saving the environment variables.

The Render blueprint is configured for a single instance with `eventlet` because live quiz monitoring uses Socket.IO and in-memory room state.

## Login

- Instructor: `/signin/admin`
- Student: `/signin/user`

## Notes

- The app uses a Flask application factory in `src/app.py`.
- When Supabase credentials are present, the app reads and writes quiz data through Supabase.
- New databases no longer seed default users. Create your first instructor account manually.
- Base44 login remains a placeholder; the AI question generation and quiz workflow are wired into the Flask routes.
