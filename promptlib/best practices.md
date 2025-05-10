# Cursor Best Practices

I'm using Cursor, so I'm wondering if I should go over best practices of using it first. This is to ensure as I work on my projects, I don’t get overwhelmed by complexity.

---

- **Before using Cursor**, ask Claude to create a clear and detailed plan in markdown (ask it to ask clarifying questions and then critique its own plan and then regenerate). Add it to `instructions.md` (so you can ask Cursor to refer to it frequently)

  - I tell ChatGPT what I want to create, then I ask it to provide instructions for another AI which will do the coding. Then I paste everything into the Cursor composer agent.
  - Basically ChatGPT adds another layer of planning which reduces the rate of encountering problems.
  - On one project Cursor was running into some issues and couldn't figure it out no matter what. I wasted hours stuck in a loop. Then I started from scratch, but this time I asked ChatGPT to write clear instructions for another coding AI. It worked like a charm.

- **Use `.cursorrules`** (they are always in AI context) to tell broad rules. See [https://cursor.directory/](https://cursor.directory/)

  - E.g. Write tests first, then the code, then run the tests and update the code until tests pass.

- **Get agent to write code incrementally**, in small chunks of Edit-Test loops:

  1. Define a small task or feature increment.
  2. Write (or have the AI write) a test case that fails for this increment.
  3. Instruct the AI (typically in Agent mode) to write the code to make the test pass.
  4. Instruct the AI to run the test.
  5. If the test fails, the AI analyzes the failure and attempts to fix the code, looping back to step 4.
  6. Once the test passes, the developer reviews the changes.

- **Encourage chain of thought** in your prompts.

- When you run into problems, [ask Cursor](https://ask.cursor) to write a report with all files listed and what they do and the problems encountered. Ask Claude/ChatGPT how to fix the problem.

- [gitinjest.com](https://gitinjest.com) to get all the scripts configs and relevant files (you can filter by extension) in a single page ingestible by ChatGPT.

- [https://context7.com](https://context7.com) / MCP for referring to latest docs.

- **Use git to version control** frequently. Don’t have too many uncommitted changes.

- **Keep context short** by explicitly adding files via `@`. Longer the context, more detailed AI will go.

  - Start new chats when context gets longer.

- **Resync / index code frequently**

  - Use `.cursorsignore` to exclude irrelevant files.
  - Use /Reference on open editors to quickly add them to context.

- **Notepads are frequently used prompts.**

- **Optional**: enable YOLO mode so it writes tests
  - Any kind of tests are always allowed like `vitest`, `npm test`, `nr test`, etc. Also basic build commands like `build`, `tsc`, etc. Creating files and making directories (like `touch`, `mkdir`, etc) is always OK too.

---

## Optional: System prompt in “Rules for AI” in Cursor settings

- Keep answers concise and direct
- Suggest alternative solutions
- Avoid unnecessary explanations
- Prioritize technical details over generic advice
