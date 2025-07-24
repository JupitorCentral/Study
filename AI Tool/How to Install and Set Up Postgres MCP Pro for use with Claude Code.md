
- [x] How to Install and Set Up Postgres MCP Pro for use with Claude Code ➕ 2025-07-15 ✅ 2025-07-22

### Prerequisites

#### 1. Python Version Requirements

- **Required**: Python 3.12 or higher
    
- **Recommended**: Python 3.13.5 (latest stable)
    

**Install Python 3.13 on macOS:**

```bash
# Using Homebrew (recommended)
brew install python@3.13

# Verify installation
python3.13 --version
```

#### 2. PostgreSQL Setup

- Ensure PostgreSQL is running (via Postgres.app or Homebrew)
    
- Database should be accessible and properly indexed
    

### Installation Steps

#### Step 1: Create Virtual Environment

```bash
# Create project directory
mkdir ~/Projects/postgres-mcp-project
cd ~/Projects/postgres-mcp-project

# Create virtual environment with Python 3.13
python3.13 -m venv .venv

# Activate virtual environment
source .venv/bin/activate
```

#### Step 2: Install Postgres MCP Pro

```bash
# Install using pip (ensure virtual environment is activated)
pip install postgres-mcp

# Verify installation
pip list | grep postgres-mcp
```

#### Step 3: Test Installation

```bash
# Test if uv can run the package
uv run postgres-mcp --help
```

### Claude Code Configuration

#### Step 1: Locate Configuration File

The Claude Code configuration file is located at:


```
~/.claude.json
```

#### Step 2: Add MCP Server Configuration

Add the following configuration to your `~/.claude.json` file:


> *****!!! Need to set per-project inside ~/.claude.json file for project specfiic location, not like this !!!*****

```json
{
  "mcpServers": {
    "postgres-mcp": {
      "command": "uv",
      "args": [
        "--directory", "/Users/your_username/Projects/postgres-mcp-project",
        "run",
        "postgres-mcp",
        "--access-mode=unrestricted"
      ],
      "env": {
        "DATABASE_URI": "postgresql://username:password@localhost:5432/dbname"
      }
    }
  }
}
```

**Replace the following values:**

- `/Users/your_username/Projects/postgres-mcp-project` → Your actual project path
    
- `username:password@localhost:5432/dbname` → Your PostgreSQL connection details
    

#### Step 3: Access Modes

Choose the appropriate access mode:

**For Development (Unrestricted):**

```json
"args": ["--access-mode=unrestricted"]
```

**For Production (Restricted/Read-only):**

```json
"args": ["--access-mode=restricted"]
```

### Database Connection Setup

#### Connection String Format

```
postgresql://username:password@host:port/database
```

**Example for Postgres.app:**

```
postgresql://your_username:@localhost:5432/your_database
```

#### Test Database Connection


```bash
# Test PostgreSQL connection
psql -h localhost -p 5432 -U your_username -d your_database -c "SELECT version();"
```

### Optional Enhancements

#### Install PostgreSQL Extensions

For advanced features, install these extensions in your database:

```sql
-- Connect to your database
psql your_database

-- Install extensions
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
CREATE EXTENSION IF NOT EXISTS hypopg;
```

**These extensions enable:**

- **pg_stat_statements**: Query execution statistics
    
- **hypopg**: Hypothetical index simulation
    

### Verification

#### 1. Check Claude Code Integration

1. Restart Claude Code
    
2. Look for the hammer icon (🔨) indicating MCP tools are available
    
3. Test by asking: _"Show me my database tables"_
    

#### 2. Test Database Operations

Ask Claude Code to perform these operations:

- _"Analyze my database health"_
    
- _"What are my database tables?"_
    
- _"Check for slow queries"_
    
- _"Suggest performance improvements"_
    

### Troubleshooting

#### Common Issues

**Python Version Error:**

```bash
# If you get Python version errors
python3.13 --version # Verify Python 3.13 is installed
```

**Module Not Found:**

```bash
# Ensure virtual environment is activated
source .venv/bin/activate
pip install postgres-mcp
```

**Connection Errors:**

- Verify PostgreSQL is running
    
- Check DATABASE_URI format
    
- Ensure database credentials are correct
    

#### Debug Commands

```bash
# Test manual execution
cd ~/Projects/postgres-mcp-project
source .venv/bin/activate
uv run postgres-mcp --help

# Check Claude Code logs
claude --debug
```

### Key Features Available

With Postgres MCP Pro successfully installed, you'll have access to:

- **Database Health Analysis** - Index health, connection monitoring
    
- **Advanced Index Tuning** - AI-powered index recommendations
    
- **Query Plan Analysis** - EXPLAIN plans and optimization
    
- **Schema Intelligence** - Context-aware SQL generation
    
- **Safe SQL Execution** - Configurable access controls
    

### Summary

**Prerequisites**

- Python: 3.12+ (3.13.5 recommended)
    
- PostgreSQL: Running and accessible instance
    

**Core Installation Process**

- Create a dedicated project directory.
    
- Set up a Python virtual environment (`python3.13 -m venv .venv`).
    
- Activate the environment (`source .venv/bin/activate`).
    
- Install the package via pip (`pip install postgres-mcp`).
    

**Claude Code Configuration (`~/.claude.json`)**

- Define an `mcpServers` entry for `postgres-mcp`.
    
- Set the `command` to `uv` and provide the `args` pointing to your project directory.
    
- Configure the `DATABASE_URI` environment variable with your connection string.
    
- Set the `--access-mode` to `unrestricted` for development or `restricted` for production.
    

**Database Setup**

- Connection String: `postgresql://username:password@host:port/database`.
    
- Test connection using `psql`.
    
- Optional Extensions: `pg_stat_statements` (query stats), `hypopg` (hypothetical indexes).
    

**Verification and Usage**

- Restart Claude Code and look for the tool icon (🔨).
    
- Test with commands like "Analyze my database health" or "Check for slow queries".
    

**Available Features**

- Database Health Analysis
    
- Advanced Index Tuning
    
- Query Plan Analysis
    
- Schema Intelligence
    
- Safe SQL Execution