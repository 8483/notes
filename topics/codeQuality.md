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

### 1. Don't use single letters

```js
let x = 1;

let someValue = 1;
```

### 2. Don't abbreviate names

```js
function relScore(m1, m2) {}

function movieRelationScore(movie1, movie2) {}
```

### 3. Don't put types in the name (Hungarian notation)

```c
bool bIsValid;
int32_t iSpeed;
uint32_t uNumUsers;
char szUserName;
```

### 4. Add units to variables.

```js
function execute(delay) {}

function execute(delaySeconds) {}
```

### 5. Refactor if you find yourself using `utils`.

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
