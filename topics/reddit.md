# 1. Create app

https://old.reddit.com/prefs/apps

# 2. Install dependencies

```
npm i snoowrap express --save
```

# 3. Scraper

```js
var snoowrap = require("snoowrap");
const express = require("express");
const app = express();
const port = 6000;

// (async () => {
app.get("/", async (req, res) => {
    // https://old.reddit.com/prefs/apps/
    const r = new snoowrap({
        userAgent: "A random string.",
        clientId: "APP_ID",
        clientSecret: "APP_SECRET",
        username: "REDDIT_USER",
        password: "REDDIT_PASSWORD",
    });

    const subreddit = await r.getSubreddit("cscareerquestions");
    const topPosts = await subreddit.search({ query: "Salary Sharing thread for EXPERIENCED DEVS", time: "all", sort: "new" });

    console.log("topPosts");

    let posts = topPosts.filter((post) => post.title.includes("EXPERIENCED DEVS"));

    console.log("posts");

    let data = [];

    /*
    submisson
        comments
            comment
                replies
                    replies
            comment
                replies
                    replies
    */

    for (let i = 0; i < posts.length; i++) {
        let post = posts[i];
        let id = post.id;

        let submission = await r.getSubmission(id).expandReplies({ limit: 1, depth: 1 });

        console.log("submission");

        let comments = submission.comments;

        comments.forEach((comment) => {
            let region = comment.body.split("**")[1];

            if (!region) return;

            comment.replies.forEach((reply) => {
                let foo = { region, permalink: `https://old.reddit.com/${reply.permalink}` };

                let lines = reply.body.split("\n");

                lines.forEach((line) => {
                    let parts = line.split(": ");
                    let key = "";
                    let value = parts[1];

                    if (parts[0].includes("Experience")) key = "experience";
                    if (parts[0].includes("Tenure")) key = "tenure";
                    if (parts[0].includes("Industry")) key = "industry";
                    if (parts[0].includes("Title")) key = "title";
                    if (parts[0].includes("Location")) key = "location";
                    if (parts[0].includes("Salary")) key = "salary";
                    if (parts[0].includes("Total")) key = "total";

                    if (key && value) foo[key] = value;
                });

                data.push(foo);
            });
        });
    }

    console.log(data);

    console.log("done");

    res.send(data);
});

app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`);
});
// })();
```
