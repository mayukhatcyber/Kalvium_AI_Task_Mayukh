# Lesson Studio

**Evidence-led lesson design for instructors.** A complete local-first prototype for the Kalvium AI Research Intern assignment. It takes custom lesson briefs, searches reference catalogs, creates a source-aware 45-minute lesson, runs two separate quality reviews, and saves portable instructor-ready exports.

## Start the app

Requires Node.js 20 or later. No npm dependencies are required.

1. Unzip the project.
2. Open a terminal in the `lesson-studio` folder.
3. Run `node server.js`.
4. Open `http://127.0.0.1:4173` in your browser.

The main **Research & build my lesson** action runs the end-to-end workflow. Without an AI key, it searches Wikipedia and Google Books, then builds an evidence-first 45-minute lesson using the returned source notes. With AI configured, the lesson-generation request also invokes the Responses API web search tool to research the topic more broadly, synthesize the findings, and return citation links. If catalogs or AI are unreachable, the app preserves your brief and falls back to an explicit research-needed draft with retry options.

## Full workflow

1. **Brief:** Enter any custom lesson title, learning objective, learner level, and optional teaching context. The two sample buttons prefill the hiring assignment's required HTTP task or a second JavaScript Promises task.
2. **Research + build:** Click once to search web and book catalogs, attach the initial sources, and design the 45-minute flow. With AI enabled, the model also searches the wider web, synthesizes research, and returns cited URLs. Without AI, the evidence-first draft arranges the catalog notes into a complete teaching flow and flags where the instructor should verify or add subject detail.
3. **Refine sources:** Review excerpts, publisher/author details, and source type. Select or remove sources and add references manually. A YouTube search lead is available; choose and inspect a specific video yourself unless AI search finds a direct video source.
4. **Edit:** Revise each section, its activity, and its check for understanding. Add or remove segments. Sources stay attached to the lesson.
5. **Review:** Run a research-accuracy review and a separate instructional-quality review. The checks inspect citations, source coverage, objective coverage, examples, practice, knowledge checks, and pacing. Record notes and apply selected recommendations to the review notes.
6. **Save and export:** Save to the local lesson library, reopen drafts, search the library, export or re-import JSON, export Markdown, or print the lesson.

## Optional AI generation

The app keeps secrets on the local server. To enable AI-assisted synthesis:

1. Copy `.env.example` to `.env`.
2. Set `OPENAI_API_KEY` to your own API key. You can set `AI_MODEL` and `AI_BASE_URL` if needed.
3. Restart `node server.js`.

The server calls the OpenAI Responses API with its web search tool (required for each AI lesson build) and supplies the selected source excerpts as additional untrusted evidence. See the [official web search guide](https://developers.openai.com/api/docs/guides/tools-web-search). Search citations are retained as lesson references. The key is never returned to browser code. Model and web-search usage may incur charges. The AI is asked to use inline `[S1]` / `[W1]` citations and flag gaps, but generated content still requires instructor review.

## Accuracy and research notes

- The app searches Wikipedia for topic overviews and Google Books for bibliographic records. Topic-specific curated starters include MDN and standards references for the supplied HTTP and JavaScript examples.
- A catalog record or encyclopedia overview is a discovery lead, not proof. Open each source and confirm the claim, author, relevance, and date.
- YouTube is a search link, not an automatically selected or verified video. Add a specific video link after watching it.
- The no-key builder creates a structured draft from source excerpts and teaching prompts; it does not claim to have verified the source itself. AI-assisted mode uses excerpts provided by research and likewise cannot guarantee factual accuracy.
- QC scores are explainable heuristics, not independent expert fact-checking. Re-run reviews after editing and use a human reviewer.

## Data and security

- Saved lessons live in `data/lessons.json` on the same computer. This folder is excluded from the project ZIP and Git by `.gitignore`. Topic searches send the title to Wikipedia and Google Books. If AI is enabled, the brief and selected research excerpts are sent to the configured model provider.
- Briefs and in-progress lesson edits are also backed up to browser local storage for recovery after a failed step or page reload.
- The server binds to `127.0.0.1`; it is intended for local evaluation, not public deployment.
- Never include a real API key when sharing the project. `.env` is ignored by Git.

## Technical decisions

- Dependency-free Node.js server and browser JavaScript keep setup simple.
- The one-click workflow runs research and generation as visible, retryable stages. Source records travel with generated lesson drafts.
- AI provider calls happen server-side, and the experience still works in no-key mode.
- Sources are represented as structured records with URL, excerpt, publisher, type, and citation ID.
- Two QC layers use separate criteria and reports so research reliability and teaching quality can be inspected independently. With a configured key, the app can also call two distinct AI audit passes and attach their findings to the lesson. These passes use the same model with different review instructions, so they are separate audits, not statistically independent expert opinions.

## Demo walkthrough (about 3–4 minutes)

1. Start the app and show a custom brief, then show the HTTP sample values prefilled from the task guide.
2. Run research. Inspect a standards/documentation source, a book result, and the YouTube search link; explain source review and selection.
3. Build and edit a lesson section; show evidence citations, activity, and check for understanding.
4. Run the Research Accuracy review. Show a missing citation or source-quality issue, apply a recommendation, and record reviewer notes.
5. Run the Instructional Quality review. Show objective overlap, practice, example, check, and pacing separately.
6. Save and reopen the lesson from the library; export Markdown and JSON.
7. Load the second sample (JavaScript Promises) and explain how optional server-side AI can be enabled without exposing the API key in the browser.
