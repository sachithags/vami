# Vami setup and deployment

## What to install

- **Visual Studio Code** for editing the project.
- **Node.js 20 or newer** (npm is included with Node). Vami has no npm package dependencies, so `npm install` is not needed for local use.
- **Git** if you want to publish the project to GitHub and deploy it from a repository.
- A modern browser. Microphone and camera permissions are requested by the browser when you use those features.
- **API credentials:** a Gnani API key ID for Prisma and Timbre, a Groq API key for fast photo/text answers, and a Google Gemini API key for PDFs and fallback. Vami keeps all keys on the server.

Install Node.js and Git on any computer you use for development. VS Code is optional; the project also runs from a terminal.

## Open and run in VS Code

1. In VS Code choose **File → Open Folder…** and select this `Vami` folder.
2. Open the `.env` file in the project root. Paste your credentials after the equals signs, without quotes:

   ```dotenv
   GNANI_API_KEY_ID=your_gnani_api_key_id
   GROQ_API_KEY=your_groq_api_key
   GEMINI_API_KEY=your_gemini_api_key
   PORT=3001
   ```

   The `.env` file is excluded from Git. Keep it on your computer and never paste keys into `index.html`, source code, screenshots, or a public repository.
3. Press **F5** and choose **Run Vami (F5)** if prompted. Or open **Terminal → New Terminal** and run `npm start`.
4. Visit <http://localhost:3001> (or the port printed by the server). If you add keys while the server is running, stop it with **Ctrl+C** and start it again.

For fast image answers and a backup reader, create a Groq API key and set `GROQ_API_KEY`. Add `GEMINI_API_KEY` for PDFs and fallback. If you see **“Document reading is not configured yet”**, add at least one reasoning-provider key, then restart the server. Gnani speech is configured separately with `GNANI_API_KEY_ID`. Check `http://localhost:<PORT>/api/health` to see which providers are configured; it reports presence only and never returns a key.

## Open Vami on a phone

For the full experience, open the deployed Render `https://...onrender.com` address in the phone's browser. The **Upload** button opens the phone's file/photo picker; **Scan** opens the camera picker for a new photo. Approve microphone access when starting voice chat.

To preview from a computer on the same Wi-Fi, keep the server running, run `ipconfig` on Windows, find that computer's Wi-Fi **IPv4 Address**, and open `http://<IPv4-address>:<PORT>` on the phone. This is useful for layout, text, and photo-upload checks. Phone microphone capture requires a secure browser context; use the HTTPS deployment for voice. [The browser microphone API requires HTTPS or localhost](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia.).

## Deploy to Render

This project includes `render.yaml` for a Render **Web Service**. It runs a Node server and serves the UI and API together, so do not create a Static Site for this build.

1. On GitHub, create a new **public** repository named `vami`. Leave it empty: do not add a README, license, or `.gitignore` there. In VS Code, open the integrated terminal in this project folder. Confirm `.env` is ignored and is not tracked:

   ```powershell
   git check-ignore .env
   git ls-files .env
   ```

   The first command should print `.env`; the second should print nothing. Stage the project source, safe example config, and documentation. This allowlist leaves out `.env`, uploaded files, and the runtime account database:

   ```powershell
   git add .gitignore .env.example .vscode DEPLOYMENT.md HACKATHON_GUIDE.md README.md SAFETY_AND_OPERATIONS.md index.html package.json render.yaml server.mjs
   git status --short
   git diff --cached --check
   git commit -m "Prepare Vami for public demo"
   git remote add origin https://github.com/YOUR_ACCOUNT/vami.git
   git push -u origin main
   ```

   Replace `YOUR_ACCOUNT` with your GitHub username. Check the staged file list before committing; it should not contain `.env`, real documents or photos, or `data/users.json`. If Git asks, configure your author name/email and authenticate with GitHub. Keep `.env` out of screenshots and chat messages too. If a real API key has ever been committed or pushed, revoke it with its provider and create a replacement before deploying.
2. In Render, choose **New → Blueprint**, connect the GitHub repository, and select the branch. Render reads `render.yaml` and offers to create the Vami web service.
3. Enter `GNANI_API_KEY_ID`, `GROQ_API_KEY`, and `GEMINI_API_KEY` when Render prompts for the values. If you create the service outside the Blueprint flow, add them under **Environment** in the Render dashboard. These stay in Render's environment settings; never commit the values to GitHub or `render.yaml`.
4. Create the service and wait for the deploy to finish. Open its `onrender.com` URL and check `/api/health`.

The first deployment requires your GitHub and Render accounts and provider credentials; those account actions cannot be completed from this local project folder. Render's free web services can sleep after 15 minutes idle, and their filesystem is temporary. This prototype stores demo user records in `data/users.json` and sessions in server memory, so records may disappear and sessions reset after restarts or redeploys. Use synthetic documents for a public demo. A real launch needs managed persistent storage, account recovery, and a privacy review.

## Common setup issues

- **Port already in use:** change `PORT` in `.env` (for example to `3001`) and use the matching `localhost` URL.
- **Keys are detected but API calls fail:** verify the key is active, has access to the required model/API, and has no extra spaces. Check the server terminal for the request error without sharing its secret values.
- **Camera or microphone does not start:** allow the browser permission. Camera access generally requires `localhost` or HTTPS.
- **Render starts but health check fails:** confirm the service is a Node Web Service, the start command is `npm start`, and the service logs show it listening on Render's assigned `PORT`.
