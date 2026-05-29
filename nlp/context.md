## 🎯 Context

**Context** is the actual information provided to the model to help it generate a relevant response. It is the environment or background story for your prompt.

Context can include:

- **Your current prompt:** "Write a summary of this article..."

- **The conversation history:** The previous questions you asked and the answers the AI gave, which allows it to remember what you were talking about five minutes ago.

- **System instructions:** Hidden rules given to the AI (e.g., "Act as a helpful python programmer").

- **External data:** Documents, code files, or search results fed into the model to help it answer specific questions.

Without context, an LLM would treat every single prompt as if it were meeting you for the first time, completely forgetting the previous sentence you typed.

### 🧩 Context Window

The **context window** is the maximum amount of text (measured in **tokens**) that the AI can read and process at one single time.

The context window is **shared** between your input (the prompt and history) and the AI's output (its response). If a model has a context window of 8,000 tokens, the total combined length of your conversation history, your new prompt, and the AI’s upcoming answer cannot exceed 8,000 tokens.


