# Apps

jupyter - Online IDE  
kaggle - datasets
streamlit - UI prototyping

# AI Frameworks

Wrappers to simplify using AI models to do tasks. Basically write and orchestrate prompts indirectly.

OpenAI API is quite easy to use directly and gives you the most control, I don’t see much benefit of a wrapper langChain here tbh.

Example: Langchain, Haystack, Microsoft Guidance

# Chains

What kind of actions do you want to trigger from the imported data?

# llamaindex (formerly GPT-Index)

LLama-index is a vector database, langchains is a framework that can connect multiple LLMs, databases (normal and vector) and other related software like search plugins and it also assists with pre- and postprocessing of input and generated text.

llamandex is a simple, flexible data framework for connecting
custom data sources to large language models.

The llamaindex structure involves creating an overview of data through indexing documents. The indexing process involves chunking the text document into different nodes, each with an embedding. A retriever helps retrieve documents for a given query, and a query engine manages retrieval and census.

# llamaindex vs langchain

Llama index is focused on loading documents/texts and querying them. It has a lot of great tools for extracting info from large documents to insert alongside the query to the LLM.

Langchain is more broad.

They overlap a lot - llama index is strongest for vector embed / retrieval etc. While langchain is more mature when it comes too agents / multi step chains.

Depends on your Final Goal, if its mainly an intelligent search tool llamaindex is great, if you want to build a chatgpt clone capable of creating plugins that is a whole different thing. Langchain allows you to leverage multiple instance of ChatGPT, provide them with memory, even multiple instance of llamaindex. Things you can do with langchain is build agents, that can do more than one things, one example is execute python code, while also searching google. Basically llmaindex is a smart storage mechanism, while Langchain is a tool to bring multiple tools together.

Lamaindex started life as gptindex. An academic person was its creator. The rather narrower scope of llamaindex is suggested by its name, llama is its llm, and a vector db is its other partner. The design intent of langchain, tho, is more broad, and therefore need not include llama as the llm and need not include a vectordb in the solution. Of course llamaindex may well grow beyond its original design intent, but from the very beginning you can tell its creator wasn't thinking as generally as langchains, and the associated software may reflect that lack of generality.
