# 🤖 Google ADK Multi-Agent System

<div align="center">

![Python](https://img.shields.io/badge/Python-3.12+-blue.svg)
![Status](https://img.shields.io/badge/Status-Active-success.svg)

**A powerful multi-agent system built with Google Agent Development Kit (ADK) for intelligent research and mythology queries**

🚧 **Agentic Workflow Building in Progress** 🚧

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [Architecture](#-architecture) • [Contributing](#-contributing)

</div>

---

## 📋 Table of Contents

- [✨ Features](#-features)
- [🏗️ Architecture](#️-architecture)
- [🚀 Installation](#-installation)
- [⚙️ Configuration](#️-configuration)
- [💻 Usage](#-usage)
- [🔧 Project Structure](#-project-structure)
- [📦 Dependencies](#-dependencies)
- [🤝 Contributing](#-contributing)

---

## ✨ Features

- 🎯 **Multi-Agent Architecture**: Intelligent agent orchestration with supervisor pattern
- 🔍 **Web Search Integration**: DuckDuckGo search capabilities for real-time information
- 🧠 **Advanced LLM Support**: Powered by LiteLLM with Groq's Qwen3-32B model
- 🎭 **Specialized Agents**: Dedicated agents for research and mythology queries
- 🔄 **Supervisor Pattern**: Centralized coordination for multi-agent workflows
- ⚡ **Fast & Efficient**: Optimized for performance with modern AI models
- 🛠️ **Extensible Design**: Easy to add new agents and tools

---

## 🏗️ Architecture

This project implements a **supervisor-agent pattern** with the following components:

```
┌─────────────────────────────────────┐
│     Supervisor Agent (Root)        │
│  Coordinates & synthesizes outputs │
└──────────────┬──────────────────────┘
               │
       ┌───────┴────────┐
       │                │
┌──────▼──────┐  ┌──────▼──────┐
│ Mythology   │  │  Research   │
│   Agent     │  │   Agent     │
│             │  │             │
│ 🎭 Answers  │  │ 🔬 Performs │
│ mythology   │  │   research  │
│ questions   │  │   queries   │
└─────────────┘  └─────────────┘
```

### 🤖 Agent Descriptions

| Agent | Purpose | Tools | Output Key |
|-------|---------|-------|------------|
| **Research Agent** 🔬 | Performs comprehensive web research using DuckDuckGo | `duckduckgo_search` | `research` |
| **Mythology Agent** 🎭 | Answers questions about mythology and legends | `duckduckgo_search` | `mythology` |
| **Supervisor Agent** 👑 | Coordinates sub-agents and generates final responses | Sub-agents | N/A |

---

## 🚀 Installation

### Prerequisites

- Python 3.12 or higher
- pip package manager
- Virtual environment (recommended)

### Step-by-Step Setup

1. **Clone the repository** (if applicable)
   ```bash
   git clone <repository-url>
   cd Google-ADK
   ```

2. **Create and activate a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   ```bash
   cp .env.example .env  # If you have an example file
   # Edit .env with your API keys and configuration
   ```

---

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# Groq API Configuration
GROQ_API_KEY=your_groq_api_key_here

# Other configuration variables
# Add as needed
```

### Model Configuration

The system uses **Groq's Qwen3-32B** model by default. You can modify the model in `my_agent/agent.py`:

```python
model = LiteLlm(
    model="groq/qwen/qwen3-32b",  # Change to your preferred model
)
```

---

## 💻 Usage

### Basic Usage

```python
from my_agent.agent import root_agent

# Use the supervisor agent (root_agent)
response = root_agent.run("What is the story of Zeus in Greek mythology?")
print(response)
```

### Using Individual Agents

```python
from my_agent.agent import research_agent, mythology_agent

# Research query
research_result = research_agent.run("Latest developments in AI")

# Mythology query
mythology_result = mythology_agent.run("Tell me about Norse gods")
```

### Example Workflow

```python
# The supervisor agent automatically coordinates sub-agents
query = "Compare Greek and Norse mythology"

# Supervisor will:
# 1. Route to mythology_agent for mythology information
# 2. Route to research_agent for comparative research
# 3. Synthesize the final response
result = root_agent.run(query)
```

---

## 🔧 Project Structure

```
Google-ADK/
│
├── 📁 my_agent/                    # Main agent module
│   ├── __init__.py                 # Package initialization
│   ├── agent.py                    # 🎯 Main agent definitions
│   ├── mythology_instructions.txt  # Mythology agent instructions
│   └── research_instructions.txt   # Research agent instructions
│
├── 📁 gadk/                        # Virtual environment (if present)
│
├── 📄 requirements.txt             # 📦 Project dependencies
├── 📄 README.md                    # 📖 This file
└── 📄 .env                         # 🔐 Environment variables (create this)
```

---

## 📦 Dependencies

| Package | Purpose | Version |
|---------|---------|---------|
| `google-adk` | Google Agent Development Kit | Latest |
| `python-dotenv` | Environment variable management | Latest |
| `litellm` | LLM abstraction layer | Latest |
| `duckduckgo_search` | DuckDuckGo search integration | Latest |
| `ddgs` | DuckDuckGo search client | Latest |

### Install All Dependencies

```bash
pip install -r requirements.txt
```

---

## 🎯 Functionalities

### 🔍 Web Search Capability

The system includes a custom DuckDuckGo search tool that:
- Performs web searches with up to 5 results
- Returns structured search results
- Integrates seamlessly with agent workflows

```python
def duckduckgo_search(query: str) -> List:
    """
    Perform a web search using DuckDuckGo.
    Returns a list of search results.
    """
    results = DDGS().text(query, max_results=5)
    return results
```

### 🤖 Agent Capabilities

#### Research Agent 🔬
- Conducts comprehensive web research
- Gathers information from multiple sources
- Synthesizes research findings
- Output key: `research`

#### Mythology Agent 🎭
- Answers mythology-related questions
- Provides detailed explanations about myths and legends
- Covers various mythological traditions
- Output key: `mythology`

#### Supervisor Agent 👑
- Coordinates multiple sub-agents
- Routes queries to appropriate agents
- Synthesizes outputs from sub-agents
- Generates comprehensive final responses

---

## 🛠️ Customization

### Adding a New Agent

1. **Define the agent function**:
   ```python
   def my_custom_agent(query: str) -> str:
       # Your agent logic here
       return result
   ```

2. **Create the agent**:
   ```python
   custom_agent = Agent(
       name="custom_agent",
       model=model,
       description="Your agent description",
       instruction="Your agent instructions",
       tools=[your_tools],
       output_key="custom",
   )
   ```

3. **Add to supervisor**:
   ```python
   supervisor_agent = Agent(
       # ... other parameters
       sub_agents=[mythology_agent, research_agent, custom_agent],
   )
   ```

### Creating Custom Tools

```python
from google.adk.tools import FunctionTool

def my_custom_tool(input: str) -> str:
    """Tool description"""
    # Tool implementation
    return result

custom_tool = FunctionTool(func=my_custom_tool)
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. 🍴 Fork the repository
2. 🌿 Create a feature branch (`git checkout -b feature/amazing-feature`)
3. 💾 Commit your changes (`git commit -m 'Add amazing feature'`)
4. 📤 Push to the branch (`git push origin feature/amazing-feature`)
5. 🔀 Open a Pull Request

### Contribution Guidelines

- ✨ Follow the existing code style
- 📝 Add comments for complex logic
- 🧪 Include tests for new features
- 📖 Update documentation as needed

---

## 🙏 Acknowledgments

- **Google ADK** for the Agent Development Kit
- **LiteLLM** for LLM abstraction
- **Groq** for providing fast inference
- **DuckDuckGo** for search capabilities

---

## 📞 Support

If you encounter any issues or have questions:

- 🐛 Open an issue on GitHub
- 💬 Start a discussion
- 📧 Contact the maintainers

---

<div align="center">

**Made with ❤️ using Google ADK**

⭐ Star this repo if you find it helpful!

---

🚧 **Agentic Workflow Building in Progress** 🚧

</div>

