# perplexity-mcp MCP server

A Model Context Protocol (MCP) server that provides web search functionality using [Perplexity AI's](https://www.perplexity.ai/) API.  Works with the [Anthropic](https://www.anthropic.com/news/model-context-protocol) Claude desktop client. 

<a href="https://glama.ai/mcp/servers/ebg0za4hn9"><img width="380" height="200" src="https://glama.ai/mcp/servers/ebg0za4hn9/badge?v1" alt="Perplexity Server MCP server" /></a>

## Components

### Prompts

The server provides a single prompt:
- perplexity_search_web: Search the web using Perplexity AI
  - Required "query" argument for the search query
  - Uses Perplexity's API to perform web searches

### Tools

The server implements one tool:
- perplexity_search_web: Search the web using Perplexity AI
  - Takes "query" as a required string argument
  - Returns search results from Perplexity's API

## Installation

### Using uv (recommended)

```bash
# Install from source
uv pip install git+https://github.com/jsonallen/perplexity-mcp.git

# Or install in development mode
git clone https://github.com/jasonallen/perplexity-mcp.git
cd perplexity-mcp
uv pip install -e .
```

### Environment Variables

The following environment variable is required in your claude_desktop_config.json. You can obtain an API key from [Perplexity](https://perplexity.ai)

- `PERPLEXITY_API_KEY`: Your Perplexity AI API key


#### Claude Desktop

Add this tool as a mcp server

On MacOS: `~/Library/Application\ Support/Claude/claude_desktop_config.json`
On Windows: `%APPDATA%/Claude/claude_desktop_config.json`


  ```json
    "perplexity-mcp": {
      "env": {
        "PERPLEXITY_API_KEY": "123456790"
      },
      "command": "uv",
      "args": [
        "--directory",
        "/ABSOLUTE/PATH/TO/perplexity-mcp",
        "run",
        "perplexity-mcp"
      ]
    }
  ```
