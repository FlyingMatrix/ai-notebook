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

### 🧩 What Happens When You Exceed the Context Window?

When the conversation history exceeds the context window, the model utilizes a "sliding window" approach:

- It **drops the oldest parts** of the conversation from its memory.

- It retains the most recent messages.

- If you refer back to something you discussed at the very beginning of a massive chat session, the AI will likely become confused or hallucinate because that data literally no longer exists in its active memory.

### 🧩 The Challenge: "Lost in the Middle"

AI researchers discovered a phenomenon known as **"Lost in the Middle."** LLMs are incredibly good at remembering information at the very beginning of a prompt and the very end of a prompt. However, if you hide a specific fact right in the middle of a massive 100,000-token document, the AI sometimes fails to retrieve it accurately. Newer models have drastically improved this (often tested using "Needle in a Haystack" benchmarks), but it remains a technical hurdle.
