# AutoGen

**AutoGen** is an open-source framework by Microsoft Research for building multi-agent AI systems. Agents communicate through natural language conversations to collaboratively solve complex tasks.

- **Website:** [microsoft.github.io/autogen](https://microsoft.github.io/autogen)
- **GitHub:** [github.com/microsoft/autogen](https://github.com/microsoft/autogen)
- **Languages:** Python
- **License:** MIT

---

## Key Features

- **Multi-Agent Conversations** – Agents send messages to each other to complete tasks
- **Human-in-the-Loop** – Optionally involve humans at any point in the conversation
- **Code Execution** – Agents can write and run code in isolated environments
- **Flexible Topologies** – Two-agent chats, group chats, nested chats
- **AutoGen Studio** – No-code UI for building multi-agent workflows
- **Model Agnostic** – Works with OpenAI, Azure OpenAI, Anthropic, local models

---

## Installation

```bash
pip install pyautogen

# With code execution support
pip install pyautogen[local-llm]

# AutoGen Studio (no-code UI)
pip install autogenstudio
```

---

## Quick Examples

### Two-Agent Code Generation

```python
import autogen

# Configure the LLM
config_list = [{"model": "gpt-4o", "api_key": "your-api-key"}]
llm_config = {"config_list": config_list}

# The assistant writes code
assistant = autogen.AssistantAgent(
    name="assistant",
    llm_config=llm_config,
)

# The user proxy executes code and gives feedback
user_proxy = autogen.UserProxyAgent(
    name="user_proxy",
    human_input_mode="NEVER",    # fully automated
    max_consecutive_auto_reply=10,
    code_execution_config={"work_dir": "workspace", "use_docker": False},
)

# Start the conversation
user_proxy.initiate_chat(
    assistant,
    message="Write a Python script to fetch the top 5 trending GitHub repositories today."
)
```

### Group Chat with Multiple Agents

```python
import autogen

config_list = [{"model": "gpt-4o", "api_key": "your-api-key"}]
llm_config = {"config_list": config_list}

# Define specialist agents
planner = autogen.AssistantAgent(
    name="Planner",
    system_message="You create detailed task plans. Break work into clear steps.",
    llm_config=llm_config,
)

coder = autogen.AssistantAgent(
    name="Coder",
    system_message="You write Python code to implement the planner's steps.",
    llm_config=llm_config,
)

critic = autogen.AssistantAgent(
    name="Critic",
    system_message="You review code for correctness, efficiency, and security.",
    llm_config=llm_config,
)

user_proxy = autogen.UserProxyAgent(
    name="user_proxy",
    human_input_mode="TERMINATE",
    code_execution_config={"work_dir": "workspace", "use_docker": False},
)

# Create a group chat
group_chat = autogen.GroupChat(
    agents=[user_proxy, planner, coder, critic],
    messages=[],
    max_round=12,
)

manager = autogen.GroupChatManager(
    groupchat=group_chat,
    llm_config=llm_config,
)

user_proxy.initiate_chat(
    manager,
    message="Build a REST API endpoint that returns weather data for a given city."
)
```

---

## Core Concepts

### Agent Types

| Agent | Description |
|-------|-------------|
| `AssistantAgent` | LLM-powered agent that generates responses and code |
| `UserProxyAgent` | Represents the user; can execute code and solicit human input |
| `GroupChatManager` | Manages turn-taking in a multi-agent group chat |
| `ConversableAgent` | Base class for custom agents |

### Conversation Patterns

```
Two-Agent Chat:
  UserProxy ←→ Assistant

Group Chat:
  UserProxy → GroupChatManager → Agent1 / Agent2 / Agent3

Nested Chat:
  Agent1 → (inner: Agent2 ←→ Agent3) → Agent1
```

---

## When to Use AutoGen

✅ Automating software engineering tasks (code, tests, reviews)  
✅ Building multi-agent pipelines where agents debate and refine answers  
✅ Research workflows requiring iterative reasoning  
✅ Tasks that benefit from code execution in a feedback loop  
✅ Rapid prototyping of multi-agent systems  

⚠️ For structured, role-based agent teams with defined processes, consider [CrewAI](crewai.md).

---

## Further Reading

- [AutoGen Documentation](https://microsoft.github.io/autogen/docs/Getting-Started)
- [AutoGen Examples](https://github.com/microsoft/autogen/tree/main/notebook)
- [AutoGen Studio](https://microsoft.github.io/autogen/docs/autogen-studio/getting-started)
