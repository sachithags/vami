# Vami setup and deployment

## What to install

- **Visual Studio Code** for editing the project.
- **Node.js 20 or newer** (npm is included with Node). Vami has no npm package dependencies, so `npm install` is not needed for local use.
- **Git** if you want to publish the project to GitHub and deploy it from a repository.
- A modern browser. Microphone and camera permissions are requested by the browser when you use those features.
- **API credentials:** a Gnani API key ID for Prisma and Timbre, plus a Google Gemini API key for reading and reasoning over documents. Vami keeps both keys on the server.

VS Code, Node.js 24, and npm are already available on the computer where this folder was prepared. Git is only needed for the GitHub deployment route.

## Open and run in VS Code

1. In VS Code choose **File → Open Folder…** and select this `Vami` folder.
2. Open the `.env` file in the project root. Paste your credentials after the equals signs, without quotes:

   ```dotenv
   GNANI_API_KEY_ID=your_gnani_api_key_id
   GEMINI_API_KEY=your_gemini_api_key
   PORT=3001
   ```

   The `.env` file is excluded from Git. Keep it on your computer and never paste keys into `index.html`, source code, screenshots, or a public repository.
3. Press **F5** and choose **Run Vami (F5)** if prompted. Or open **Terminal → New Terminal** and run `npm start`.
4. Visit <http://localhost:3001> (or the port printed by the server). If you add keys while the server is running, stop it with **Ctrl+C** and start it again.

If you see **“Document reasoning is not configured yet”**, add a valid `GEMINI_API_KEY` to `.env` and restart the server. Gnani speech is configured separately with `GNANI_API_KEY_ID`. Check `http://localhost:<PORT>/api/health` to see whether each service is detected; it reports presence only and never returns the key.

## Deploy to Render

This project includes `render.yaml` for a Render **Web Service**. It runs a Node server and serves the UI and API together, so do not create a Static Site for this build.

1. The local `main` branch and first commit are already prepared. Create an **empty private GitHub repository** for Vami (do not initialize it with a README or license). In VS Code open the integrated terminal in this folder and check the staged project is clean:

   ```powershell
   git status --short --branch
   ```

   `.env` and `data/users.json` are excluded from Git. Add the repository URL GitHub provides and push the existing commit:

   ```powershell
   git remote add origin https://github.com/YOUR_ACCOUNT/YOUR_REPOSITORY.git
   git push -u origin main
   ```

   Replace the URL with your own repository URL. If Git asks, configure your author name/email and authenticate with GitHub.
2. In Render, choose **New → Blueprint**, connect the GitHub repository, and select the branch. Render reads `render.yaml` and offers to create the Vami web service.
3. Enter `GNANI_API_KEY_ID` and `GEMINI_API_KEY` when Render prompts for the values. If you create the service outside the Blueprint flow, add both under **Environment** in the Render dashboard. Never commit these values to `render.yaml`.
4. Create the service and wait for the deploy to finish. Open its `onrender.com` URL and check `/api/health`.

The first deployment requires your GitHub and Render accounts and the two API credentials; those account actions cannot be completed from this local project folder. Render's free web services can sleep after 15 minutes idle, and their filesystem is temporary. This prototype stores demo user records in `data/users.json` and sessions in server memory, so records may disappear and sessions reset after restarts or redeploys. Use synthetic documents for a public demo. A real launch needs managed persistent storage, account recovery, and a privacy review.

## Common setup issues

- **Port already in use:** change `PORT` in `.env` (for example to `3001`) and use the matching `localhost` URL.
- **Keys are detected but API calls fail:** verify the key is active, has access to the required model/API, and has no extra spaces. Check the server terminal for the request error without sharing its secret values.
- **Camera or microphone does not start:** allow the browser permission. Camera access generally requires `localhost` or HTTPS.
- **Render starts but health check fails:** confirm the service is a Node Web Service, the start command is `npm start`, and the service logs show it listening on Render's assigned `PORT`.
