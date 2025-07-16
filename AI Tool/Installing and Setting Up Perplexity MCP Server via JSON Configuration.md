- [ ] **Integrating Perplexity API with AI Assistants via Alcova-AI MCP Server** ➕ 2025-07-15 

### What are the installation steps for `perplexity-mcp`?

To install `perplexity-mcp`, you first need to add the Alcova-AI tap to Homebrew and then install the package.

```bash
# Add the Alcova-AI tap
brew tap alcova-ai/tap

# Then install
brew install perplexity-mcp
```

### How is the `perplexity-mcp` server configured?

The `perplexity-mcp` server is configured using a JSON object that specifies the server's behavior. This configuration is typically added to the settings file of your AI assistant (e.g., Claude Desktop).

```json
{
  "mcpServers": {
    "perplexity-mcp": {
      "type": "stdio",
      "command": "perplexity-mcp",
      "args": [
        "--model", "sonar-pro",
        "--reasoning-model", "sonar-reasoning-pro"
      ],
      "env": {
        "PERPLEXITY_API_KEY": "pplx-YOUR-API-KEY-HERE"
      }
    }
  }
}
```

#### Configuration Details

- **`type`: "stdio"**
    
    - This specifies that the communication with the MCP server will happen over standard input/output.
        
- **`command`: "perplexity-mcp"**
    
    - This is the executable command that starts the MCP server.
        
- **`args`: [...]**
    
    - These are the arguments passed to the command.
        
        - `--model sonar-pro`: Sets the default model for search queries to `sonar-pro`.
            
        - `--reasoning-model sonar-reasoning-pro`: Designates the `sonar-reasoning-pro` model for more complex reasoning tasks.
            
- **`env`: {...}**
    
    - This section sets environment variables for the server process.
        
        - `PERPLEXITY_API_KEY`: You must replace `"pplx-YOUR-API-KEY-HERE"` with your actual Perplexity API key.
            

### Summary

**Installation**

- **Homebrew Tap**: Add the Alcova-AI tap to Homebrew.
    
    - `brew tap alcova-ai/tap`
        
- **Installation Command**: Install `perplexity-mcp` using Homebrew.
    
    - `brew install perplexity-mcp`
        

**Configuration (`mcpServers`)**

- **Server Type (`type`)**:
    
    - `stdio`: Communication via standard input/output.
        
- **Execution Command (`command`)**:
    
    - `perplexity-mcp`: The command to launch the server.
        
- **Command Arguments (`args`)**:
    
    - `--model sonar-pro`: Utilizes the `sonar-pro` model for search-related queries.
        
    - `--reasoning-model sonar-reasoning-pro`: Employs the `sonar-reasoning-pro` model for tasks requiring in-depth reasoning.
        
- **Environment Variables (`env`)**:
    
    - `PERPLEXITY_API_KEY`: Your personal API key for Perplexity services.