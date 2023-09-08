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
