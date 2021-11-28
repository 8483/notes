# Install

```bash
npm install --save-dev jest
```

# Unit test

**scripts.js**

```javascript
exports.addNumbers = function (a, b) {
    return `The result is ${a + b}`;
};
```

**scripts.test.js** / **scripts.spec.js**

We must name the test files like this, in order to be found.

```javascript
const { addNumbers } = require("./scripts");

// Task runner
test("2 + 3 = 5, output the sum in a string format", () => {
    // Assertion library
    expect(addNumbers(2, 3)).toBe("The result is 5");
});
```

**package.json**

```json
"scripts": {
    "test": "jest"
}
```

We can also run the tests automatically with every change we make.

```json
"scripts": {
    "test": "jest --watch"
}
```

We finally run the test...

```bash
npm test

--
 PASS  ./scripts.test.js
  ✓ 2 + 3 = 5, output the sum in a string format (3ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.605s
Ran all test suites.
```

# E2E UI test

Puppeteer works with Jest.

```bash
npm install --save-dev puppeteer
```

This literally creates a browser window in which the specified actions are simulated.

**scripts.test.js**

```javascript
test("Should insert a todo item", async () => {
    const browser = await puppeteer.launch({
        headless: false, // Display the browser
        slowMo: 80, // slow down the actions in the browser
        args: ["--window-size=1920,1080"],
    });
    const page = await browser.newPage();

    await page.goto("http://domain.com/todo");
    await page.type("input#todo", "Some arbitrary task");
    await page.click("#btn-add-user");

    const todoItem = await page.$eval(".todo-item", (el) => el.textContent);
    expect(todoItem).toBe("Some arbitrary task");
});
```

The test will run much faster without rendering the browser.

```javascript
const browser = await puppeteer.launch({
    headless: true,
});
```

# Example

**cashier.js**

```js
/**
 * creates a new cashier
 * takes in a currency, e.g. $
 */
export function newCashier(currency) {
    return null;
}

/**
 * Receives the cashier and returns a list with all the bills and coins currently in the cashier, in descending order.
 *    Output example: [100, 50, 50, 5, 2, 1, 1, 0.05, 0.01, 0.01]
 */
export function getCurrentCash(cashier) {
    return null;
}

/**
 * Receives the cashier, the purchase price and the bills and coins received as payment.
 *
 * For example, to pay $12 with a $10 bill and a $5 bill you would call:
 *   pay(cashier, 12, [10, 5])
 * Change should be given back to the customer in descending order, biggest bills and coins first.
 * The result can be:
 * - {success: true, cashier, change: [10, 5, 2]}, if we can give change back to the customer. The result contains:
 *    - cashier: the updated cashier.
 *    - change: the list of the bills and coins we gave back as change, in descending order.
 * - {success: false, cashier, error: "NOT ENOUGH CHANGE"}, if we can't give change back.
 * - {success: false, cashier, error: "PAID LESS THAN PRICE"}, if the customer paid too little.
 * - {success: false, cashier, error: "INVALID DENOMINATION"}, if the customer paid with an invalid bill or coin.
 */

export function pay(cashier, purchasePrice, purchaseBills) {
    return null;
}
```

**cashier.spec.js**

```js
import { newCashier, getCurrentCash, pay } from "../cashier.js";

describe("cashier", () => {
    it("contains the correct cash after an exact payment", () => {
        let cashier = newCashier("$");
        let success, change;

        ({ success, change, cashier } = pay(cashier, 25, [10, 10, 5]));
        expect(change).toEqual([]);
        expect(success).toBeTruthy();
        expect(getCurrentCash(cashier)).toEqual([10, 10, 5]);
    });

    it("returns an error when there it isn't possible to give change back", () => {
        const cashier = newCashier("$");

        const { error, success } = pay(cashier, 10, [5, 2, 2, 2]);
        expect(success).toBeFalsy();
        expect(error).toBe("NOT ENOUGH CHANGE");
    });

    it("returns an error when paid amount is lower than purchase price", () => {
        const cashier = newCashier("$");

        const { error, success } = pay(cashier, 10, [5, 2, 2]);
        expect(success).toBeFalsy();
        expect(error).toBe("PAID LESS THAN PRICE");
    });

    it("returns an error when invalid bills or coins are paid", () => {
        const cashier = newCashier("$");
        let error, success;

        ({ error, success } = pay(cashier, 11, [11]));
        expect(success).toBeFalsy();
        expect(error).toBe("INVALID DENOMINATION");

        ({ error, success } = pay(cashier, 1, [0.5, 0.3, 0.2]));
        expect(success).toBeFalsy();
        expect(error).toBe("INVALID DENOMINATION");
    });

    it("gives change as long as it has the necessary coins and bills", () => {
        let cashier = newCashier("$");
        let success, change, error;

        ({ cashier, success, change } = pay(cashier, 11, [5, 2, 2, 1, 1]));
        expect(success).toBeTruthy();
        expect(change).toHaveLength(0);

        ({ cashier, success, change } = pay(cashier, 4, [5]));
        expect(success).toBeTruthy();
        expect(change).toEqual([1]);

        ({ cashier, success, change } = pay(cashier, 4, [5]));
        expect(success).toBeTruthy();
        expect(change).toEqual([1]);

        ({ cashier, success, error } = pay(cashier, 4, [5]));
        expect(success).toBeFalsy();
        expect(error).toBe("NOT ENOUGH CHANGE");

        expect(getCurrentCash(cashier)).toEqual([5, 5, 5, 2, 2]);
    });

    it("can trade up bills within multiple transactions", () => {
        let cashier = newCashier("$");
        let success, change, error;

        ({ cashier, success, change } = pay(cashier, 11, [5, 2, 2, 1, 1]));
        expect(success).toBeTruthy();
        expect(change).toHaveLength(0);

        ({ cashier, success, change } = pay(cashier, 6, [10]));
        expect(success).toBeTruthy();
        expect(change).toEqual([2, 2]);

        ({ cashier, success, change } = pay(cashier, 8, [20]));
        expect(success).toBeTruthy();
        expect(change).toEqual([10, 1, 1]);

        ({ cashier, success, error } = pay(cashier, 1, [2]));
        expect(success).toBeFalsy();
        expect(error).toBe("NOT ENOUGH CHANGE");

        ({ cashier, success, error } = pay(cashier, 3, [5]));
        expect(success).toBeFalsy();
        expect(error).toBe("NOT ENOUGH CHANGE");

        ({ cashier, success, change } = pay(cashier, 25, [50]));
        expect(success).toBeTruthy();
        expect(change).toEqual([20, 5]);

        expect(getCurrentCash(cashier)).toEqual([50]);
    });

    it("gives superfluous coins/bills back, even when the cashier is empty", () => {
        let cashier = newCashier("$");
        let success, change;

        ({ cashier, success, change } = pay(cashier, 6, [5, 1, 1]));
        expect(success).toBeTruthy();
        expect(change).toEqual([1]);

        expect(getCurrentCash(cashier)).toEqual([5, 1]);
    });

    it("can deal with small coins", () => {
        let cashier = newCashier("$");
        let success, change;

        ({ cashier, success, change } = pay(cashier, 0.55, [0.25, 0.1, 0.1, 0.1]));
        expect(success).toBeTruthy();
        expect(change).toHaveLength(0);

        ({ cashier, success, change } = pay(cashier, 0.7, [1]));
        expect(success).toBeTruthy();
        expect(change).toEqual([0.1, 0.1, 0.1]);

        expect(getCurrentCash(cashier)).toEqual([1, 0.25]);
    });

    it("handles scenario where some of the largest bills/coins can't be used to provide change", () => {
        let cashier = newCashier("$");
        let success, change, error;

        ({ cashier, success, change } = pay(cashier, 11.25, [5, 2, 2, 1, 1, 0.25]));
        expect(success).toBeTruthy();
        expect(change).toHaveLength(0);

        ({ cashier, success, change } = pay(cashier, 3.75, [10]));
        expect(success).toBeTruthy();
        expect(change).toEqual([5, 1, 0.25]);
    });
});
```
