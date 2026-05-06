# AI Workshop: Chatbot Building Prompts

Welcome to the AI Chatbot building workshop! Follow these step-by-step prompts to design, document, build, and deploy your chatbot.

---

## Stage 1: Domain Knowledge Brief

**Use this prompt to generate the specialized knowledge rules for your chatbot.**

```text
You are a senior research analyst preparing a knowledge brief for an AI chatbot specialist on the topic of: [TOPIC — e.g., CRYPTOGRAPHY].

The chatbot must ONLY respond about [TOPIC]. Anything off-topic gets politely redirected.
Produce a structured brief with these sections:

1. CORE DOMAIN — 7 key concepts the bot must understand. One-line definition each.
2. USER INTENT PATTERNS — 10 most common questions a user would bring to this bot. Group similar ones.
3. ANSWER TEMPLATES — For each intent, sketch a 2-3 sentence ideal response. Tone: friendly expert.
4. EDGE CASES — What the bot REFUSES or REDIRECTS:
   - Off-topic questions (redirect politely to [TOPIC])
   - Harmful/abusive requests
   - Things outside the bot's scope
   For each, give the polite refusal phrasing.
5. RELATED IMAGE CATEGORIES — 6-8 image types a user would upload that ARE [TOPIC]. List concrete examples.
6. UNRELATED IMAGE CATEGORIES — 3-5 image types that are NOT [TOPIC]. The bot replies: "This image doesn't appear to be related to [TOPIC]. I'm specialized in [TOPIC] — try uploading [example of related image]."
7. PERSONALITY — Pick a tone: friendly expert / playful coach / no-nonsense advisor. One sentence why.

Be specific, not generic. This brief feeds an AI to design UI and backend, so accuracy matters more than length.
```

---

## Stage 2: Google Stitch UI Generation

**Paste the output from Stage 1 into this prompt to generate the UI for your chatbot using Google Stitch.**

```text
You are a senior product designer creating a Google-Stitch-ready UI prompt for a chatbot.
Below is the research brief for the chatbot's domain. Read it carefully.

[PASTE PROMPT 1 OUTPUT HERE]

Now produce a single descriptive prompt I can paste into Google Stitch. The prompt must:
- Reference Apple Human Interface Guidelines (clarity, deference, depth)
- Reference Gemini app's chat UI patterns (clean message bubbles, subtle attachments, distraction-free input)
- Reference Material Design 3 spacing and elevation
- Specify: chat message list, input area with image-upload button, header with topic name and menu, empty state with example questions
- Use a calm, on-topic color palette (suggest 1 primary color that fits the topic)
- Specify mobile-first responsive layout
- IMPORTANT: include a guard that prevents sending empty messages (the send button should disable when input is empty)
- Be one continuous paragraph, 150-250 words

Output ONLY the Stitch prompt. No preamble. No notes.
```

---

## Stage 3: Architecture Documentation & Build Prompt

**Use the HTML generated from Stitch to create your project documentation and the build prompt for Antigravity.**

```text
You are a senior software architect. I will share a chatbot's UI HTML export. You will produce exactly 3 markdown documentation files PLUS one separate build prompt.

UI EXPORT (HTML):
[PASTE STITCH HTML HERE]

Produce the following in order, each in a separate fenced markdown block clearly labelled:

=== File 1: prd.md ===
Product Requirements Document. Include: vision, target user, must-have features (chat with topic-specialization + image upload with topic verification), non-goals, success criteria.
For the system instruction guidance — the bot must be helpful, not paranoid. It should answer general informational questions in its domain (including general home remedies, common knowledge, well-established practices) WITH appropriate disclaimers when relevant. It should refuse only: serious diagnoses, prescription-level advice, harmful instructions, off-topic asks. Do NOT make the bot refuse routine questions.

=== File 2: plan.md ===
Implementation Plan. Stack: FastAPI backend, vanilla JS frontend (use the provided HTML as-is — do NOT redesign), gemini-2.5-flash-lite via google-generativeai Python SDK, GEMINI_API_KEY from environment. List components in build order. Note that empty messages must be blocked at the frontend (Send button disabled when input is empty). Note that the frontend MUST render Markdown (Gemini outputs Markdown natively — raw ** and * symbols will appear otherwise).

=== File 3: tasks.md ===
Numbered list of 10-14 small tasks Antigravity should execute, in order. Each task: 1-2 sentences, no ambiguity. Must include:
- Replace any placeholder/demo/sample data in the HTML with real working API calls
- Add empty-send guard (disable Send button when input is empty/whitespace)
- Integrate marked.js (or equivalent) to render Markdown in chat bubbles — Gemini outputs ** and * for bold/italics and these MUST render properly
- Handle 429 (rate limit) with one 2-second-backoff retry, then return friendly "busy, try again" message
- Handle 403 (PermissionDenied) and other GoogleAPIError exceptions DISTINCTLY from 429 — show the actual error message in the chat (e.g. "API key is invalid or has no permissions"), NOT the generic busy message. This helps students debug bad keys.
- Load GEMINI_API_KEY from a .env file in the project folder

=== Build Prompt for Antigravity ===
After the 3 files, provide one separate fenced text block containing the EXACT prompt the student will paste into Antigravity. The prompt must say:

"This folder contains everything needed to build the chatbot:
- prd.md (product requirements)
- plan.md (implementation plan)
- tasks.md (build steps)
- code.html (UI exported from Google Stitch)

Read all 3 markdown files carefully and analyze code.html. Then:
1. Build a FastAPI backend that connects to gemini-2.5-flash-lite via the google-generativeai Python SDK. Use GEMINI_API_KEY from a .env file.
2. Integrate the backend with code.html — wire up the chat input to a /chat POST endpoint and the image upload to a /vision POST endpoint.
3. REMOVE any demo data, placeholder messages, hardcoded sample replies, or fake conversation history present in code.html. The chat must show only real responses from Gemini.
4. Block empty message sends in the frontend (disable the Send button when input is empty/whitespace).
5. Integrate marked.js via CDN to render Gemini's Markdown output as proper HTML (bold, italics, lists). DO NOT skip this — Gemini will return text with ** and * symbols and they must render as formatted text, not raw symbols.
6. Error handling — handle these distinctly:
   - HTTP 429 (rate limit): retry once after a 2-second wait. If still failing, return a friendly 'busy, try again in a moment' message.
   - HTTP 403 / PermissionDenied / GoogleAPIError: show the actual error in the chat (e.g. 'API key invalid or has no permissions for this project'). Do NOT swallow this as a generic 'busy' message — it makes debugging impossible.
   - Other exceptions: log them and show 'something went wrong, please retry'.
7. The system instruction must let the bot be HELPFUL, not paranoid. General domain questions should get answers with appropriate disclaimers. Refuse only serious harm, prescription-level advice, or off-topic asks.
8. Run on port 8000 and serve code.html at /.
9. Reference Gemini docs at ai.google.dev for SDK syntax.
Build the complete working app — do not stop after just the backend."

All artifacts must reference: gemini-2.5-flash-lite. Free tier limits: 15 RPM, 1000 RPD, 1M context, multimodal supported.
```

---

## Stage 4: Add Vision Feature (Antigravity)

**Paste this into the SAME Antigravity session that already built the working chatbot. Antigravity will read the existing project files and add a vision endpoint that ties into the existing UI's image upload button.**

```text
Now add an image upload + analysis feature to this chatbot. The frontend (code.html) already has an image upload button — wire it up. Do not redesign the UI.

Workflow when a user uploads an image:

1. FRONTEND: when an image is selected via the existing upload button, send it to a new POST /vision endpoint as multipart/form-data along with the chat history context. Disable the upload button while a request is in flight (prevent double-submits). Show the user's uploaded image in the chat as a thumbnail bubble immediately.

2. BACKEND /vision endpoint logic:
   a. Receive the image and validate (max 4MB, only jpg/png/webp). Reject others with a friendly error in chat.
   b. PRE-FLIGHT TOPIC CHECK: send the image to gemini-2.5-flash-lite with this exact prompt:
      "Does this image relate to [READ THE TOPIC FROM prd.md]? Reply with ONLY the word 'yes' or 'no'. No other text, no punctuation."
   c. If response (lowercased, stripped) is 'no': return a friendly chat message — "This image doesn't seem to be related to [TOPIC]. I'm specialized in [TOPIC] — try uploading [pull example from prd.md's RELATED IMAGE CATEGORIES]." Do NOT call Gemini again.
   d. If response is 'yes': send the image to gemini-2.5-flash-lite again with: "Describe this image in detail in the context of [TOPIC]. Be specific, helpful, and useful to the user. Use Markdown for formatting (bold key terms, use lists where helpful)." Return the full description to chat.

3. ERROR HANDLING — same rules as the chat endpoint:
   - 429 -> retry once after 2-second backoff, then friendly 'busy, try again' message
   - 403 / PermissionDenied / GoogleAPIError -> show the actual error in chat (not the generic busy message)
   - Other exceptions -> log and show 'something went wrong, please retry'

4. RENDERING — bot's image-description response must be rendered through the same marked.js pipeline as text chat responses. Bold, lists, headings must render properly (not raw ** and * symbols).

5. The existing chat endpoint must continue to work unchanged. Do not break what was already built.

Test workflow:
- Upload a topic-related image -> bot should describe it in detail
- Upload an unrelated image (e.g., a random food photo for a movie chatbot) -> bot should politely refuse with the redirect message
```

---

## Stage 5: GitHub Guide & Deployment

**Run this prompt to create a Git repository, add a `.gitignore`, initialize a `README.md`, and push your code to GitHub.**

```text
Initialize a git repository in this project folder. Create a .gitignore file that excludes:
- .env (NEVER commit the API key)
- __pycache__/
- *.pyc
- venv/, .venv/, env/
- node_modules/
- .DS_Store
- any IDE folders (.vscode/, .idea/)

Also create a README.md with a short description of this chatbot — what topic it specializes in, the tech stack used (FastAPI + gemini-2.5-flash-lite + vanilla JS), and how to run it locally. Mention that the user must add their own GEMINI_API_KEY to a .env file before running.

Then commit all files with message "Initial commit — VIBECODED workshop chatbot" and push to this remote: https://github.com/[MY_USERNAME]/[MY_REPO_NAME].git

Set the default branch to 'main'. If git asks for credentials, prompt me to enter my GitHub username and personal access token (not password — GitHub no longer accepts passwords for git push). If I don't have a token, walk me through creating one at github.com/settings/tokens with 'repo' scope.
```
