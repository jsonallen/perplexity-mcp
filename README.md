# perplexity-mcp MCP server

[![smithery badge](https://smithery.ai/badge/perplexity-mcp)](https://smithery.ai/server/perplexity-mcp)

A Model Context Protocol (MCP) server that provides web search functionality using [Perplexity AI's](https://www.perplexity.ai/) API. Works with the [Anthropic](https://www.anthropic.com/news/model-context-protocol) Claude desktop client.

## Example

Let's you use prompts like, "Search the web to find out what's new at Anthropic in the past week."

## Glama Scores

<a href="https://glama.ai/mcp/servers/ebg0za4hn9"><img width="380" height="200" src="https://glama.ai/mcp/servers/ebg0za4hn9/badge" alt="Perplexity Server MCP server" /></a>

## Components

### Prompts

The server provides a single prompt:

- perplexity_search_web: Search the web using Perplexity AI
  - Required "query" argument for the search query
  - Optional "recency" argument to filter results by time period:
    - 'day': last 24 hours
    - 'week': last 7 days
    - 'month': last 30 days (default)
    - 'year': last 365 days
  - Uses Perplexity's API to perform web searches

### Tools

The server implements one tool:

- perplexity_search_web: Search the web using Perplexity AI
  - Takes "query" as a required string argument
  - Optional "recency" parameter to filter results (day/week/month/year)
  - Returns search results from Perplexity's API

## Installation

### Installing via Smithery

To install Perplexity MCP for Claude Desktop automatically via [Smithery](https://smithery.ai/server/perplexity-mcp):

```bash
npx -y @smithery/cli install perplexity-mcp --client claude
```

### Requires [UV](https://github.com/astral-sh/uv) (Fast Python package and project manager)

If uv isn't installed.

```bash
# Using Homebrew on macOS
brew install uv
```

or

```bash
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh

# On Windows.
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Next, install the MCP server

```bash
# Install from PyPi
uv pip install perplexity-mcp
```

or

```bash
# Install from source
uv pip install git+https://github.com/jsonallen/perplexity-mcp.git
```

### Environment Variables

The following environment variable is required in your claude_desktop_config.json. You can obtain an API key from [Perplexity](https://perplexity.ai)

- `PERPLEXITY_API_KEY`: Your Perplexity AI API key

Optional environment variables:

- `PERPLEXITY_MODEL`: The Perplexity model to use (defaults to "sonar" if not specified)

  Available models:

  - `sonar-deep-research`: 128k context - Enhanced research capabilities
  - `sonar-reasoning-pro`: 128k context - Advanced reasoning with professional focus
  - `sonar-reasoning`: 128k context - Enhanced reasoning capabilities
  - `sonar-pro`: 200k context - Professional grade model
  - `sonar`: 128k context - Default model
  - `r1-1776`: 128k context - Alternative architecture

And updated list of models is avaiable (here)[https://docs.perplexity.ai/guides/model-cards]

#### Claude Desktop

Add this tool as a mcp server by editing the Claude config file.

On MacOS: `~/Library/Application\ Support/Claude/claude_desktop_config.json`
On Windows: `%APPDATA%/Claude/claude_desktop_config.json`

```json
  "perplexity-mcp": {
    "env": {
      "PERPLEXITY_API_KEY": "XXXXXXXXXXXXXXXXXXXX",
      "PERPLEXITY_MODEL": "sonar"
    },
    "command": "uv",
    "args": [
      "run",
      "perplexity-mcp"
    ]
  }
```

To verify the server is working. Open the Claude client and use a prompt like "search the web for news about openai in the past week". You should see an alert box open to confirm tool usage. Click "Allow for this chat".

  <img width="600" alt="mcp_screenshot" src="https://github.com/user-attachments/assets/922d8f6a-8c9a-4978-8be6-788e70b4d049" />

### Cursor Installation

You can also use perplexity-mcp as MCP server in Cursor.

Since there no way to set an the the PERPLEXITY_API_KEY in Cursor, you'll have to create a simple wrapper script to set the environment variable.

Example wrapper script:

```
#!/bin/bash

# Set the your Perplexity API key
export PERPLEXITY_API_KEY="pplx-XXXXXXXXXXXX"

# Set the Perplexity model (optional, defaults to "sonar" if not set)
export PERPLEXITY_MODEL="sonar-pro"

# Run the command
uv run perplexity-mcp
```

Once you've completed your script, navigate to the MCP settings in Cursor and Add MCP Server. Set the server name to perplexity-mcp and the type to command. For the command, you want to specify the path of the wrapper script you just created.

 <img width="800" alt="mcp_screenshot" src="https://github.com/user-attachments/assets/375bf6c2-c7c7-4682-a5bb-271af62742ce" width=600>

If everything is working correctly, you should now be able to call the tool from Cursor.
<img width="800" alt="mcp_screenshot" src="https://github.com/user-attachments/assets/d9adef45-b53c-4396-9bd4-dd798a742b48" width=600>
