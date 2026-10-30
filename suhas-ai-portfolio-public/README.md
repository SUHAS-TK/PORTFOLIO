# Suhas AI Portfolio

A Flask-hosted personal portfolio with an AI chat assistant, optional PDF resume upload, and an email-draft/review/send workflow.

## Security first

- **Do not upload `.env`**. It is intentionally excluded by `.gitignore`.
- Never put Groq or Gmail credentials in `app.py`, `index.html`, GitHub, screenshots, or chat messages.
- For local development, copy `.env.example` to `.env` and add your own values there.
- For public hosting, add secrets in the hosting provider's Environment Variables / Secrets settings.
- Anyone with access to the hosting service's settings may be able to manage or view configured secrets. Public visitors to the website do not receive environment variables through this app.
- If a key was ever committed or shared, revoke/rotate it immediately; deleting it from the latest commit is not enough.

## Run locally (Windows PowerShell)

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
Copy-Item .env.example .env
notepad .env
.\.venv\Scripts\python.exe app.py
```

Add your Groq key to `.env` for AI replies. Gmail settings are optional and are needed only if you want the chatbot's send-email feature. Open http://127.0.0.1:5000.

## Deploy on Render

1. Push this folder to a **Public** GitHub repository. Confirm `.env` is not included in the commit.
2. In Render, create **New → Web Service** and connect the repository.
3. Build command: `pip install -r requirements.txt`
4. Start command: `gunicorn app:app`
5. In Render → Environment, add `GROQ_API_KEY` and `GROQ_MODEL`. Add `GMAIL_ADDRESS` and `GMAIL_APP_PASSWORD` only if email sending is required.
6. Deploy. Render provides the public HTTPS URL.

The Flask app binds to `0.0.0.0` and uses the `PORT` environment variable for hosted deployment.

## Email behavior

When a visitor requests an email, the assistant creates a draft and asks for the recipient's email address. The visitor can edit the subject and body. The app sends only after the visitor clicks **Send email**. Configure a Gmail App Password (not your normal Gmail password) if enabling Gmail SMTP.

## Notes

- GitHub Pages cannot run this Flask backend. Use a Python web host such as Render.
- The Groq API key is required for AI chat. The portfolio page itself can load without it.
- Uploaded PDF resume text is processed in memory for the chat request; this app does not save the uploaded PDF to disk.
