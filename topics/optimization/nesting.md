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

# **Methods of denesting**

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
