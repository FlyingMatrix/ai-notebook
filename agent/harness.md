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




