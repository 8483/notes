```html
<!DOCTYPE html>
<html>
    <head>
        <title>Tagify Demo</title>
        <meta charset="UTF-8" />

        <script src="https://cdnjs.cloudflare.com/ajax/libs/tagify/3.21.0/tagify.min.js" integrity="sha512-YKRkB65j3s2S05MOobXwnLQvUw0asCy+5Dt8aIVK6rPqI4RxJmGLLExUObcGjn0K8/kQDNXXwSeLFkqXwXYtfA==" crossorigin="anonymous"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/tagify/3.21.0/tagify.polyfills.min.js" integrity="sha512-SvqV96Tz5KzauECowu+oMXacoJXm29uMw9GBl3sNW8nQZXxnLUzFP27dGn9SOSFuugSmXL6PedEbWdijxPkDVg==" crossorigin="anonymous"></script>
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/tagify/3.21.0/tagify.min.css" integrity="sha512-91wa7heHLbuVdMrSXbWceVZva6iWDFlkFHnM+9Sc+oXFpTgw1FCqdnuaGBJfDVuNSNl0DwDmeGeJSORB0HyLZQ==" crossorigin="anonymous" />
    </head>

    <body>
        <script src="src/index.js"></script>
    </body>
</html>
```

```js
let tagsInput = document.createElement("input");
tagsInput.className = "activity-entity-tags";
document.body.append(tagsInput);

let tags = [
    {
        id: 1,
        value: "foo 1",
        searchBy: "foo 1",
    },
    {
        id: 2,
        value: "bar 2",
        searchBy: "bar 2",
    },
    {
        id: 3,
        value: "baz 3",
        searchBy: "baz 3",
    },
];

let tags = new Tagify(tagsInput, {
    templates: {
        tag: function (tagData) {
            try {
                return `
                    <tag 
                        title='${tagData.value}' 
                        contenteditable='false' 
                        spellcheck="false" 
                        class='tagify__tag ${tagData.class ? tagData.class : ""}' 
                        ${this.getAttributes(tagData)}
                    >
                        <x title='remove tag' class='tagify__tag__removeBtn'></x>
                        <div>
                            <span class='tagify__tag-text'>${tagData.value}</span>
                        </div>
                    </tag>
                `;
            } catch (err) {
                console.log(err);
            }
        },

        dropdownItem: function (tagData) {
            try {
                return `
                    <div 
                        class='tagify__dropdown__item ${tagData.class ? tagData.class : ""}' 
                        style="font-size: 8pt;"
                    >
                      <span class="tags-dropdown-value">${tagData.searchBy}</span>
                    </div>
                `;
            } catch (err) {
                console.log(err);
            }
        },
    },
    enforceWhitelist: true,
    whitelist: tags,
    backspace: false,
    editTags: false,
    delimiters: ";",
    dropdown: {
        // fuzzySearch: true,
        maxItems: 500,
        // searchKeys: ["searchBy"],
        enabled: 0, // suggest tags after a single character input, zero i show on focus
        classname: "extra-properties", // custom class for the suggestions dropdown
    }, // map tags' values to this property name, so this property will be the actual value and not the printed value on the screen
});

tags.addTags([tags[0]]);

tags.on("dropdown:show", (e) => {
    console.log("showing");
});

tags.on("dropdown:select", (e) => {
    console.log("selecting");
    console.log(e.detail);
});
```
