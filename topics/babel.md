Babel is a transpiler (translator + compiler) and it converts modern code into old code, able to run in older browsers.

```bash
npm install babel-cli babel-core babel-preset-env
```

`babel-cli` - Command line interface. Takes a js file and returns a transpiled one.
`babel-core` - The transpiling logic.  
`babel-preset-env` - Each new javascript feature has a **separate** plugin. Intead of installing them one by one, we can install a preset containing all of them.

To run the transpiler we can use:
```bash
# Use the env preset to transpile index.js and put the output in the build folder in index.js.
babel --presets env index.js -o build/index.js
```
This command is for one file. In order to transpile many files, a bundler like Webpack is needed. Here, every file is transpiler first, before being added to the bundle.

We can add this in `package.json` for convenience:
```json
{
    "scripts": {
        "babel": "babel --presets env index.js -o build/index.js"
    }
}
```