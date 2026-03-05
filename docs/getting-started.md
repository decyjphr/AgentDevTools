# Getting Started

This guide introduces the core concepts of AI Agents and helps you choose and set up your first agent framework.

---

## Core Concepts

### What Is an LLM?

A **Large Language Model (LLM)** is a neural network trained on vast amounts of text data that can understand and generate human language. Examples include GPT-4, Claude, Gemini, and Llama.

### What Makes Something an "Agent"?

An agent is an LLM-powered system that can:

1. **Perceive** its environment (read documents, query APIs, observe tool outputs)
2. **Reason** about what to do next (using chain-of-thought or planning strategies)
3. **Act** by calling tools, writing code, or invoking other agents
4. **Learn** by updating its memory or state based on outcomes

### Key Components

```
┌─────────────────────────────────────────────┐
│                  AI Agent                   │
│                                             │
│  ┌──────────┐   ┌──────────┐  ┌─────────┐  │
│  │   LLM    │   │  Memory  │  │  Tools  │  │
│  │ (Reason) │◄──│(Context) │  │(Actions)│  │
│  └──────────┘   └──────────┘  └─────────┘  │
│        │                           ▲        │
│        └───────────────────────────┘        │
└─────────────────────────────────────────────┘
```

| Component | Description |
|-----------|-------------|
| **LLM** | The reasoning engine (GPT-4, Claude, etc.) |
| **Memory** | Short-term (context window) and long-term (vector stores, databases) |
| **Tools** | Functions the agent can call (web search, code execution, APIs) |
| **Orchestrator** | Logic that routes tasks and manages agent lifecycles |

---

## Choosing a Framework

Use this quick decision guide:

```
Are you building a RAG (Retrieval-Augmented Generation) system?
  └─ Yes → Consider LlamaIndex
  └─ No  → Do you need multiple collaborating agents?
              └─ Yes → Consider AutoGen or CrewAI
              └─ No  → Do you need enterprise / .NET / Java support?
                          └─ Yes → Consider Semantic Kernel
                          └─ No  → LangChain is a solid general-purpose choice
```

See the [Tool Comparison guide](guides/comparison.md) for a detailed breakdown.

---

## Your First Agent with LangChain

### 1. Install dependencies

```bash
pip install langchain langchain-openai
```

### 2. Set up your API key

```bash
export OPENAI_API_KEY="your-api-key-here"
```

### 3. Write your first agent

```python
from langchain_openai import ChatOpenAI
from langchain.agents import create_react_agent, AgentExecutor
from langchain_community.tools import DuckDuckGoSearchRun
from langchain import hub

# Initialize the LLM
llm = ChatOpenAI(model="gpt-4o", temperature=0)

# Give the agent a tool
tools = [DuckDuckGoSearchRun()]

# Pull a standard ReAct prompt
prompt = hub.pull("hwchase17/react")

# Create the agent
agent = create_react_agent(llm, tools, prompt)
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

# Run it
result = agent_executor.invoke({
    "input": "What are the latest AI agent frameworks released in 2024?"
})
print(result["output"])
```

### 4. Run the script

```bash
python agent.py
```

---

## Next Steps

- Explore individual [tool documentation](tools/index.md)
- Compare frameworks in the [comparison guide](guides/comparison.md)
- Check each tool's official documentation for advanced features
