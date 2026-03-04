# Semantic Kernel

**Semantic Kernel** is an open-source SDK by Microsoft that integrates large language models (LLMs) into enterprise applications. It provides a plugin-based architecture and supports Python, C#, and Java, making it ideal for organizations with existing enterprise stacks.

- **Website:** [learn.microsoft.com/semantic-kernel](https://learn.microsoft.com/en-us/semantic-kernel/overview/)
- **GitHub:** [github.com/microsoft/semantic-kernel](https://github.com/microsoft/semantic-kernel)
- **Languages:** Python, C#, Java
- **License:** MIT

---

## Key Features

- **Plugins** – Encapsulate AI functions and native code as reusable components
- **Planner** – Automatically selects and sequences plugins to achieve goals
- **Memory** – Semantic search over stored memories using embeddings
- **Process Framework** – Orchestrate long-running, stateful business processes
- **Multi-Model Support** – OpenAI, Azure OpenAI, Hugging Face, and more
- **Enterprise-Ready** – Built for integration with Azure, .NET, and Java ecosystems

---

## Installation

=== "Python"
    ```bash
    pip install semantic-kernel
    ```

=== "C#"
    ```bash
    dotnet add package Microsoft.SemanticKernel
    ```

=== "Java"
    ```xml
    <dependency>
      <groupId>com.microsoft.semantic-kernel</groupId>
      <artifactId>semantickernel-api</artifactId>
      <version>1.0.0</version>
    </dependency>
    ```

---

## Quick Examples

### Basic Kernel Setup (Python)

```python
import asyncio
from semantic_kernel import Kernel
from semantic_kernel.connectors.ai.open_ai import OpenAIChatCompletion
from semantic_kernel.connectors.ai.open_ai import OpenAIChatPromptExecutionSettings
from semantic_kernel.contents.chat_history import ChatHistory

async def main():
    kernel = Kernel()

    # Add OpenAI chat service
    kernel.add_service(
        OpenAIChatCompletion(
            service_id="chat",
            ai_model_id="gpt-4o",
        )
    )

    chat_history = ChatHistory()
    chat_history.add_user_message("What are the key benefits of AI agents?")

    settings = OpenAIChatPromptExecutionSettings(max_tokens=500)
    chat_service = kernel.get_service("chat")
    response = await chat_service.get_chat_message_content(
        chat_history=chat_history,
        settings=settings,
    )
    print(response)

asyncio.run(main())
```

### Semantic Function (Prompt Template)

```python
import asyncio
from semantic_kernel import Kernel
from semantic_kernel.connectors.ai.open_ai import OpenAIChatCompletion
from semantic_kernel.prompt_template import PromptTemplateConfig

async def main():
    kernel = Kernel()
    kernel.add_service(OpenAIChatCompletion(ai_model_id="gpt-4o"))

    # Define a semantic function via a prompt template
    summarize_function = kernel.add_function(
        function_name="summarize",
        plugin_name="TextPlugin",
        prompt="Summarize the following text in 3 bullet points:\n\n{{$input}}",
        template_format="semantic-kernel",
    )

    result = await kernel.invoke(
        summarize_function,
        input=(
            "LangChain, LlamaIndex, AutoGen, CrewAI, and Semantic Kernel are all "
            "popular frameworks for building AI agents and LLM-powered applications. "
            "Each has unique strengths for different use cases."
        )
    )
    print(result)

asyncio.run(main())
```

### Plugin with Native Functions

```python
import asyncio
from semantic_kernel import Kernel
from semantic_kernel.connectors.ai.open_ai import OpenAIChatCompletion
from semantic_kernel.functions import kernel_function

class MathPlugin:
    @kernel_function(description="Add two numbers together")
    def add(self, a: float, b: float) -> float:
        return a + b

    @kernel_function(description="Multiply two numbers together")
    def multiply(self, a: float, b: float) -> float:
        return a * b

async def main():
    kernel = Kernel()
    kernel.add_service(OpenAIChatCompletion(ai_model_id="gpt-4o"))
    kernel.add_plugin(MathPlugin(), plugin_name="Math")

    result = await kernel.invoke(
        kernel.plugins["Math"]["multiply"],
        a=5.0, b=12.0
    )
    print(f"Result: {result}")  # Result: 60.0

asyncio.run(main())
```

---

## Core Concepts

### Architecture

```
┌───────────────────────────────────────┐
│              Semantic Kernel           │
│                                       │
│  ┌──────────┐  ┌──────────────────┐   │
│  │ Plugins  │  │   AI Services    │   │
│  │(Functions│  │(OpenAI, Azure,   │   │
│  │ & Skills)│  │  Hugging Face)   │   │
│  └──────────┘  └──────────────────┘   │
│                                       │
│  ┌──────────┐  ┌──────────────────┐   │
│  │  Memory  │  │    Planner       │   │
│  │(Semantic │  │(Auto-orchestrate │   │
│  │ Search)  │  │   functions)     │   │
│  └──────────┘  └──────────────────┘   │
└───────────────────────────────────────┘
```

### Key Components

| Component | Description |
|-----------|-------------|
| **Kernel** | Central orchestrator that manages services and plugins |
| **Plugins** | Collections of functions (semantic or native) |
| **Planner** | Automatically chains functions to fulfill goals |
| **Memory** | Stores and retrieves information via semantic search |
| **Connectors** | Integrations with LLM providers and data sources |

---

## When to Use Semantic Kernel

✅ Enterprise applications with existing .NET or Java codebases  
✅ Projects requiring Azure integration (Azure OpenAI, Azure Cognitive Search)  
✅ Plugin-based architectures where AI augments existing business logic  
✅ Organizations standardizing on Microsoft AI tooling  
✅ Long-running, stateful business process automation  

⚠️ For pure Python prototyping and a larger community ecosystem, consider [LangChain](langchain.md).

---

## Further Reading

- [Semantic Kernel Documentation](https://learn.microsoft.com/en-us/semantic-kernel/overview/)
- [Semantic Kernel GitHub](https://github.com/microsoft/semantic-kernel)
- [Semantic Kernel Samples](https://github.com/microsoft/semantic-kernel/tree/main/python/samples)
