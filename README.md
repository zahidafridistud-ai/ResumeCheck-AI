# 📄 ATS Resume Checker

A Streamlit app that scores a resume out of 100 for ATS (Applicant Tracking System) friendliness and suggests concrete improvements, powered by Google Gemini Flash.

## Features

- Upload a resume as **PDF, DOCX or TXT**
- **Overall ATS score (0-100)** computed from five weighted categories:
  Keywords & Skills (25%), Experience & Impact (25%), Formatting & Structure (20%), Content & Clarity (15%), Contact & Sections (15%)
- Strengths, prioritized issues with fixes, and missing keywords
- Before/after **bullet point rewrites**
- Optional **job description** to tailor the keyword match
- Rule-based checks (email, phone, LinkedIn, section headings, word count)

## Project structure

```
app.py             # the Streamlit app
requirements.txt   # Python dependencies
README.md          # this file
```

## Run locally

1. Install Python 3.10+.
2. Get a free Gemini API key: https://aistudio.google.com/apikey
3. Install and run:

```bash
pip install -r requirements.txt

# Mac/Linux
export GEMINI_API_KEY="your-key"
# Windows PowerShell
$env:GEMINI_API_KEY="your-key"

streamlit run app.py
```

You can also paste the key into the sidebar instead of using an environment variable.

## Configuration

| Setting | Where | Default |
|---|---|---|
| `GEMINI_API_KEY` | Env var, Streamlit secret, or sidebar | none (required) |
| Model name | Sidebar, or `GEMINI_MODEL` env var | `gemini-3.5-flash` |

Google renames and retires models over time. If you get a "model not found" error, change the model name in the sidebar (for example `gemini-2.5-flash`) or check the current list at https://ai.google.dev/gemini-api/docs/models.

## Push to GitHub (using the website, no commands)

1. Sign in at https://github.com and click **+** (top right) > **New repository**.
2. Name it `ats-resume-checker`, choose **Public** (the free Streamlit Community Cloud works best with public repos), and click **Create repository**.
3. On the new repo page, click **uploading an existing file**.
4. Drag and drop `app.py`, `requirements.txt` and `README.md` into the page.
5. Type a commit message such as `Initial commit` and click **Commit changes**.

> Never upload your API key to GitHub. Keep it in Streamlit secrets (below).

## Deploy on Streamlit Community Cloud

1. Go to https://share.streamlit.io and sign in with GitHub (authorize access when asked).
2. Click **Create app** (or **New app**) and choose to deploy from an existing GitHub repo.
3. Select your repository, the `main` branch, and set **Main file path** to `app.py`.
4. Open **Advanced settings** > **Secrets** and paste:

   ```toml
   GEMINI_API_KEY = "your-gemini-api-key"
   ```

5. Click **Deploy**. The first build takes a few minutes; you then get a public `*.streamlit.app` link.

**Updating the app:** edit a file on GitHub (pencil icon > Commit changes). Streamlit redeploys automatically.
**Changing the key later:** app menu > Settings > Secrets.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Almost no text could be extracted" | The PDF is a scanned image. Export a text-based PDF or use DOCX. |
| "Gemini rejected the API key" | Re-copy the key; make sure it has no spaces or quotes inside. |
| 503 "high demand" / UNAVAILABLE | Temporary Google overload. The app retries automatically (waits 2s, 4s) and then tries backup models (`gemini-2.5-flash`, `gemini-3-flash-preview`). If it still fails, wait a minute and click Analyze again. |
| Rate limit / quota error | Wait a minute; the free tier has per-minute limits. |
| Model not found | Change the model name in the sidebar. |
| Build fails on Streamlit Cloud | Confirm `requirements.txt` is in the repo root and spelled correctly. |

## Privacy

The resume text is sent to the Google Gemini API for analysis. The app does not save uploaded files.

## Disclaimer

The score is an AI-assisted estimate of ATS-friendliness, not the output of any real employer's ATS. Use it as guidance for improving your resume.
