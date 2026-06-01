## 🎯 Harness

**Agent = Model + Harness**

**If you're not the model, you're the harness.**

A **harness** is every piece of code, configuration, and execution logic that isn't the model itself.  A raw model is not an agent. But it becomes one when a harness gives it things like state, tool execution, feedback loops, and enforceable constraints.

Concretely, a harness includes things like:

- System Prompts
- Tools, Skills, MCPs + and their descriptions
- Bundled Infrastructure (filesystem, sandbox, browser)
- Orchestration Logic (subagent spawning, handoffs, model routing)
- Hooks/Middleware for deterministic execution (compaction, continuation, lint checks)

There are many messy ways to split the boundaries of an agent system between the model and the harness. But this is the cleanest definition because it forces us to think about **designing systems around model intelligence.**

### 🧩 Anthropic Harness Framework

The **Anthropic Harness Framework** is for running Claude as a long-horizon, autonomous AI agent, which relies on a **three-agent architecture**, a decoupled backend infrastructure, and a "GAN-style" feedback loop.

1. **The Three-Agent Architecture**

Instead of asking a single LLM to handle everything (which leads to context loss and code degradation), the framework splits the cognitive load among three distinct agent personas:

- **The Planner:** Takes a brief user prompt (even just 1–4 sentences) and expands it into an ambitious, high-level product specification. It intentionally avoids granular technical implementation steps to prevent early errors from cascading downstream.

- **The Generator:** Works in iterative "sprints," tackling one feature from the spec at a time. It uses a modern tech stack (e.g., React, Vite, FastAPI, PostgreSQL) and utilizes Git for version control to manage state.

- **The Evaluator:** Acts as a strict QA engineer. Crucially, it uses **Playwright via the Model Context Protocol (MCP)** to spin up the application, spin up a browser, click around the UI, test API endpoints, and check database state.
2. **The "GAN-Style" Feedback Loop**

A major limitation of LLMs is **self-evaluation bias**—models are notoriously terrible at critiquing their own work and will often praise buggy code they just wrote. To solve this, Anthropic’s framework mimics a **Generative Adversarial Network (GAN)**:

```
[ Planner ] ──> Creates Product Spec
                      │
                      ▼
 ┌──────────────> [ Generator ] (Writes/Fixes Code)
 │                    │
 │                    ▼ (Hands over app build)
 └─────────────── [ Evaluator ] (Runs Playwright MCP Tests)
                      │
                      ▼ (Passes criteria?)
               [ Final Polished App ]
```

The Evaluator grades the Generator’s output using a strict, weighted rubric across four dimensions:

- **Design Quality:** Holistic visual identity and layout cohesion.

- **Originality:** Heavily penalizes generic, predictable AI-template designs to force creative output.

- **Craft:** Technical execution, spacing, typography, and color harmony.

- **Functionality:** Raw usability and verification that the code actually works.

The Generator and Evaluator loop continuously (often 5 to 15 iterations over 2 to 4 hours) until the criteria are met, transforming a $10 broken prototype into a highly polished, fully functioning application.

### 🧩 OpenAI Harness Framework

While Anthropic’s framework focuses on **multi-agent collaboration (Planner / Generator / Evaluator)**, OpenAI’s framework focuses heavily on **Environment Design**—shaping the repository so the agent can autonomously navigate and maintain it over massive, long-horizon lifecycles.

**Core Pillars of the OpenAI Framework:**

1. **Rigid Architectural Invariants**

Agents fail when given too much blank-canvas freedom; they thrive within predictable constraints. OpenAI built the product around a strict architectural model with fixed boundaries.

- **Enforcing Invariants:** Instead of telling the agent *how* to write code, the harness enforces *rules* (e.g., forcing data shape validation at service boundaries).

- **Mechanical Enforcement:** These constraints are mechanically checked via custom, agent-generated linters and structural tests embedded in the CI/CD pipeline. If the model violates the architecture, the harness automatically rejects it before human review.
2. **Total Context Localization & "Agent Legibility"** 

To an AI agent, if a piece of information doesn't exist in its active context window, it doesn't exist at all. OpenAI eliminated external dependencies like Google Docs, Jira, or Slack threads.

- **Repository as the Source of Truth:** Everything—product specs, execution plans, technical debt tracking, and quality grades—is written in Markdown and checked directly into the Git repository.

- **Living Documentation:** The repository maps out package layers and indexes domains. The agent reads this "map" locally using command-line tools (like `gh` and custom scripts), giving it full architectural awareness without requiring humans to copy-paste context.

An OpenAI-style harness codebase distributes the living documentation across specialized files and folders in the repository root:

```
├── AGENTS.md                  <-- The Entry Point (Tech stack, commands, map of docs)
├── CLAUDE.md                  <-- (If using Anthropic tools alongside it)
├── docs/
│   ├── architecture/          
│   │   ├── layers.md          <-- Top-level map of domains and package layering
│   │   └── invariants.md      <-- Rules for data validation at system boundaries
│   ├── quality/
│   │   └── domain_grades.md   <-- Scorecard tracking technical debt per layer
│   ├── design/
│   │   └── tokens_and_ui.md   <-- Core visual identities and UX guidelines
│   └── plans/
│       ├── active/            <-- Current step-by-step execution plans
│       └── completed/         <-- Decision logs of what was already built
```

**How the Living Documentation Actually Works**

Instead of reading everything at once, the agent navigates this documentation dynamically using CLI tools (like `ls`, `grep`, or custom repo-indexing scripts):

- **`AGENTS.md` (The Entry Point)**

This file is kept minimal. It includes just the core tech stack, the exact commands to run tests/linters, and short pointers to the rest of the documentation. It outlines "Forbidden Patterns" (e.g., *“Do NOT use the requests library; use httpx”*).

- **Architecture Maps (`/docs/architecture/`)**

This is where the package layers and domain indexes actually live. It defines the rigid boundaries of the application. For instance, it explicitly states which layers are allowed to talk to each other (e.g., `Data Layer` $\rightarrow$ `Service Layer` $\rightarrow$ `API Layer`).

- **Execution Plans (`/docs/plans/`)**

OpenAI treats plans as first-class, version-controlled artifacts. If a task is complex, an execution plan markdown file is checked into `/docs/plans/active/`. As the agent completes features or changes architectural decisions, it updates this markdown file directly. This ensures that if an agent session times out, the next sub-agent can read the plan's log and seamlessly pick up exactly where the last one left off.

- **The Quality Document (`/docs/quality/`)**

This file dynamically tracks gaps over time, grading each product domain on a rubric. Background agents scan this file to determine which domains have accumulated too much technical debt, automatically spinning up PRs to refactor those areas.

3. **The "Ralph Wiggum Loop" (Self-Review)**

To handle the development of features end-to-end, OpenAI implemented a local, autonomous iteration loop for the agent:

- The engineer feeds a high-level prompt or plan into the **Codex CLI**.

- The agent generates the code and automatically reviews its own changes locally.

- The harness spins up an ephemeral local observability stack, allowing the agent to capture real-time logs, metrics, and traces to **reproduce bugs and validate its own fixes**.

- The agent requests automated code reviews from *other* local and cloud-based sub-agents, iterating in a continuous loop until all agent reviewers are satisfied before it ever hits a human engineer's queue.
4. **Continuous Automated "Garbage Collection"**

Left completely alone, AI-generated codebases suffer from severe pattern drift and entropy. To combat this, OpenAI's harness treats technical debt like a system-level process:

- **Background Scanning:** Background agent tasks constantly scan the codebase for deviations from "golden principles."

- **Auto-Refactoring PRs:** The harness automatically generates targeted refactoring pull requests to clean up code rot and update documentation. Because these PRs are small and mathematically verified by structural tests, human engineers can review and auto-merge them in seconds.

### 🧩 Relevant Links

**OpenAI:**

- Harness engineering: leveraging Codex in an agent-first world

    [Harness engineering: leveraging Codex in an agent-first world | OpenAI](https://openai.com/index/harness-engineering/)

**Anthropic:**

- Effective harnesses for long-running agents

    [Effective harnesses for long-running agents \ Anthropic](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)

- Harness design for long-running application development

    [Harness design for long-running application development \ Anthropic](https://www.anthropic.com/engineering/harness-design-long-running-apps)

**LangChain:**

- The Anatomy of an Agent Harness

    [The Anatomy of an Agent Harness](https://www.langchain.com/blog/the-anatomy-of-an-agent-harness)

**Mitchell Hashimoto:**

- My AI Adoption Journey

    [My AI Adoption Journey – Mitchell Hashimoto](https://mitchellh.com/writing/my-ai-adoption-journey)

**martinfowler.com:**

- Harness Engineering - first thoughts

    [Harness Engineering - first thoughts](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering-memo.html)

- Harness engineering for coding agent users

    [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)
