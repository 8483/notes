# Don't write comments

```
code > comments
```

Comments get bugs like code. When code is updated, usually the comment is not.

Use comments only when there is some non-obvious code optimization, explaining why the code is weird, or if it references some specific math or algorithm.

# Write documentation

Comments = How code works  
Documentation = How code is used

# Use constants

```js
// A status of 5 signals message sent
if (status == 5) {
    message.markSent();
}
```

```js
CONST MESSAGE_SENT = 5
if (status == MESSAGE_SENT) {
    message.markSent();
}
```

# Refactor with constants

```js
/*
You can update a message IF the current user is the author of the message and the message was delivered less 5 minutes ago OR if the current user is an administrator. You can also edit the message if the message wasn't delivered yer.
*/

if ((message.user.id == currentUser.id && (!message.deliveredTime() || datetime.now() - message.deliveredTime() < 300)) || currentUser.type == "administrator") {
    message.updateText(text);
}
```

```js
const FIVE_MINUTES = 5 * 60;

let userIsAuthor = message.user.id == currentUser.id;
let isRecent = !message.deliveredTime() || datetime.now() - message.deliveredTime() < FIVE_MINUTES;
let userIsAdmin = currentUser.type == "administrator";

if ((userIsAuthor && isRecent) || userIsAdmin) {
    message.updateText(text);
}
```

```js
function canEditMessage(currentUser, message) {
    const FIVE_MINUTES = 5 * 60;

    let userIsAuthor = message.user.id == currentUser.id;
    let isRecent = !message.deliveredTime() || datetime.now() - message.deliveredTime() < FIVE_MINUTES;
    let userIsAdmin = currentUser.type == "administrator";

    return (userIsAuthor && isRecent) || userIsAdmin;
}

if (canEditMessage(currentUser, message)) {
    message.updateText(text);
}
```
