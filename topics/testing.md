# Overview

The idea is to automate the manual testing of code, with code.

This requires an initial time investment, which is quickly paid back in the time saved by making sure the application is stable.

# Test types

-   **Unit tests**  
    Testing single functions with no dependencies (thousands).

-   **Integration test**  
    Testing single functions with dependencies i.e. functions that call other functions (couple). Does the combination of single functions work?

-   **End-to-end (E2E) / UI test**  
    Testing by simulating a literal manual UI interaction (few).

# Setup

The following is needed to test code.

1. **Test runner**  
   Executes tests and summarizes results, with `Mocha`.

2. **Assertion library**  
   Define testing logic and conditions, with `Chai`.

3. **Headless browser** - E2E testing  
   Simulate browser interactions, with `Puppeteer`.

-   `Jest` is both a test runner and an assertion library.

# Jest

```bash
npm install --save-dev jest
```

## Unit test

**scripts.js**

```javascript
exports.addNumbers = function(a, b) {
    return `The result is ${a + b}`;
};
```

**scripts.test.js** / **scripts.spec.js**

We must name the test files like this, in order to be found.

```javascript
const { addNumbers } = require("./scripts");

// Task runner
test("should output a sum", () => {
    const sum = addNumbers(2, 3);
    // Assertion library
    expect(sum).toBe("The result is 5");
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
  ✓ should output a sum (3ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.605s
Ran all test suites.
```

## E2E UI test

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
        args: ["--window-size=1920,1080"]
    });
    const page = await browser.newPage();

    await page.goto("http://domain.com/todo");
    await page.type("input#todo", "Some arbitrary task");
    await page.click("#btn-add-user");

    const todoItem = await page.$eval(".todo-item", el => el.textContent);
    expect(todoItem).toBe("Some arbitrary task");
});
```

The test will run much faster without rendering the browser.

```javascript
const browser = await puppeteer.launch({
    headless: true
});
```
