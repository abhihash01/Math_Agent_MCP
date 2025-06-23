# Math Agent that uses MCP

Building a math agent that can use different mcp servers.

## 🚀 Features

- **Multi-MCP Integration**: Uses multiple MCP servers for different mathematical operations
- **Conversational Context**: Maintains conversation history across multiple queries
- **Intelligent Tool Selection**: Automatically chooses the right tools based on user queries
- **Error Handling**: Robust error handling with proper validation
- **Extensible Architecture**: Easy to add new MCP servers and mathematical operations

## 🏗️ Architecture

The system consists of:

1. **Main Agent** (`client.py`): LangGraph-based agent that orchestrates multiple MCP servers
2. **Basic Math Server** (`basic_math_server.py`): Basic arithmetic operations
3. **Advanced Math Server** (`advanced_math_server.py`): Power operations and advanced math

```
┌─────────────────┐    
│   Math Agent    │
└─────────┬───────┘
          │
          ├── Basic Math MCP Server
          │   ├── Addition
          │   ├── Subtraction  
          │   ├── Multiplication
          │   └── Division
          │
          └── Advanced Math MCP Server
              ├── Power/Exponentiation
              ├── Square
              ├── Cube
              └── Square Root
```

## 📋 Prerequisites

- Python 3.8+
- Google Cloud credentials (for Vertex AI Gemini model)
- MCP servers running capability

## 🛠️ Installation

1. **Clone the repository**:
   ```bash
   git clone <your-repo-url>
   cd Math_Agent_MCP
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up Google Cloud credentials**:
   ```bash
   # Option 1: Using gcloud CLI
   gcloud auth application-default login
   
   # Option 2: Using service account key
   export GOOGLE_APPLICATION_CREDENTIALS="path/to/your/service-account-key.json"
   ```

## 🚀 Usage

### Basic Usage

Run the main demonstration:

```bash
python client.py
```

This will start the math agent and demonstrate various mathematical operations across multiple queries.

### Example Interactions

The demo includes several example queries:

1. **Basic Arithmetic**: `"What's (3 + 5) × 12?"`
2. **Contextual Follow-up**: `"Now I want to get the square of that result"`
3. **Advanced Operations**: `"What's 2^8 + 15?"`

### Sample Output

```
🧮 LangGraph Multi-MCP Math Agent Demo
========================================

🔍 Query 1: What's (3 + 5) × 12?
----------------------------------------
Elementary Math: 3 + 5 = 8
Elementary Math: 8 × 12 = 96
AI: The result of (3 + 5) × 12 is 96.

🔍 Query 2: Now I want to get the square of that result
----------------------------------------
Advanced Math: 96² = 9216
AI: The square of 96 is 9,216.

🔍 Query 3: What's 2^8 + 15?
----------------------------------------
Advanced Math: 2^8 = 256
Elementary Math: 256 + 15 = 271
AI: 2^8 + 15 equals 271.
```

## 📁 Project Structure

```
├── client.py                           # Main LangGraph agent
├── basic_math_server.py           # Basic math operations MCP server
├── advanced_math_server.py       # Advanced math operations MCP server
├── requirements.txt                    # Python dependencies
└── README.md                          # This file
```

## 🔧 Available Operations

### Elementary Math Server
- **Addition**: `add(a, b)` - Add two integers
- **Subtraction**: `subtract(a, b)` - Subtract two integers
- **Multiplication**: `multiply(a, b)` - Multiply two integers
- **Division**: `divide(a, b)` - Divide with quotient and remainder

### Advanced Math Server
- **Power**: `power(base, exponent)` - Calculate base^exponent
- **Square**: `square(number)` - Calculate number²
- **Cube**: `cube(number)` - Calculate number³
- **Square Root**: `square_root(number)` - Calculate √number

## 🎯 Key Components

### MathAgent Class

The core `MathAgent` class provides:

- **Initialization**: Sets up MCP clients and tools
- **Graph Building**: Creates the LangGraph workflow
- **Query Processing**: Handles individual queries with context
- **Message Management**: Formats and displays conversation history

### LangGraph Workflow

The agent uses a simple but effective workflow:

1. **Model Call**: LLM analyzes the query and decides on tool usage
2. **Tool Execution**: If tools are needed, execute them via MCP servers
3. **Response Generation**: Generate final response based on tool results
4. **Continuation Logic**: Determine if more tool calls are needed

## 🔄 Extending the System

### Adding New MCP Servers

1. **Create a new MCP server**:
   ```python
   from mcp.server.fastmcp import FastMCP
   
   mcp = FastMCP("Your Server Name")
   
   @mcp.tool()
   def your_function(param: int) -> int:
       """Your function description"""
       return param * 2
   
   if __name__ == "__main__":
       mcp.run(transport="stdio")
   ```

2. **Add to client configuration**:
   ```python
   server_config = {
       "your_server": {
           "command": "python",
           "args": ["your_server.py"],
           "transport": "stdio",
       },
       # ... existing servers
   }
   ```

### Adding New Operations

Simply add new `@mcp.tool()` decorated functions to the appropriate server file.

## 🐛 Troubleshooting

### Common Issues

1. **Google Cloud Authentication**:
   ```bash
   # Verify authentication
   gcloud auth list
   gcloud config list
   ```

2. **MCP Server Connection Issues**:
   - Ensure Python scripts are executable
   - Check that MCP servers start without errors
   - Verify stdio transport is working

3. **Tool Not Found Errors**:
   - Check that all MCP servers are properly configured
   - Verify tool names match between server and client

### Debug Mode

Enable detailed logging by modifying the logging level:

```python
logging.basicConfig(level=logging.DEBUG)
```

## 📊 Performance Considerations

- **Tool Caching**: MCP tools are loaded once during initialization
- **Message Context**: Conversation history grows with each query
- **Concurrent Operations**: MCP servers handle requests asynchronously