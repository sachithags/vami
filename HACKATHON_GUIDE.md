# Vami — hackathon pitch and submission guide

## The one-line pitch

**Vami turns intimidating English paperwork into a private, spoken conversation in the language a family is most comfortable using.**

## The problem

Families often receive important information as English PDFs, letters, policy schedules, bills, and loan terms. Asking a relative to interpret them is slow and can expose private information. General-purpose voice assistants can answer general questions, but Vami's job is to stay grounded in *this person's document*, explain consequential details in everyday language, and let the person ask follow-up questions without learning how to type a prompt.

## What is genuinely different

Vami is a document-first voice interface, rather than a voice assistant with a document-upload button added on. Its flow keeps the document and the family's follow-up questions together:

1. A person uploads a PDF or takes a photo of the paper.
2. They ask naturally in Tamil, Hindi, Telugu, Kannada, Malayalam, Bengali, Marathi, Gujarati, Punjabi, or English.
3. Gnani Prisma transcribes their spoken question.
4. The document is supplied as context to the reasoning step, which should answer from the text, explain dates and amounts, and flag uncertainty.
5. Gnani Timbre speaks the answer, and the person can ask another question hands-free.

General voice agents are strong at open-ended conversation. Vami's differentiator is **document grounding + voice access + plain-language explanation + local-language follow-up in one focused task**. Avoid claiming that no other product can do document Q&A, OCR, translation, or voice. The stronger and defensible claim is the combination and audience focus, then prove it in the demo.

### How to compare Vami honestly

ChatGPT supports document uploads and voice conversations, including adding images during a live voice chat. Gemini Live supports camera/screen conversations, and Google documents a PDF-question workflow on eligible Android devices. So the core building blocks already exist in major assistants. Vami should stand out through a narrower, parent-first workflow: regional-language voice controls, large readable UI, guided questions about dates and charges, a privacy explanation, and a strong “show me where in the document” trust feature as the next build. The focused UX is the product difference; don't claim the underlying capabilities are exclusive.

| Product | Strengths a user may already have | Vami's honest angle |
| --- | --- | --- |
| General voice assistants | Broad knowledge, voice conversation, image/document handling | Vami opens directly into document help and guides the user toward dates, charges, exclusions, and next steps |
| Vami (current prototype) | Upload/camera, typed/voice questions, Gnani regional STT/TTS, answer from attached document | A parent-oriented, multilingual paperwork flow that can be tuned for accessibility and high-stakes caution |
| Vami (best next version) | Add page citations, text highlighting, source confidence, and a retake-photo quality check | Make every consequential answer easy to check against the original paper |

This comparison was checked against the vendors' current official help pages: [ChatGPT uploads](https://help.openai.com/en/articles/8555545-uploading-files-and-audio-to-chatgpt), [ChatGPT Voice](https://help.openai.com/en/articles/20001274-chatgpt-voice), [Gemini Live camera guidance](https://blog.google/products-and-platforms/products/gemini/gemini-live-camera-how-to/), and [Gemini PDF questions](https://support.google.com/files/answer/15880481?hl=en-JE). Availability for competitor features can depend on plan, device, and region.

### Gnani technology in this build

- **Prisma v3 speech-to-text:** the microphone audio is converted to 16 kHz mono WAV and sent to Gnani's REST transcription API.
- **Timbre v2.5 text-to-speech:** the answer is streamed as PCM audio chunks; the client begins playback while later chunks are still arriving. Vami selects a voice and language for each supported Indian language.
- **Gemini 3.8 Flash via Interactions API:** multimodal document reading and answer reasoning. This part currently uses a second provider. Name this clearly in the submission and demo; don't imply that Gnani generates the answer. The contest page lists Prisma and Timbre APIs and Evon v3.3 open weights, so a later single-provider version could replace Gemini with Evon after access and hosting are confirmed.

## Features in the current project

- Responsive, polished document workspace.
- Document upload for PDF, text, CSV, JSON, JPEG, PNG, and WebP; camera capture on compatible mobile devices.
- Automatic first-pass review for obligations, payment values/dates, exclusions, penalties, eligibility, expiry, renewals, and restrictions the document actually states.
- Private email/password sign-up, sign-in, sign-out, salted password hashes, and server-side API keys.
- Gnani-powered voice questions and streamed, spoken answers.
- A voice conversation loop: record → transcribe → answer → speak → listen again. Tap the microphone to end the loop.
- Typed questions, follow-up context, suggested questions, and language selection.
- Guardrails for missing/out-of-document facts, document prompt injection, insurance cover vs approved payout, gambling constraints, and decision-making boundaries; structured request logs that omit prompts, documents, transcripts, emails, and IPs; basic rate limits and a credential-safe health endpoint.
- Clear setup docs, working local HTTP server, key isolation, and a project feature/limitation guide.

## Advantages

- **A real access problem:** understandable loan, insurance, government, and medical paperwork matters to families.
- **Voice-first:** helps people who have low confidence with reading or typing in English.
- **Document-grounded interaction:** the user can ask about their own paperwork instead of getting a generic answer.
- **Strong fit for Gnani:** Prisma and Timbre have real, visible roles in the user journey, not a token mention in the README.
- **Easy to demonstrate:** show an English due date or exclusion, ask in a regional language, then let a family member hear the explanation.
- **Privacy-conscious architecture:** credentials remain server-side and the Vami server does not save uploaded documents.

## Tradeoffs and risks to own honestly

- **A wrong explanation could matter.** Contracts and policies need source references, uncertainty language, and a prominent reminder to confirm consequential decisions with the provider or a qualified person.
- **Two AI providers mean two credentials and two possible costs.** Prisma and Timbre are Gnani; Gemini is the current document reasoner.
- **Live voice depends on network and account access.** API credit, key permissions, microphone support, language selection, and service latency all need a real run-through before the public demo.
- **The current accounts are a prototype.** User records are stored in a local JSON file and sessions live in server memory. This is not a hardened cloud identity system.
- **The prototype does not save a cloud document library.** It keeps a selected file in the current browser session; be precise in the pitch about what does and does not persist.
- **Photographs can be hard to read.** Glare, blur, handwriting, and low contrast should trigger a request for a clearer photo, not a confident guess.
- **No speech biometrics or identity verification.** A voice transcript is a question, not proof of who is speaking.

## What would make Vami more distinctive next

Choose one or two improvements you can actually build and show. Avoid presenting roadmap items as finished features.

1. **Show the evidence:** each answer includes a page and a highlighted sentence or amount from the source, with “I couldn't find that in the document” when unsupported.
2. **Dates and money at a glance:** identify payment amount, due date, waiting period, exclusions, cancellation window, late fee, and contact number; let the user hear each item.
3. **Ask before signing:** a short checklist of questions to ask the bank or insurer, based only on clauses in the uploaded document.
4. **Family review mode:** let a trusted relative review a shareable, redacted explanation rather than the entire original document.
5. **Code-mixed speech:** explicitly demo a natural mix of Tamil/English or Hindi/English, and let the user choose the reply language separately from the language used to ask.
6. **Low-connectivity accessibility:** cache the interface, clearly queue no sensitive document, and offer a phone-call or assisted-service route if the provider connection is unavailable.
7. **Document confidence and image quality check:** ask the user to retake an unreadable page before interpreting it.
8. **Accessibility controls:** large text, slower speech, repeat, pause, and “say that more simply.”

The best next feature for a persuasive prototype is **source-linked answers**. It directly addresses trust and is simple to explain: every important statement points back to where it came from.

## Startup direction

### Product

Start with one job: “Help me understand this paper in the language I speak.” Make Vami an accessible mobile web app for families, with an assisted mode that a community worker or adult child can use alongside a parent. A QR code on a participating institution's letter could open the right-language explanation flow without asking the user to find the correct app first.

### Who could pay

- **B2C:** a free starter allowance, then a low-cost family plan for repeat document explanations and saved checklists.
- **B2B2C:** banks, insurers, clinics, government service desks, and community organisations could sponsor verified explanations or offer Vami from their customer-support pages.
- **Assisted service:** NGOs or local-language help centres could use a staff console to explain a document with a customer present.

Avoid ad targeting based on policy, loan, health, or betting documents. Don't sell document contents or use them to train a model without clear, specific consent. Show who processes a file and give a way to close the session and clear local data.

### Roadmap that earns trust before adding breadth

1. **Prototype:** upload/photo, regional voice question, plain-language answer, visible limits, synthetic sample docs.
2. **Trust:** answer citations with page and highlighted clause; confidence/source coverage; “not found” behavior; blurry-page retake prompt.
3. **Pilot:** 10–20 families via a local NGO or community group; test whether users can correctly answer “what is due and when?” before and after Vami. Collect consent and comprehension results without keeping their real documents.
4. **Institution trial:** one lender/insurer or public-service office; build branded safe links, domain-specific glossary, and human escalation.
5. **Scale:** audit logs with opt-in and retention controls, language QA by native speakers, secure account recovery, cloud deployment, and reviewed privacy terms.

For the hackathon, show one working end-to-end task and a real comprehension improvement. A careful Tamil/Hindi walkthrough with a synthetic document and source clause is stronger than a long feature list.

## A competitive submission strategy

- The strongest natural categories for Vami are regional-language use and real-world impact. Select the category whose criteria the recorded demo can actually prove; the rules say each team/person can receive at most one award.
- Demonstrate an actual Gnani request and returned transcript/audio if your key/credits allow it. The user's contribution and the API contribution should be said out loud, not buried in the README.
- If time is short, prioritise: one-language end-to-end live run, automatic review, one follow-up, visible source wording, and a clean 60-second video. Don't spend the final hours adding sign-in polish or speculative features.
- Use genuine user feedback if available. Never buy reactions, trade engagement, or use bots; the official rules explicitly forbid these.

## 60-second video script

Record the real app, real audio, and a synthetic sample document. Keep the Gnani API indicators visible or name the exact APIs out loud.

> “This is Vami. My parents can receive an important insurance or loan document in English, but the part they need is often the hardest to understand. Vami lets them take a photo or upload the PDF, then ask about it in their own language.
>
> Here I’m asking in Tamil: ‘When is my next payment, and what happens if it’s late?’ Gnani Prisma turns my voice into text. Vami checks the uploaded document and explains the answer simply. Gnani Timbre streams the reply back in a natural Tamil voice. I can ask another question without typing.
>
> Vami is built for one specific moment: when a family needs to understand the paperwork in front of them, privately and clearly. Gnani Prisma powers listening; Gnani Timbre powers speaking. This is Vami — documents, understood.”

Keep the recording to 50–60 seconds. Do not use a synthetic result as if it came from a live API; if a service is unavailable, label the fallback clearly.

## Three-minute judging walkthrough

1. **0:00–0:20 — Person and pain.** “A parent receives an English policy schedule and wants to know what they pay and when.”
2. **0:20–0:40 — Upload.** Sign into the demo account, add a synthetic sample, show the filename and selected language.
3. **0:40–1:10 — Voice.** Record one natural Tamil/Hindi/etc. question. Show the transcript and identify Prisma.
4. **1:10–1:45 — Grounded answer.** Ask about a visible date/amount/exclusion. Read the sentence in the PDF and compare to the answer. Mention Gemini 3.8 Flash through the Interactions API is the document-reasoning provider in this prototype.
5. **1:45–2:15 — Timbre.** Play the streamed response; ask one follow-up hands-free. Mention Timbre v2.5's streamed audio and language-matched voice.
6. **2:15–2:40 — Trust.** Show the professional boundary: Vami explains document wording and doesn't tell someone to sign, borrow, buy, or cancel.
7. **2:40–3:00 — Close.** State the impact, API contribution, and the next feature: answers linked to source page and sentence.

## Official challenge checklist (checked November 9, 2026)

- The event is the **Great Indian AI Internship Challenge**. Its current official page says the brief is open and requires a working app using at least one Gnani AI API or model.
- The current eligibility page says entrants must be enrolled in a college/university course in India, 18 or older, and submit solo or in a team of two. Recheck the terms for your situation.
- Final submission deadline: **November 10, 2026 at 11:59 PM IST**. Build and post the public demo before submitting.
- The official flow asks for a demo video, written project explanation, Gnani API/model names, member contributions, and nominated LinkedIn/X post URLs. Tag Gnani AI and include **#GnaniAI** and **#GreatIndianAIInternshipChallenge**.
- The site says public post engagement is checked for shortlist, then shortlisted participants interview individually. That is not a guaranteed selection or job offer.
- Published ₹50,000 category awards include regional languages, code-mixed voice, noisy telephonic audio, real-world impact, solo/duo, creativity, Tier 2/Tier 3, best 60-second demo, and Crowd Favourite. Review current rules and eligibility on the official page before submitting.
- Do not buy engagement, use bots, arrange engagement swaps, plagiarize, or misrepresent an API demo. Use a synthetic document and do not show real Aadhaar, PAN, phone, bank, or policy details.
- The registration page says registered participants receive 5,000 programme API credits. Confirm your own access and balance in the Gnani platform.

### Suggested submission description

**Project:** Vami — Voice-first document understanding for Indian families.

**Problem:** English financial and official paperwork excludes people who are more comfortable asking questions by voice in an Indian language.

**Solution:** Vami lets a user upload a document or capture it with a phone, ask a question in their language, and hear a plain-language explanation tied to that document.

**Gnani APIs:** Gnani Prisma v3 REST speech-to-text; Gnani Timbre v2.5 SSE text-to-speech (language-matched voice selected in the app). **Other model:** Gemini 3.8 Flash through Google's Interactions API is used for document reading and answer reasoning in this prototype.

**Impact:** Reduces the language and reading barrier between a family and the information in a document. It does not replace professional advice or make decisions for the user.

**Contribution:** Replace this line with the real work done by each participant. For a solo entry, say “Designed and built the app, API integration, and demo.” Do not claim work that wasn't yours.

## Event links

- [Official challenge page and terms](https://www.gnani.ai/internship)
- [Official registration/submission guidance](https://www.gnani.ai/great-indian-ai-internship-registration-form)
- [Gnani API quick start](https://docs.gnani.ai/api/introduction/quick-start)
- [Prisma speech-to-text docs](https://docs.gnani.ai/api/STT/speech-to-text)
- [Timbre streaming TTS docs](https://docs.gnani.ai/api/TTS/tts-sse)
- [Timbre voice catalog](https://docs.gnani.ai/api/TTS/available-voices)
