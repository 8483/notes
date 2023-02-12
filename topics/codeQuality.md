# Optimization

> _"Premature optimization is the root of all evil"_ - Donald Knuth

You're writing code to solve a real world problem. Getting to a solution faster is more important than a fast solution.

-   Macro performance (Design level) - system wide decisions ex. sql vs nosql
-   Micro performance (Fine tuning) - modules, functions...

1. Have a **real** performance problem.
2. Make 80% moves (what change leads to an 80% reduction? usually data structures).
3. Use a profiler to fix hot spots.
4. Get under the hood (meomry).

# Never nest beyond 3 levels

```js
function calculate(bottom, top) {
    //  One
    if (top > bottom) {
        //  Two
        let sum = 0;

        for (let number = bottom; number <= top; number++) {
            //  Three
            if (number % 2 == 0) {
                //  Four
                sum += number;
            }
        }

        return sum;
    } else {
        return 0;
    }
}
```

### **Methods of denesting**

-   **Extraction** - Move code into a separate function
-   **Inversion** - Early termination of conditions

```js
// Extraction i.e. Separate function
function filterNumber(number) {
    // One
    if (number % 2 == 0) {
        // Two
        return number;
    }
    return 0;
}

function calculate(bottom, top) {
    // One

    // Inversion i.e. Early termination
    if (top < bottom) return 0;

    let sum = 0;

    for (let number = bottom; number <= top; number++) {
        // Two
        sum += filterNumber(number);
    }

    return sum;
}
```

# Naming variables

### 1. Don't abbreviate names

```js
function relScore(m1, m2) {}

function movieRelationScore(movie1, movie2) {}
```

### 2. Use Endiannes / Smurfing / Molds

```bash
# big-endian = Most significant word FIRST
profitMonthlyMax

# little-endian = Most significant word LAST
maxMonthlyProfit
```

### 3. Don't use single letters

```js
let x = 1;

let someValue = 1;
```

### 4. Don't put types in the name (Hungarian notation)

```c
bool bIsValid;
int32_t iSpeed;
uint32_t uNumUsers;
char szUserName;
```

### 5. Add units to variables.

```js
function execute(delay) {}

function execute(delaySeconds) {}
```

### 6. Refactor if you find yourself using `utils`.

```js
// utils.js
function assignDefaults() {}
function relationScore() {}
function moviesOnPage() {}
function topMoviesByRating() {}
function moviesByDirector() {}
function parseCookie() {}
function assignCookie() {}
```

```js
// movies.js
function assignDefaults() {}
function relationScore() {}

// pager.js
function moviesOnPage() {}

// movieCollection.js
function topMoviesByRating() {}
function moviesByDirector() {}

// cookie.js
function parseCookie() {}
function assignCookie() {}
```

# Don't write comments

```
code > comments
```

Comments get bugs like code. When code is updated, usually the comment is not.

Use comments only when there is some non-obvious code optimization, explaining why the code is weird, or if it references some specific math or algorithm.

### Write documentation

Comments = How code works  
Documentation = How code is used

### Use constants

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

### Refactor with constants

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
