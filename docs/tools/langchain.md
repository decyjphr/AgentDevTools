# LangChain

**LangChain** is the most widely used framework for building applications powered by large language models. It provides composable abstractions for chains, agents, memory, and retrieval-augmented generation.

- **Website:** [langchain.com](https://www.langchain.com)
- **GitHub:** [github.com/langchain-ai/langchain](https://github.com/langchain-ai/langchain)
- **Languages:** Python, JavaScript/TypeScript
- **License:** MIT

---

## Key Features

- **Chains** – Compose sequences of LLM calls and tool invocations
- **Agents** – Autonomous reasoning loops (ReAct, OpenAI Functions, etc.)
- **Memory** – Conversation history, entity memory, summary memory
- **Retrievers** – Connect to vector stores, document loaders, and search APIs
- **LangSmith** – Observability and tracing platform
- **LangServe** – Deploy chains as REST APIs

---

## Installation

```bash
# Python
pip install langchain langchain-openai langchain-community

# JavaScript / TypeScript
npm install langchain @langchain/openai
```

---

## Quick Examples

### Simple LLM Call

```python
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage

llm = ChatOpenAI(model="gpt-4o")
response = llm.invoke([HumanMessage(content="What is an AI agent?")])
print(response.content)
```

### Chain with Prompt Template

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate

llm = ChatOpenAI(model="gpt-4o")
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("user", "{question}")
])

chain = prompt | llm
result = chain.invoke({"question": "Explain AI agents in one paragraph."})
print(result.content)
```

### Tool-Calling Agent

```python
from langchain_openai import ChatOpenAI
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain_core.prompts import ChatPromptTemplate
from langchain_community.tools import DuckDuckGoSearchRun

llm = ChatOpenAI(model="gpt-4o")
tools = [DuckDuckGoSearchRun()]

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("user", "{input}"),
    ("placeholder", "{agent_scratchpad}"),
])

agent = create_tool_calling_agent(llm, tools, prompt)
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

result = agent_executor.invoke({"input": "Search for the latest LangChain release."})
print(result["output"])
```

### RAG Pipeline

```python
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_community.vectorstores import FAISS
from langchain_community.document_loaders import TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.chains import RetrievalQA

# Load and split documents
loader = TextLoader("my_document.txt")
documents = loader.load()
splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = splitter.split_documents(documents)

# Create a vector store
embeddings = OpenAIEmbeddings()
vectorstore = FAISS.from_documents(chunks, embeddings)

# Build the QA chain
llm = ChatOpenAI(model="gpt-4o")
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever()
)

answer = qa_chain.invoke({"query": "What does the document say about AI agents?"})
print(answer["result"])
```

---

## Core Concepts

### LCEL (LangChain Expression Language)

LCEL uses the pipe operator (`|`) to compose chains declaratively:

```python
chain = prompt | llm | output_parser
result = chain.invoke({"input": "Hello"})
```

### Runnable Interface

Every component in LangChain implements the `Runnable` interface, providing:

| Method | Description |
|--------|-------------|
| `.invoke()` | Run synchronously with a single input |
| `.batch()` | Run on a list of inputs |
| `.stream()` | Stream output tokens |
| `.ainvoke()` | Async version of invoke |

---

## When to Use LangChain

✅ Building chatbots and conversational agents  
✅ Creating RAG pipelines with document Q&A  
✅ Prototyping quickly with many LLM providers  
✅ Connecting to external tools and APIs  
✅ You need a large ecosystem of integrations  

⚠️ For complex multi-agent orchestration, consider [AutoGen](autogen.md) or [CrewAI](crewai.md).

---

## Further Reading

- [LangChain Documentation](https://python.langchain.com)
- [LangChain Cookbook](https://github.com/langchain-ai/langchain/tree/master/cookbook)
- [LangSmith (Observability)](https://smith.langchain.com)
