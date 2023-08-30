# chatGPT

example from web dev simplified for terminal prompt

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

# Langchain

```
Answer the following questions as best you can. You have access to the following tools:

search: a search engine. useful for when you need to answer questions about current
        events. input should be a search query.
calculator: useful for getting the result of a math expression. The input to this
            tool should be a valid mathematical expression that could be executed
            by a simple calculator.

Use the following format:

Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [search, calculator]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question

Begin!

Question: ${question}
Thought:
```

```
Question: What was the high temperature in SF yesterday in Fahrenheit?
Thought: I can try searching the answer
Action: search
Action Input: "high temperature san francisco yesterday fahrenheit"
Observation: Found an article from the San Francisco Chronicle forecasting
             a high of 69 degrees
Thought: I can use this to determine the answer
Final Answer: The high temperature in SF yesterday was 69 degrees Fahrenheit.
```
