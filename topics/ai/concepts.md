Forward propagation = Prediction  
Back propagation = Measuring i.e. learning

# IDE

jupyter - Online IDE

# Datasets

kaggle - datasets

# Frameworks

Wrappers to simplify using AI models to do tasks. Basically write and orchestrate prompts indirectly.

OpenAI API is quite easy to use directly and gives you the most control, I don’t see much benefit of a wrapper langChain here tbh.

Example: Langchain, Haystack, Microsoft Guidance

# autoGPT

An agent talking to itself. A recursive LLM.

It’s a thinking AI basically, Chat GPT is just LLM. A LLM is like a baby without understanding, but using tools like Langchain and Pinecone you can teach the AI task. Auto GPT on has a few task, it’s really like a template to build your own thinking AI.

Research Langchain and Pinecone to get a better understanding of the potential.

# Agent

An LLM with a predefined behavior, a personality.

The word agent is being thrown around a lot to refer to an application that can execute multiple tasks according to a given control flow (see Control flows section). A task can leverage one or more tools. In the example above, SQL executor is an example of a tool.

In broad terms, with the Agent model, the LLM becomes an orchestrator, taking a question, decomposing it into chunks, then using appropriate tools to pull together an answer.

# Tools / Plugins

Ways in which the agent can choose to do things ex. search the web, query a database.

Tools and plugins are basically the same things.

Ex.

-   search (e.g. by using Google Search API or Bing API)
-   web browser (e.g. given a URL, fetch its content)
-   bash executor
-   calculator

# Transformer

A transformer is a type of neural network, which have numbers as inputs. Both the input and output words need to be turned into numbers.

There are many ways to do the conversion, with the most common being word embedding.

# LLM

LLMs (Large language models) are text completion engines

Examples: gpt-3.5, gpt-4, hugginface, llama

# Custom AI

2 ways to do this:

1. Finetune LLM (Behave a certain way ex. talk like Trump) - Re-train the model. More complex, saves cost.
2. Knowledge base. (Gain domain knowledge) - Create embeddings and store them in a vector database, which is the searched for and fed into an LLM prompt.
