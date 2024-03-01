# GPT 4

```js
const { Configuration, OpenAIApi } = require("openai");

const { apiKey } = require("./config");

const configuration = new Configuration({
    apiKey: apiKey,
});

const openai = new OpenAIApi(configuration);

async function run() {
    const response = await openai.createChatCompletion({
        model: "gpt-4",
        messages: [
            { role: "system", content: "You are a helpful assistant." },
            { role: "user", content: "Who won the world series in 2020?" },
            { role: "assistant", content: "The Los Angeles Dodgers." },
        ],
        max_tokens: 2000,
        n: 1,
        stop: null,
        temperature: 0,
    });

    console.log(response.data.choices[0].message.content);
}

run();
```

# GPT 3

```js
const { Configuration, OpenAIApi } = require("openai");

const { organization, apiKey } = require("./config");

const configuration = new Configuration({
    organization: organization,
    apiKey: apiKey,
});

const openai = new OpenAIApi(configuration);

async function run() {
    const completion = await openai.createCompletion({
        model: "text-davinci-003",
        prompt: "Who won the world series in 2020?",
        max_tokens: 3000,
        temperature: 0,
    });

    console.log(completion.data.choices[0].text);
}

run();
```

# Notes

Remember that the model predicts which text is most likely to follow the text preceding it.

As a rough rule of thumb, 1 token is approximately 4 characters or 0.75 words for English text.

temperature = randomness. Smaller = less random. Lowering temperature means it will take fewer risks, and completions will be more accurate and deterministic. Increasing temperature will result in more diverse completions.

In the context of OpenAI's Chat API, the role and content parameters work together to define the conversation history.

role: This parameter is used to specify the role of a message sender in the conversation.
It can take one of three values: 'system', 'user', or 'assistant'.

'system': This role is typically used for instructions that set the behavior of the assistant. It often begins the conversation.
'user': This role is typically for the end user or human in the conversation.
'assistant': This role is for the AI model or assistant's responses.

content: This parameter holds the actual text of the message from the role.
For example, for the 'user' role, the content would be what the user said or asked.
For the 'assistant', it would be the assistant's response.

In the array of messages you provided:

```
messages: [
{ role: "system", content: "You are a helpful assistant." },
{ role: "user", content: "Who won the world series in 2020?" },
{ role: "assistant", content: "The Los Angeles Dodgers." },
{ role: "user", content: "Where was it played?" },
]
```

The 'system' role is telling the assistant that its role is to be helpful. Then the 'user' asks a question about who won the World Series in 2020, the 'assistant' provides the answer, and then the 'user' asks another question. These messages together represent the conversation history. When a new message is added to the conversation history and a chat completion is created, the AI model will generate a response in the context of this history.
