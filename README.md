# Vami

**Documents, understood in your language.** Vami is a voice-first document guide for people who find English paperwork difficult to read. Upload a loan, insurance, medical, or official document, ask a question in an Indian language, and hear a plain-language answer.

## Run the preview

Use Node.js 20 or newer. This project has no npm dependencies to install. Opening `index.html` directly with `file://` will not work because the app calls its API. Start the included server instead:

1. Open this folder in VS Code. Add your `GNANI_API_KEY_ID`, `GROQ_API_KEY`, and `GEMINI_API_KEY` values to `.env`.
2. Press **F5** and choose **Run Vami (F5)**, or run `npm start` in VS Code's terminal.
3. Open **http://localhost:3001** (or the port printed in the terminal). Sign in or create a demo account, then upload a file and try a question.

Keep `.env` private. Do not put API keys in `index.html` or a public repository. See [DEPLOYMENT.md](DEPLOYMENT.md) for complete VS Code setup and Render deployment instructions.

## What works in this build

- Responsive web app with a refined document workspace, recent document, upload, camera capture, chat, and account controls. The globe menu switches the full interface between English, Kannada, and Hindi. Automatic mode uses a supported browser language, or English as a fallback; Kannada and Hindi questions also switch the interface when detected. Voice input and replies support ten Gnani language settings.
- Email and password account creation, sign-in, and sign-out. Passwords are salted and hashed; demo account records are stored in `data/users.json`, excluded from Git. Sessions last until the server restarts or the user signs out.
- PDF, text, CSV, JSON, JPEG, PNG, and WebP uploads. Text files are read in the browser; PDFs and images are passed to the multimodal document reasoning endpoint. The Scan button opens the phone camera on supported devices.
- Phone-ready layout keeps the document navigation, message pane, and composer in their own scroll areas. **Upload** opens the phone's document/photo picker; **Scan** asks for a new rear-camera photo. **Paste text** lets users add copied letters, clauses, or other plain text as a source to ask about.
- Automatic first-pass review that checks for payment dates, amounts, exclusions, penalties, requirements, expiry/renewal terms, and betting-related restrictions if those are in the document.
- **Gnani Prisma v3** speech-to-text for recorded questions, through Gnani's documented REST endpoint. Auto language identification detects the spoken language instead of requiring a dropdown change. Browser audio is converted to 16 kHz mono WAV before upload. Each recorded question is limited to 28 seconds.
- **Gnani Timbre v2.5** streamed text-to-speech for answers, using the server-sent events API and a language-matched voice. Hands-free voice mode automatically speaks each reply, then listens for the next question until stopped. If Timbre is unavailable, the browser's voice is used as a fallback.
- Spoken and written replies in Gnani-supported Indian languages: English, Hindi, Tamil, Telugu, Kannada, Malayalam, Bengali, Marathi, Gujarati, and Punjabi. Availability may vary by API account entitlement.
- Credentials stay on the server. Gnani Prisma and Timbre handle speech. Groq's Qwen 3.8 vision model is tried first for photos and plain text because it responds quickly; Gemini 3.8 Flash is the fallback and remains the PDF reader. Add `GROQ_API_KEY` for fast photo/text answers and `GEMINI_API_KEY` for PDFs and provider fallback. If one provider is busy or times out, Vami tries the other compatible configured provider rather than waiting through repeated Gemini retries.

## API flow

`Microphone → browser WAV conversion → Gnani Prisma STT → Groq Qwen vision (photo/text) or Gemini Flash (PDF/fallback) → Gnani Timbre SSE → spoken answer`

For uploaded text documents, the browser extracts text and attaches it to the question. Photos can go to Groq's vision API, with Gemini as a fallback. PDFs go to Gemini's Interactions API, with `store: false`; the document is still processed by Google but the Interaction is not stored as a conversation resource. Gnani speech keys are sent only from the Node server to `api.vachana.ai`.

## Current prototype limits

- This is a hackathon prototype, not a production financial service. User records are local to one server; sessions and basic rate-limit counters are in memory. There is no email verification, password reset, distributed rate limiting, database, or cloud document library.
- Documents stay in browser memory and are sent to the configured reasoning provider when asking a question. They are not persisted by Vami; Gemini Interactions use `store: false`, while Groq also processes submitted photos/text. Use synthetic or redacted documents in a public demo.
- Camera capture works where the browser and device support it. A photo may need to be clear and well lit. Scanned-document quality is not guaranteed.
- Continuous back-and-forth uses discrete recorded turns, not full-duplex realtime interruption. Each question is transcribed after recording ends; the answer audio is streamed in chunks.
- The current PDF reader uses Gemini; photo and plain-text questions prefer Groq Qwen 3.8 and fall back to Gemini. If you want a single-provider Gnani stack, replace this with a document pipeline backed by Evon v3.3 and OCR after confirming model access and practical hosting needs.
- Vami explains what text appears to say. It does not verify whether a contract is fair or provide legal, insurance, medical, or financial advice.

See [HACKATHON_GUIDE.md](HACKATHON_GUIDE.md) for the event requirements, project positioning, strengths and tradeoffs, feature roadmap, and a demo script.

See [SAFETY_AND_OPERATIONS.md](SAFETY_AND_OPERATIONS.md) for the answer guardrails, current security controls, monitoring/logging design, remaining production work, and manual evaluation cases.
