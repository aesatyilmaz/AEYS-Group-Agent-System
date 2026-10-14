# AEYS Group — AI Agent System

An **8-agent multi-agent system** that runs a solo AI-powered company. There is one flat team with no divisions. An on-demand orchestrator plans each task, picks the right specialist and the right model for every step, and gates careful work through an independent quality reviewer. A plain-Markdown company memory keeps sessions in sync.

> 8 Claude Code subagents · on-demand orchestration · per-task model selection (Opus / Sonnet) · independent quality gate · shared Obsidian memory with read/write rules · approval guardrails · local-first, free by default

---

## Architecture

```
Founder
  │  (calls "Victor" only when the team is needed; otherwise the main session works alone)
  ▼
Victor: Orchestrator (always the strongest model)
  │  plans → picks agent + model per step → dispatches (parallel when independent)
  ├─ Sam    Software Engineer   code, desktop apps, browser games, code-rendered video
  ├─ Neo    AI Engineer         LLM features, prompts, agentic systems
  ├─ Dana   Data Scientist      analysis, statistics, ML, dashboards
  ├─ Alex   Designer            graphics, UI/UX, video direction (designs only, no code)
  ├─ Tess   Social Media        strategy, content packages, fact-checked research
  └─ Nora   Hiring Manager      adds and merges agents, skills, and tools
  │
  ▼
Oscar: Observer (independent quality gate; never fixes another agent's work)
```

### How a task flows
1. **On demand.** The agent system is off by default. The founder calls Victor for team-sized work.
2. **Plan and route.** Victor breaks the goal into steps and assigns each one an agent and a model:
   - **Opus** is the default. It handles careful builds, design, real logic, judgment calls, and quality gates.
   - **Sonnet** handles small, clear, low-stakes steps.
   - When Victor is unsure, he picks Opus. Weak Sonnet output is re-run on Opus.
3. **Brief.** Every brief states the goal, context, constraints, expected output, stage (quick draft or careful build), and how the result will be verified.
4. **Gate.** Careful builds go to Oscar, who returns **PASS** or **SEND BACK** with evidence. After two failed rounds, the issue escalates to the founder.
5. **Report.** Victor reports the result first, listing which agent did which step on which model.

---

## Meet the team

Every agent opens each reply with its name and role ("Sam, Software Engineer, AEYS Group"), so the founder always knows who is speaking. Each persona was distilled from several open-source agent libraries into one focused role. The full definitions are in [`agents/`](agents/).

### 🧭 Victor — COO / Orchestrator · [`coo.md`](agents/coo.md)
Victor is the brain of the company and the only entry point to the team.
- **What he does:**
  - Turns a goal into steps and picks the right agent and the right model for each step.
  - Runs independent steps in parallel and passes outputs forward, so Alex designs and then Sam builds.
- **What he never does:** specialist work himself.
- **Model:** always the strongest one (Opus).
- **Every brief he writes states:** the goal, context, constraints, expected output, stage, and how the work will be verified.
- **His other jobs:**
  - Sends careful builds through Oscar's gate.
  - Keeps the company memory up to date.
  - Suggests a paid decision model only when a step passes an explicit five-question test.

### 🛠️ Sam — Software Engineer · [`software-engineer.md`](agents/software-engineer.md)
Sam is the full-stack builder. He works across:
- Python, TypeScript, React, FastAPI and SQL
- Electron desktop apps
- Browser games (Phaser, Three.js)
- Code-rendered video

**How he works:**
- He follows *minimal-change engineering*: the smallest correct diff.
- He works test-first, debugs systematically to the root cause, and won't say "done" until he has run it and shown the output.
- Before non-trivial work, he names the 2–3 tradeoffs that matter.
- He designs the data model from the access patterns first.

He builds the UIs that Alex designs, and never ships or deploys without approval.

### 🧠 Neo — AI Engineer · [`ai-engineer.md`](agents/ai-engineer.md)
Neo builds the AI core: LLM integration, prompt engineering, NLP, and agentic or multi-agent architecture. The AEYS system itself is one of these.

**How he works:**
- **Eval-driven:** He never tunes a prompt by feel. He collects real outputs, groups the failures, fixes the biggest group, and re-runs.
- **One decision per step:** For each step he decides whether code or an LLM should do it. Every LLM step gets a fallback.
- **Untrusted input:** He treats all outside text as possible prompt injection.

He keeps the house rule: local models only do thin jobs (speech-to-text, TTS), and the reasoning stays with Claude.

### 📊 Dana — Data Scientist · [`data-scientist.md`](agents/data-scientist.md)
Dana turns data into decisions. Her work covers statistics, ML modelling, SQL, A/B and cohort analysis, charts and dashboards.

**How she works:**
1. She profiles the data first.
2. She queries, models or tests it.
3. She checks the methodology and looks for bias before sharing anything.
4. She delivers an insight with a clear recommendation, not just numbers.

She is rigorous and never overclaims.

### 🎨 Alex — Designer · [`designer.md`](agents/designer.md)
Alex owns how everything looks and feels:
- Graphics and image-prompt engineering
- Brand identity
- UI/UX for software and games
- Direction for code-rendered video

**How Alex works:**
- **UI/UX:** Alex sets a design system, writes the spec and hands it to Sam. After the build, Alex audits the result.
- **Video:** Alex writes a brand kit and a second-by-second storyboard, and the founder approves the look before anything is built.

**Hard rules:** Alex designs and never writes code. No image is ever faked, and a missing tool is reported plainly.

### 📣 Tess — Social Media Manager · [`social-media-manager.md`](agents/social-media-manager.md)
Tess has three modes:
- **Strategy:** content calendars, platform differences, hashtag sets, posting cadence.
- **Production:** a full package for each post: visual, caption and hashtags. For video she adds a script and a spec for each platform's cut.
- **Research:** a fact-checked brief on the real story behind a piece of content.

**Hard rules:** Tess never invents facts and never posts. The founder reviews and posts everything.

### 🔍 Oscar — Observer / Quality Gate · [`observer.md`](agents/observer.md)
Oscar is the team's independent eyes. **He never fixes another agent's work.** He returns a verdict of **PASS** or **SEND BACK**, and every finding comes with a severity and evidence (`file:line`, command output, or a source).

He runs the verifier himself and checks the work by type:
- **Code:** tests, review, QA, a security audit, and a check for over-engineering.
- **Design:** accessibility and design anti-patterns.
- **Content:** every claim must have a source.
- **Analysis:** methodology and bias.
- **Video:** frames, audio sync, legibility and licenses.

He also owns the product eval sets and the error analysis. When asked, he reviews how the team itself is performing.

### 🧩 Nora — Hiring Manager · [`hiring-manager.md`](agents/hiring-manager.md)
Nora grows and maintains the team itself.

**Placement:** When a new repo, skill or tool shows up, she works out what it really is and then places it.
- Anything that needs a paid key or an outside service is parked.
- Nothing is added "just in case".
- She prefers adding to an existing agent over creating a new one.

**Distillation:** She merges overlapping personas and skills into one clean version. She never just stacks them together, because a bloated prompt reasons worse.

---

## Memory: shared Obsidian vault

Plain Markdown with no index or database. It is read and written only when the team is called.

| Note | Purpose |
|---|---|
| `Home.md` | Entry point. Holds the read/write rules, active projects, and the founder's preferences. Read first. |
| `Projects/<name>.md` | One note per active project, with its **Status** and **Next** steps. |
| `History.md` | Dated decision log. A reversed decision is struck through and marked `superseded`. |
| `Rules.md` | Readable summary of all company and agent rules. |
| `Oscar/` | One quality-check record per review, used as the source for error metrics. |

**Write discipline:**
- Only the main session and Victor write to the vault.
- Specialists get vault context through Victor's brief.
- Code-level knowledge stays in each project's own docs.
- Secrets are never stored.

---

## Guardrails
- **No outward action without approval.** Nothing is published, posted, deployed, or paid for without the founder's OK.
- **Honest gaps.** Agents report what they can't do instead of faking output.
- **Local-first, free by default.** Anything that needs a paid key or an external service is parked unless the founder overrides.
- **No silent files.** New files and folders are announced first (path, reason, contents) and created only after approval. Oscar's own records folder is the only exception.
- **Brain = Claude everywhere.** Local models are used only for thin roles: speech-to-text and TTS for video voiceover.
- **Paid decision models are suggested, never defaulted.** A step must pass an explicit test before one is even proposed.

---

## Built with the system

Real projects the team has shipped or is building. Each one follows the same loop: plan, route to the right specialist, build, have Oscar verify, and get the founder's approval.

### MOR27 AI — personal job-search hub *(private)*
"My Own Recruiter 2027": a local web app that runs on your own PC. Claude does the AI work.
- **What it does:**
  - Collects postings from company job boards (Greenhouse, Lever, Ashby, SmartRecruiters, Workday) and public listing pages.
  - Scores every job 0–100 against the user's own profile. The factors are role, commute, experience, skills, pay and freshness.
  - Tailors CV bullets and cover letters, using only facts already in the CV.
  - Tracks every application and draws maps of jobs and target companies.
  - Writes a weekly review.
- **Private by design:** All data stays in a local database. The app only drafts and recommends; it never applies, sends or messages anything by itself.
- **Stack:** FastAPI, SQLite and React/Vite, with a one-click Windows installer.
- **How the team built it:**
  - **Victor** owned the phased plan.
  - **Neo** wrote the scoring spec and the tested scoring cases.
  - **Dana** measured how well the scores matched real jobs (calibration).
  - **Alex** ran a design gate: no UI code until the founder approved a direction from three prototypes.
  - **Sam** built the backend, the UI and the collection pipeline.
  - **Oscar** gated every phase.
- **Pace:** about 100 commits and 76 versions in its first week.

### ROME AI — always-on personal assistant *(private, in progress)*
A from-scratch voice assistant for Windows with a $0 running-cost target. It is built as a cost funnel: each request falls through cheap local layers first, and only the hard leftovers reach a free cloud model.
- **Pipeline:** wake word, then Whisper speech-to-text, then a reflex layer of 67 commands, then semantic memory with hybrid recall (synced with Obsidian), then a local LLM brain with 13 tools, then Kokoro TTS.
- **Engineering discipline:**
  - Every interaction writes one trace row.
  - Nothing is learned without an eval.
  - Each phase has its own plan, and Oscar reviews every phase gate.
- **Progress:** Phases 0–2 are done (foundation, senses and reflex, memory). Phase 3, the local brain, is about 90% done.
- **Measured so far:**
  - Reflex commands: 155/155
  - Memory recall: 35/37
  - Speech-to-text: about 0.42 s
  - Tool selection: 86.7%
- **How the team builds it:**
  - An Opus orchestrator writes each phase's plan and runs the independent agents in parallel. It writes no code.
  - **Neo** owns the AI layers.
  - **Sam** owns the runtime and the tests.
  - **Dana** owns the trace schema, the evals and the metrics.
  - **Oscar** runs the phase-gate reviews.

### Open-source apps (AEYS Innovations)
- **[TONY App](https://github.com/aesatyilmaz/TONY-App):** say "Tony", then a command. It is a fully local Windows voice assistant with 63 fixed commands and webcam gesture input. There is no cloud, no account and no API key.
- **[VISION App](https://github.com/aesatyilmaz/VISION-App):** control the computer with your hands through a webcam. You can move the mouse, click, scroll, change the volume and switch windows. It runs locally and is packaged as a double-click Windows app.

---

## Engineering highlights
- **Persona distillation and de-duplication.** Each agent is synthesized from curated open-source agent libraries into one focused, non-overlapping persona. Nora enforces this on every addition.
- **Right-sized models.** Model choice happens per step, not per agent, so cost tracks task difficulty.
- **Independent verification.** The reviewer runs the verifier itself (tests, previews, frame and audio checks for video) and never accepts "it works" without evidence.
---

## Repository layout

```
agents/       8 agent definitions (persona, scoped tools, skills, rules)
docs/         company overview; v1 test report (historical)
.env.example  configuration template (no secrets committed)
```

## Changelog
- **v2 (2026-10):** Restructured from 16 agents in 3 tiers to one flat team of 8.
  - Added Oscar (quality gate and evals).
  - Orchestrator now runs on demand, with per-task Opus/Sonnet routing.
  - Memory is now plain Obsidian with read/write rules (graphify removed).
  - Per-agent voices removed.
  - Approval required before creating files.
- **v1 (2026-08):** 16-agent hierarchy with domain coordinators, per-agent TTS voices, and a knowledge-graph memory. See [`docs/TEST-REPORT.md`](docs/TEST-REPORT.md).

---

*Personal project. Secrets are never committed (see `.gitignore`). Paths in agent files use `%USERPROFILE%` placeholders.*
