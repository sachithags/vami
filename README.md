# Vami

Vami helps people understand English paperwork by asking questions in the language they speak. Upload a document or take a photo, ask by voice or text, and get a plain-language answer you can hear.

## What it helps with

Loan agreements, insurance policies, bills, medical letters, and other documents can hide important dates, amounts, exclusions, fees, and conditions in dense English. Vami gives families a simpler way to find and ask about those details. Its focus is one complete flow: bring in a paper, ask in the language you speak, and hear an explanation you can follow up on. It can also explain betting terms neutrally, without recommending a bet.

## Features

- Upload a PDF, text file, or photo; use a phone camera to capture a page; or paste text.
- Ask by typing or speaking, then ask follow-up questions in the same conversation.
- Get an initial plain-language review of amounts, due dates, fees, exclusions, eligibility, renewal terms, and restrictions stated in the document.
- Use the interface in English, Kannada, or Hindi. Automatic mode follows supported browser settings and can switch to Kannada or Hindi when it detects those languages in a question.
- Use Gnani voice input and spoken replies in supported Indian languages, including English, Hindi, Tamil, Telugu, Kannada, Malayalam, Bengali, Marathi, Gujarati, and Punjabi. Availability depends on API access.
- Create an account, sign in, and sign out. Vami does not save uploaded documents to its server.

## Demo

**Live app:** [Open Vami]([https://vami-tsp0.onrender.com]

**Video:** [Watch the demo](https://drive.google.com/file/d/1TwuV8vu5d2SB5dy6SgeLYe777295g0iO/view?usp=sharing)

See [DEPLOYMENT.md](DEPLOYMENT.md) for GitHub and Render steps.

## How Gnani is used

- **Gnani Prisma v3** turns recorded speech into text and detects the spoken language.
- **Gnani Timbre v2.5** streams the answer back as speech. Hands-free mode listens for a follow-up after the reply.
- **Document answers use separate providers:** Groq Qwen 3.8 handles photos and plain text; Gemini 3.8 Flash reads PDFs and provides fallback for supported files. Gnani powers the listening and speaking parts of Vami, not document reasoning.

That split is worth explaining in a demo: show Prisma transcribing a spoken Kannada question, Vami checking a sample loan document, then Timbre reading the answer aloud.

## Limits to keep in mind

Vami is a reading aid, not a financial, legal, medical, or insurance adviser. It can miss text or misunderstand a scan; check important dates and amounts against the original document or the provider. Files are sent to the configured AI provider when Vami answers a question. Demo accounts are stored in a local JSON file and are not ready for a production launch.

For a hackathon demo, use a fictional loan or insurance document. Ask about a due date or fee, then ask for a detail that is missing and show that Vami says it cannot find it in the file.

More detail: [SAFETY_AND_OPERATIONS.md](SAFETY_AND_OPERATIONS.md).
