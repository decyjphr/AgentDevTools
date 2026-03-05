# LlamaIndex

**LlamaIndex** (formerly GPT Index) is a data framework for building LLM-powered applications over custom data. It specializes in Retrieval-Augmented Generation (RAG) and provides tools for ingesting, indexing, and querying diverse data sources.

- **Website:** [llamaindex.ai](https://www.llamaindex.ai)
- **GitHub:** [github.com/run-llama/llama_index](https://github.com/run-llama/llama_index)
- **Languages:** Python, TypeScript
- **License:** MIT

---

## Key Features

- **Data Connectors** – Load data from PDFs, databases, APIs, Notion, Slack, and 100+ sources
- **Indexes** – Vector Store Index, Summary Index, Knowledge Graph Index
- **Query Engine** – Ask questions over your data with citations
- **Chat Engine** – Conversational interface with memory
- **Agents** – ReAct and OpenAI function-calling agents
- **Workflows** – Event-driven orchestration for complex pipelines
- **Observability** – Built-in tracing and evaluation

---

## Installation

```bash
# Python (core)
pip install llama-index

# With OpenAI support
pip install llama-index llama-index-llms-openai llama-index-embeddings-openai

# TypeScript
npm install llamaindex
```

---

## Quick Examples

### Basic Document Q&A

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
from llama_index.llms.openai import OpenAI
from llama_index.core import Settings

# Configure the LLM
Settings.llm = OpenAI(model="gpt-4o", temperature=0)

# Load documents from a directory (replace "./my_docs" with your actual path)
documents = SimpleDirectoryReader("./my_docs").load_data()

# Build the index
index = VectorStoreIndex.from_documents(documents)

# Query the index
query_engine = index.as_query_engine()
response = query_engine.query("What are the main topics in these documents?")
print(response)
```

### Chat with Your Data

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader("./my_docs").load_data()
index = VectorStoreIndex.from_documents(documents)

# Create a chat engine with memory
chat_engine = index.as_chat_engine(chat_mode="condense_plus_context")

response = chat_engine.chat("Tell me about the AI tools mentioned.")
print(response)

response = chat_engine.chat("Which one would you recommend for beginners?")
print(response)
```

### ReAct Agent with Tools

```python
from llama_index.core.agent import ReActAgent
from llama_index.llms.openai import OpenAI
from llama_index.core.tools import FunctionTool

def multiply(a: float, b: float) -> float:
    """Multiply two numbers."""
    return a * b

def add(a: float, b: float) -> float:
    """Add two numbers."""
    return a + b

tools = [
    FunctionTool.from_defaults(fn=multiply),
    FunctionTool.from_defaults(fn=add),
]

llm = OpenAI(model="gpt-4o")
agent = ReActAgent.from_tools(tools, llm=llm, verbose=True)

response = agent.chat("What is (3 + 5) * 12?")
print(response)
```

---

## Core Concepts

### Data Ingestion Pipeline

```
Raw Data → Data Connectors → Documents → Node Parser → Nodes → Index
```

| Component | Description |
|-----------|-------------|
| **Data Connectors** | Load data from files, APIs, databases |
| **Documents** | Raw text units with metadata |
| **Node Parser** | Split documents into chunks (nodes) |
| **Index** | Stores nodes with embeddings for retrieval |

### Index Types

| Index Type | Best For |
|------------|----------|
| `VectorStoreIndex` | Semantic similarity search over large datasets |
| `SummaryIndex` | Summarization over all documents |
| `KnowledgeGraphIndex` | Relationship-based queries |
| `KeywordTableIndex` | Keyword-based retrieval |

---

## When to Use LlamaIndex

✅ Building RAG applications over custom documents  
✅ Creating document Q&A systems with citations  
✅ Connecting to diverse data sources (PDFs, databases, APIs)  
✅ Building knowledge base chatbots  
✅ You need fine-grained control over indexing and retrieval  

⚠️ For general-purpose agent pipelines with many integrations, consider [LangChain](langchain.md).

---

## Further Reading

- [LlamaIndex Documentation](https://docs.llamaindex.ai)
- [LlamaIndex Examples](https://github.com/run-llama/llama_index/tree/main/docs/docs/examples)
- [LlamaHub (Data Connectors)](https://llamahub.ai)
