# CrewAI

**CrewAI** is a framework for orchestrating role-playing, autonomous AI agents. It enables groups of agents to work together as a "crew," each with defined roles, goals, and tools, collaborating to accomplish complex tasks.

- **Website:** [crewai.com](https://www.crewai.com)
- **GitHub:** [github.com/crewAIInc/crewAI](https://github.com/crewAIInc/crewAI)
- **Languages:** Python
- **License:** MIT

---

## Key Features

- **Role-Based Agents** – Assign each agent a name, role, goal, and backstory
- **Task Assignment** – Define tasks with expected outputs and assign them to agents
- **Sequential & Hierarchical Processes** – Control how tasks are executed
- **Tool Integration** – Equip agents with web search, code execution, and custom tools
- **Inter-Agent Delegation** – Agents can delegate subtasks to other agents
- **Memory & Context Sharing** – Agents share context across tasks

---

## Installation

```bash
pip install crewai crewai-tools
```

---

## Quick Examples

### Basic Crew

```python
from crewai import Agent, Task, Crew, Process
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o", temperature=0.7)

# Define agents with roles
researcher = Agent(
    role="Senior Research Analyst",
    goal="Uncover cutting-edge developments in AI agent frameworks",
    backstory=(
        "You are an expert analyst at a tech research firm with a passion for "
        "discovering emerging trends in AI."
    ),
    llm=llm,
    verbose=True,
)

writer = Agent(
    role="Tech Content Writer",
    goal="Craft compelling, easy-to-understand articles about AI tools",
    backstory=(
        "You are an experienced technical writer who translates complex AI "
        "concepts into clear, engaging content for developers."
    ),
    llm=llm,
    verbose=True,
)

# Define tasks
research_task = Task(
    description="Research the top 5 AI agent frameworks in 2024 and summarize their key features.",
    expected_output="A bullet-point summary of the top 5 frameworks with their strengths.",
    agent=researcher,
)

writing_task = Task(
    description="Write a 500-word blog post based on the research summary.",
    expected_output="A polished blog post ready for publication.",
    agent=writer,
)

# Assemble the crew
crew = Crew(
    agents=[researcher, writer],
    tasks=[research_task, writing_task],
    process=Process.sequential,  # tasks run in order
    verbose=True,
)

result = crew.kickoff()
print(result)
```

### Crew with Web Search Tool

```python
from crewai import Agent, Task, Crew, Process
from crewai_tools import SerperDevTool
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o")
search_tool = SerperDevTool()

researcher = Agent(
    role="AI Trends Researcher",
    goal="Find the latest news about AI agents",
    backstory="You are a diligent researcher who stays current with AI news.",
    tools=[search_tool],
    llm=llm,
)

task = Task(
    description="Search for the 3 most recent AI agent framework releases and summarize them.",
    expected_output="A numbered list of 3 recent releases with brief descriptions.",
    agent=researcher,
)

crew = Crew(agents=[researcher], tasks=[task], verbose=True)
result = crew.kickoff()
print(result)
```

---

## Core Concepts

### Agents

An agent has four defining properties:

| Property | Description |
|----------|-------------|
| `role` | The agent's job title (e.g., "Data Scientist") |
| `goal` | What the agent is trying to achieve |
| `backstory` | Provides context that shapes the agent's behavior |
| `tools` | List of tools the agent can use |

### Tasks

A task defines:

| Property | Description |
|----------|-------------|
| `description` | What needs to be done |
| `expected_output` | Format or content of the result |
| `agent` | Which agent is responsible |

### Process Types

| Process | Description |
|---------|-------------|
| `Process.sequential` | Tasks execute one after another |
| `Process.hierarchical` | A manager agent delegates and reviews work |

---

## When to Use CrewAI

✅ Building structured workflows with specialized agent roles  
✅ Content creation pipelines (research → write → review)  
✅ Business process automation with defined stages  
✅ Teams where agents need to delegate to each other  
✅ Projects where role clarity improves output quality  

⚠️ For free-form multi-agent conversations and code execution loops, consider [AutoGen](autogen.md).

---

## Further Reading

- [CrewAI Documentation](https://docs.crewai.com)
- [CrewAI Examples](https://github.com/crewAIInc/crewAI-examples)
- [CrewAI Tools](https://github.com/crewAIInc/crewAI-tools)
