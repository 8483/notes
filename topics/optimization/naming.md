# 1. Don't abbreviate names

```js
function relScore(m1, m2) {}

function movieRelationScore(movie1, movie2) {}
```

# 2. Use Endiannes / Smurfing / Molds

```bash
# big-endian = Most significant word FIRST
profitMonthlyMax

# little-endian = Most significant word LAST
maxMonthlyProfit
```

# 3. Don't use single letters

```js
let x = 1;

let someValue = 1;
```

# 4. Don't put types in the name (Hungarian notation)

```c
bool bIsValid;
int32_t iSpeed;
uint32_t uNumUsers;
char szUserName;
```

# 5. Add units to variables.

```js
function execute(delay) {}

function execute(delaySeconds) {}
```

# 6. Refactor if you find yourself using `utils`.

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
