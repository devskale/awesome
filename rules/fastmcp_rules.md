# FastMCP Plugin Development Protocol

⚠️ CRITICAL: DO NOT USE attempt_completion BEFORE TESTING ⚠️

## Step 1: Planning (PLAN MODE)

- What problem does this tool solve?
- What API/service will it use?
- What are the authentication requirements?
  □ Standard API key (use environment variables)
  □ OAuth (requires separate setup script)
  □ Other credentials

## Step 2: Implementation (ACT MODE)

1.  **Setup Environment and Installation**

    Create and activate a virtual environment, then install FastMCP using `pip`.

    ```bash
    # Create a virtual environment
    python -m venv .venv

    # Activate it (on macOS/Linux)
    source .venv/bin/activate
    # On Windows, use: .\venv\Scripts\activate

    # Install the framework
    pip install fastmcp
    ```

2.  **Core Implementation**

    Create your server in a Python file (e.g., `server.py`). FastMCP makes this simple and Pythonic.

    - **Use the `FastMCP` SDK:** Define your server and decorate functions to expose them as tools, resources, or prompts.
    - **Implement Comprehensive Logging:** Use the `Context` object to send logs to the client and Python's `logging` for server-side debugging.
    - **Add Type Definitions:** Use standard Python type hints. FastMCP uses them to generate schemas automatically.
    - **Handle Errors with Context:** Catch exceptions and report them to the client using `ctx.error()`.

    _Example `server.py`:_

    ```python
    import os
    import logging
    from fastmcp import FastMCP, Context

    # Configure server-side logging
    logging.basicConfig(level=logging.INFO)

    # Get API key from environment
    API_KEY = os.getenv("MY_API_KEY")

    mcp = FastMCP(name="My Awesome Server 🚀")

    @mcp.tool
    async def my_tool(ctx: Context, query: str) -> str:
        """
        This is a tool that does something awesome.
        The docstring is used as the tool's description.
        """
        if not API_KEY:
            await ctx.error("API Key is not configured.")
            return "Error: API_KEY not found."

        await ctx.info(f"Received query: {query}")
        try:
            # ... your logic here ...
            result = f"Result for '{query}'"
            await ctx.info("Processing complete.")
            return result
        except Exception as e:
            logging.error(f"Error processing query '{query}': {e}")
            await ctx.error(f"An unexpected error occurred: {str(e)}")
            return "Error: Processing failed."

    if __name__ == "__main__":
        mcp.run()
    ```

3.  **Running Your Server**

    With your virtual environment active, run your server from the command line.

    ```bash
    fastmcp run server.py
    ```

    To provide an API key, set it as an environment variable. You can use a `.env` file (FastMCP uses `python-dotenv`) or set it in the shell:

    ```bash
    MY_API_KEY="your_secret_key" fastmcp run server.py
    ```

## Step 3: Testing (BLOCKER ⛔️)

<thinking>
BEFORE using attempt_completion, I MUST verify:
□ Have I tested EVERY tool?
□ Have I confirmed success from the user for each test?
□ Have I documented the test results?

If ANY answer is "no", I MUST NOT use attempt_completion.
</thinking>

1.  **Test Each Tool (REQUIRED)**

    Use the in-memory client for fast and reliable testing. This is the **preferred** method. Create a test file (e.g., `test_server.py`) and use `pytest`.

    _Example `test_server.py`:_

    ```python
    import pytest
    from fastmcp import Client
    from server import mcp  # Import your FastMCP instance

    @pytest.mark.asyncio
    async def test_my_tool_works():
        # The client connects directly to your server instance in memory
        async with Client(mcp) as client:
            result = await client.call_tool("my_tool", {"query": "test"})
            assert "Result for 'test'" in result.text

    @pytest.mark.asyncio
    async def test_my_tool_handles_error():
        # Example for testing error handling (e.g., no API key)
        async with Client(mcp) as client:
            result = await client.call_tool("my_tool", {"query": "test"})
            assert "Error:" in result.text
    ```

    Install testing libraries and run the tests (ensure your venv is active).

    ```bash
    pip install pytest pytest-asyncio
    pytest
    ```

    ⚠️ DO NOT PROCEED UNTIL ALL TOOLS ARE TESTED.

## Step 4: Completion

❗ STOP AND VERIFY:
□ Every tool has been tested with valid inputs and edge cases.
□ Output format is correct for each tool.

Only after ALL tools have been tested can `attempt_completion` be used.

## Key Requirements

- ✓ Must use the **FastMCP** SDK.
- ✓ Must use the `ctx: Context` object for client-side logging and interaction.
- ✓ Must test each tool individually using the **in-memory client**.
- ✓ Must handle errors gracefully and report them via the context.
- ⛔️ **NEVER** skip testing before completion.
