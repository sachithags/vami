# Run and deploy Vami

## Run locally

Install Node.js 20 or newer. From the project folder, create `.env` from the example only if you do not already have one, then add your own keys:

```powershell
if (!(Test-Path .env)) { Copy-Item .env.example .env }
```

Add your provider credentials to `.env`:

```dotenv
GNANI_API_KEY_ID=your_gnani_api_key_id
GROQ_API_KEY=your_groq_api_key
GEMINI_API_KEY=your_gemini_api_key
PORT=3000
```

Start the app and open `http://localhost:3000`:

```powershell
npm start
```

If that port is busy, change `PORT` in `.env` and use the new port in the browser. Restart the server after changing keys. No packages need to be installed for local use. Keep `.env` private; `.gitignore` excludes it and local account data.

## Put the project on GitHub

Create an empty public repository or use the repository you already made. From the project folder, check that `.env` is ignored and not tracked:

```powershell
git check-ignore .env
git ls-files .env
```

The first command should print `.env`; the second should print nothing. Stage the project files and review the list before committing:

```powershell
git add .gitignore .env.example .vscode DEPLOYMENT.md README.md SAFETY_AND_OPERATIONS.md index.html package.json render.yaml server.mjs
git add -u
git status --short
git diff --cached --check
```

`git add -u` includes the removal of the old `HACKATHON_GUIDE.md`. Make sure `.env`, API keys, real documents, and `data/users.json` are not staged. Then commit and push:

```powershell
git commit -m "Polish Vami project and documentation"
git remote -v
git push -u origin main
```

If `origin` is missing, add it with `git remote add origin https://github.com/YOUR_ACCOUNT/vami.git`. If it points to the wrong repository, use `git remote set-url origin https://github.com/YOUR_ACCOUNT/vami.git`. Replace `YOUR_ACCOUNT` with your GitHub username. If a real key was ever pushed, revoke it with its provider before making the repo public.

## Deploy to Render

This project includes `render.yaml` for a Node web service. In Render, choose **New → Blueprint**, connect the GitHub repository, and deploy the `main` branch. When prompted, enter `GNANI_API_KEY_ID`, `GROQ_API_KEY`, and `GEMINI_API_KEY` as environment variables in Render. Do not put their values in GitHub or `render.yaml`.

Render provides an HTTPS address for the app. Check that address and `/api/health` after deployment. HTTPS is needed for microphone access on a phone. Pushing a later commit to the connected `main` branch triggers another deploy; you can keep changing the project after it is live.

The free service can sleep when idle, and its local filesystem is temporary. Vami's demo accounts and sessions are not durable, so do not use this setup for real customer data. Use a database and reviewed storage/privacy controls before a production launch.