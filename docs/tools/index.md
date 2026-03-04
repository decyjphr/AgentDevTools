# Tools Overview

This section provides documentation for the most widely-used frameworks and libraries for building AI Agents.

---

## Available Tools

### [LangChain](langchain.md)

The most popular general-purpose framework for building LLM-powered applications and agents.  
**Best for:** Chains, RAG pipelines, tool-calling agents  
**Languages:** Python, JavaScript/TypeScript

---

### [LlamaIndex](llamaindex.md)

A data framework focused on connecting LLMs to external data sources for Retrieval-Augmented Generation (RAG).  
**Best for:** RAG systems, document Q&A, knowledge base agents  
**Languages:** Python, TypeScript

---

### [AutoGen](autogen.md)

A Microsoft framework for building multi-agent conversation systems where agents collaborate via chat.  
**Best for:** Multi-agent workflows, code generation, task automation  
**Languages:** Python

---

### [CrewAI](crewai.md)

A framework for orchestrating role-based multi-agent teams, inspired by how human teams work.  
**Best for:** Role-based agents, collaborative tasks, structured pipelines  
**Languages:** Python

---

### [Semantic Kernel](semantic-kernel.md)

Microsoft's SDK for integrating LLMs into enterprise applications, with strong support for .NET, Python, and Java.  
**Best for:** Enterprise integration, plugin-based orchestration, .NET/Java environments  
**Languages:** Python, C#, Java

---

## How to Choose

| Need | Recommended Tool |
|------|-----------------|
| Build a chatbot with memory | LangChain |
| Query your documents with AI | LlamaIndex |
| Automate tasks with multiple AI agents | AutoGen or CrewAI |
| Integrate AI into an enterprise .NET app | Semantic Kernel |
| Research and experiment quickly | LangChain |

See the [detailed comparison guide](../guides/comparison.md) for more information.
