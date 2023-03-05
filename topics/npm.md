# NPM - Package manager

Installs `node` packages.

`NPM` by itself does not simply run any package. It doesn't run any package in a matter of fact. If you want to run a package using NPM, you must specify that package in your `package.json` file.

# Init

The `npm init` command is used to create a new Node.js project i.e. a convenient way of scaffolding a `package.json`;

```
npm init = npm create
```

Also, you can use `npm init <configurator>` to use a perconfigured `package.json`.

```
> npm init vite@latest
Project name: … vite-project
Select a framework: › - Use arrow-keys. Return to submit.
    vanilla
    vue
    react
    preact
    lit
  ❯ svelte
```

```
npm create svelte@latest myapp
```

# Install

When executables are installed via NPM packages, NPM links to them:

1. _local_ installs have "links" created at `./node_modules/.bin/` directory.
2. _global_ installs have "links" created from the global `bin/` directory (e.g. `/usr/local/bin`) on Linux or at `%AppData%/npm` on Windows.

One might install a package locally on a certain project:

    npm install some-package

Now let's say you want NodeJS to execute that package from the command line:

    $ some-package

The above will **fail**. Only **globally installed** packages can be executed by typing their name _only_.

To fix this, and have it run, you must type the local path:

    $ ./node_modules/.bin/some-package

You can technically run a locally installed package by editing your `packages.json` file and adding that package in the `scripts` section:

    {
      "name": "whatever",
      "version": "1.0.0",
      "scripts": {
        "some-package": "some-package"
      }
    }

Then run the script using [`npm run-script`][3] (or `npm run`):

    npm run some-package

# NPX - Package runner

Run `node` packages without installing them. Comes bundled with `npm`.

Calling npx `command` when `command` isn’t already in your `$PATH` will automatically install a package with that name from the npm registry for you, and invoke it. When it’s done, the installed package won’t be anywhere in your globals, so you won’t have to worry about pollution in the long-term.

This feature is ideal for things like generators, too. Tools like yeoman or create-react-app only ever get called once in a blue moon. By the time you run them again, they’ll already be far out of date, so you end up having to run an install every time you want to use them anyway.

`npx` will check whether `<command>` exists in `$PATH`, or in the local project binaries, and execute it. So, for the above example, if you wish to execute the locally-installed package `some-package` all you need to do is type:

    npx some-package

Another **major** advantage of `npx` is the ability to execute a package which wasn't previously installed:

    $ npx create-react-app my-app

The above example will generate a `react` app boilerplate _within_ the path the command had run in, and ensures that you always use the latest version of a generator or build tool without having to upgrade each time you’re about to use it.

---

### Use-Case Example:

`npx` command may be helpful in the `script` section of a `package.json` file,
when it is unwanted to define a dependency which might not be commonly used or any other reason:

```js
"scripts": {
    "start": "npx gulp@3.9.1",
    "serve": "npx http-server"
}
```

Call with: `npm run serve`
