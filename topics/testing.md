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
   Executes tests and summarizes results. Ex. `Jest`, `Mocha`.

2. **Assertion library**  
   Define testing logic and conditions. Ex. `Jest`, `Chai`.

3. **Headless browser** - E2E testing  
   Simulate browser interactions. Ex. `Puppeteer`, `Cypress`.

`Jest` is both a test runner and an assertion library.
