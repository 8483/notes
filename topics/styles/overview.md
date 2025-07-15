# Goal

Add all even numbers from an array.

Array:

```
[1, 2, 3, 4, 5, 6]
```

Result:

```
2 + 4 + 6 = 12
```

# Imperative

```js
let numbers = [1, 2, 3, 4, 5, 6];

let sum = 0;

for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] % 2 === 0) {
        sum += numbers[i];
    }
}

console.log(sum);
```

# Procedural

```js
function isEven(n) {
    return n % 2 === 0;
}

function addEvenNumbers(arr) {
    let total = 0;
    for (let i = 0; i < arr.length; i++) {
        if (isEven(arr[i])) {
            total += arr[i];
        }
    }
    return total;
}

let numbers = [1, 2, 3, 4, 5, 6];

console.log(addEvenNumbers(numbers));
```

# Functional

```js
let numbers = [1, 2, 3, 4, 5, 6];

let sum = numbers.filter((n) => n % 2 === 0).reduce((a, b) => a + b, 0);

console.log(sum);
```

# Object Oriented

```js
class NumberArray {
    constructor(numbers) {
        this.numbers = numbers;
    }

    isEven(n) {
        return n % 2 === 0;
    }

    sumEvens() {
        let total = 0;
        for (let n of this.numbers) {
            if (this.isEven(n)) {
                total += n;
            }
        }
        return total;
    }
}

let nums = new NumberArray([1, 2, 3, 4, 5, 6]);

console.log(nums.sumEvens());
```
