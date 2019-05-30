# Hello World

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<div id="app">
    <p>{{ title }}</p>
</div>
```
```javascript
new Vue({
    el: "#app",
    data: {
        title: "Hello World!"
    }
});
```
`Vue` instances are at the core of every Vue app, as they control their own HTML template rendered to the screen.

# Component Anatomy

All the code is encapsulated in the component.

**Output - HTML**
```html
<template>
    <div class="user">
        <h1>{{ user.name }}</h1>
    </div>
</template>
```

**Functionality - Javascript**

A default object is exported i.e. component with a name of "User". The `data()` method returns the state of the component.
```html
<script>
    export default {
        name: "User",
        data() {
            return {
                user: { name: "Foo" }
            }
        }
    }
</script>
```

**Style - CSS**
```html
<style scoped>
    h1 {
        font-size: 2 rem;
    }
</style>
```

The main page served is `index.html` located in the `public` folder.  

The entry point for a vue app is `main.js`, located in `src`. There we import the `Vue` app object (where we mount the instance to the `index.html`), and the `App` component.

# Install

```bash
sudo npm i -g vue

vue --version
```

The framework can also be referenced with a CDN.

# Vue-CLI

Qucik SPA scaffolding. It provides build setups for a modern frontend workflow, with hot-reload, lint-on-save, and production-ready builds.

```bash
vue create app-name

cd app-name
nom run serve
```

Chrome extension for debugging: Vue.js Devtools  
VS Code Syntax highlighting: Vetur

# vue-router

**Installing it overwrites the `App.vue` file**s

# vuex

State management library similar to Redux.