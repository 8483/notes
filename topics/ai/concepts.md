Forward propagation = Prediction  
Back propagation = Measuring i.e. learning

# Transformer

A transformer is a type of neural network, which have numbers as inputs. Both the input and output words need to be turned into numbers.

There are many ways to do the conversion, with the most common being word embedding.

# LLM

LLMs (Large language models) are text completion engines i.e. fancy auto-completes. They have ZERO intelligence.

Examples: chat gpt, claude code, gemini, hugginface, llama

These can be "customized" like this:

- Finetuning (Behave a certain way ex. talk like Trump) - Re-train the model. More complex, saves cost.
- Knowledge base. (Gain domain knowledge) - Create embeddings and store them in a vector database, which is the searched for and fed into an LLM prompt.

# Frameworks

Wrappers to simplify using AI models to do tasks. Basically write and orchestrate prompts indirectly.

OpenAI API is quite easy to use directly and gives you the most control, I don’t see much benefit of a wrapper langChain here tbh.

Example: Langchain, Haystack, Microsoft Guidance

# Agent

An LLM with a predefined behavior, a personality.

The word agent is being thrown around a lot to refer to an application that can execute multiple tasks according to a given control flow (see Control flows section). A task can leverage one or more tools. In the example above, SQL executor is an example of a tool.

In broad terms, with the Agent model, the LLM becomes an orchestrator, taking a question, decomposing it into chunks, then using appropriate tools to pull together an answer.

An agent is just a loop talking to an LLM, i.e. iterating over a task until stopped or completed.

OpenCode is a popular AI coding agent that runs in the terminal, similar to Claude Code, just open source and can use all the LLMs.

An agent talking to itself. A recursive LLM.

It’s a thinking AI basically, Chat GPT is just LLM. A LLM is like a baby without understanding, but using tools like Langchain and Pinecone you can teach the AI task. Auto GPT on has a few task, it’s really like a template to build your own thinking AI.

# Sub-agent

Sub-agents are specialized personas that you may want to offload specific types work to fresh contexts backed by a full agent instance.

So use subagents to keep your context clean when you want an actual full agent instance to perform something without poisoning your main context (code reviewer, skeptic, testing specialist, language/framework specialist, architect, etc).

# Skill

Skills is basically just breaking a claude.md file into usable chunks so it can access them when needed instead of having to review an entire huge document every time. Makes things run smoother with less ADHD.

Skills are specialized instructions you may want to provide to Claude in certain circumstances, but may not always be relevant.

Use skills when you want to sometimes provide instructions to your primary agent with a relevant skillset.

They are “packages” of prompts and tools.

They are more a lightweight form of MCP as they are easier to put together and share. Also, they run in context, whereas subagents create their own context.

# Model Context Protocol (MCP)

> Standard for AI/LLMs to communicate with APIs.

> MCP makes it easier to expose new tools to LLMs.

An open-source standard for building APIs in a way that makes it easier for LLMs to connect to them.

Other standards include REST, RPC, SOAP and GraphQL.

MCP is an API that calls other APIs.

The clients sends a prompt, and MPC figures out which API to call next.

To actually use MCP, you need a client that supports the MCP protocol, like Claude Desktop.

# Slash commands

Slash commands are specialized commands you may want to invoke manually in certain circumstances.

Use slash commands when there are things you specifically know you’ll want to invoke at certain points.

# Tools / Plugins

Ways in which the agent can choose to do things ex. search the web, query a database.

Tools and plugins are basically the same things.

Ex.

- search (e.g. by using Google Search API or Bing API)
- web browser (e.g. given a URL, fetch its content)
- bash executor
- calculator

# Embeddings

Embeddings are a numerical representation of text that can be used to measure the relatedness between two pieces of text.

Turn data into a vector with hundreds of dimensions.

Data, like words, converted into an array of numbers, known as a vector, which contains pattern of relationship between the data.

Open AI ada = 8,191 tokens i.e. 32,764 characters ~ 10 pages of text.

One direction that I find very promising is to use LLMs to generate embeddings and then build your ML applications on top of these embeddings, e.g. for search and recsys. As of April 2023, the cost for embeddings using the smaller model text-embedding-ada-002 is $0.0004/1k tokens. If each item averages 250 tokens (187 words), this pricing means $1 for every 10k items or $100 for 1 million items.

# Vector databases

> What pieces of text in the database have similar vectors to the prompt.

A vector database indexes and stores vector embeddings for fast retrieval and similarity search. It compares how close embeddings are.

It turns the user's question into a vector, which is compared to the vectors in the database.

Ex. Pinecone, Qdrant, Weaviate, Chroma, Faiss, Redis, Milvus, ScaNN.
