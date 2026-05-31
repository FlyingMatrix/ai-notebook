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


