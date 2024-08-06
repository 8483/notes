# Basic

Custom components **MUST** include a hyphen in the name. Since there are no html elements with hyphens, this is how custom ones are identified.

The shadow DOM is used to achieve encapsulation i.e. everything is scoped to the web component only.

```html
<!DOCTYPE html>
<html>
    <head>
        <script src="customComponent.js"></script>
    </head>

    <body>
        <todo-item>
            Todo 1
            <span slot="description">Some text</span>
        </todo-item>

        <todo-item>
            Todo 3
            <span slot="description">Some text 2</span>
        </todo-item>
    </body>
</html>
```

# Approach #1

```js
// customComponent.js

const template = document.createElement("template");

template.innerHTML = `
    <style>
        label {
            color: red;
            display: block;
        }

        .description {
            font-size: .65rem;
            font-weight: lighter;
            color: #777;
        }
    </style>

    <label>
        <input type="checkbox" />
        <slot></slot>
        <span class="description">
            <slot name="description"></slot>
        </span>
    </label>
`;

class TodoItem extends HTMLElement {
    constructor() {
        super();

        // open allows the usage of this.shadowRoot
        const shadow = this.attachShadow({ mode: "open" });
        const clone = template.content.cloneNode(true);
        shadow.append(clone);

        this.checkbox = shadow.querySelector("input");
    }

    connectedCallback() {
        console.log("connected");
    }

    static get observedAttributes() {
        return ["checked"];
    }

    attributeChangedCallback(name, oldValue, newValue) {
        if (name === "checked") this.updateChecked(newValue);
    }

    updateChecked(value) {
        this.checkbox.checked = value != null && value !== "false";
    }

    disconnectedCallback() {
        console.log("disconnected");
    }
}

customElements.define("todo-item", TodoItem);

const item = document.querySelector("todo-item");
```

# Approach #2

```js
class TodoItem extends HTMLElement {
    constructor() {
        super();

        // open allows the usage of this.shadowRoot
        this.shadow = this.attachShadow({ mode: "closed" });

        this.render();

        this.checkbox = this.shadow.querySelector("input");
    }

    connectedCallback() {
        console.log("connected");
    }

    static get observedAttributes() {
        return ["checked"];
    }

    attributeChangedCallback(name, oldValue, newValue) {
        if (name === "checked") this.updateChecked(newValue);
    }

    updateChecked(value) {
        this.checkbox.checked = value != null && value !== "false";
    }

    disconnectedCallback() {
        console.log("disconnected");
    }

    render() {
        this.shadow.innerHTML = `
            <style>
                label {
                    color: red;
                    display: block;
                }

                .description {
                    font-size: .65rem;
                    font-weight: lighter;
                    color: #777;
                }
            </style>

            <label>
                <input type="checkbox" />
                <slot></slot>
                <span class="description">
                    <slot name="description"></slot>
                </span>
            </label>
        `;
    }
}

customElements.define("todo-item", TodoItem);
```
