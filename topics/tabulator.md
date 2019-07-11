[Tabulator](http://tabulator.info) is a datagrid library.

# TODO Example
The example below renders API data in a table, and adds new data.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width">
        <title>TODO</title>
        <link href="https://unpkg.com/tabulator-tables@4.2.7/dist/css/tabulator.min.css" rel="stylesheet">
        <script type="text/javascript" src="https://unpkg.com/tabulator-tables@4.2.7/dist/js/tabulator.min.js"></script>
    </head>

    <body>
        <input id="todo-input"/>
        <button id="todo-button-submit">Submit</button>
        <div id="example-table"></div>
    </body>

    <script>
        // Get data
        fetch("https://jsonplaceholder.typicode.com/todos")
            .then(res => res.json())
            .then(data => {
            renderTable(data);
        })
        .catch(err => console.log(err));

        // Table generation
        function renderTable(data){
            let table = new Tabulator("#example-table", {
                height:500, 
                data: data,
                addRowPos:"top",
                layout:"fitColumns",
                columns:[
                    {title:"Title", field:"title"},
                    {title:"Completed", field:"completed"},
                ],
                rowClick:function(e, row){
                    console.log(row.getData());
                }
            });

            // Adding new TODO
            document.getElementById("todo-button-submit").addEventListener("click", () => {
                let todo = document.getElementById("todo-input").value;

                if(todo != ""){
                    // POST TODO to server
                    fetch('https://jsonplaceholder.typicode.com/todos', {
                        method: 'POST',
                        body: JSON.stringify({
                            title: todo,
                            completed: false,
                        }),
                        headers: {
                            "Content-type": "application/json; charset=UTF-8"
                        }
                    })
                    // RESPONSE TODO
                    .then(response => response.json())
                    .then(json => {
                        console.log(json);
                        // Add TODO to table
                        table.addRow(json);
                        document.getElementById("todo-input").value = ""
                    })
                }
            })
        }
    </script>
</html>
```

# Input Form

[Prototype](https://jsbin.com/yabegevada/edit?html,js,console,output) for adding and updating data via an input form.

**HTML**
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width">
        <title>TODO</title>
        <link href="https://unpkg.com/tabulator-tables@4.2.7/dist/css/tabulator.min.css" rel="stylesheet">
        <script type="text/javascript" src="https://unpkg.com/tabulator-tables@4.2.7/dist/js/tabulator.min.js"></script>
    </head>

    <body>
        <button id="todo-button-new">+</button>
        <input id="todo-input"/>
        <button id="todo-button-submit">Submit</button>
        <button id="todo-button-new">x</button>
        <div id="example-table"></div>
    </body>
</html>
```
**Javascript**
```javascript
// Initial data and table
fetch("https://jsonplaceholder.typicode.com/todos?&_limit=5")
    .then(res => res.json())
    .then(data => {
    renderTable(data);
})
.catch(err => console.log(err));

// Used for resetting state
let initState = {
    id: null,
    userId: null,
    title: null,
    completed: false,
}

let state = initState;

// Used for populating inputs in setState
let inputPropertyMapping = [
    {property: "title", inputId: "todo-input"}
]

function setState(newDataObject){
    // Updates state with new data
    state = {...state, ...newDataObject};
    
    // Populate inputs
    inputPropertyMapping.map(inputPropertyMappingItem => {
        let value = newDataObject[inputPropertyMappingItem.property]
        let input = document.getElementById(inputPropertyMappingItem.inputId);
        if(input) input.value = value;
    })
}

// Update state on all input changes
document
    .querySelectorAll('input')
    .forEach(e => e.addEventListener("input", () => {
        //   console.dir(e);
        setState({[e.name]: e.value})
        console.log(state)
    })
);

function clearInputs(){
    inputPropertyMapping.map(item => {
        let input = document.getElementById(item.inputId);
        if(input) input.value = "";
    })
}

// Table generation
function renderTable(data){
    let table = new Tabulator("#example-table", {
        height:500, 
        data: data,
        addRowPos:"top",
        layout:"fitColumns",
        columns:[
            {title:"Title", field:"title"},
            {title:"Completed", field:"completed"},
        ],
        rowClick:function(e, row){
            setState(row.getData());
            console.log(state)
        }
    });
    
    console.log("Initial state: ", state)
    
    document.getElementById("todo-button-new").addEventListener("click", () => {
        setState(initState)
        console.log(state)
    });

    // SUBMIT TODO
    document.getElementById("todo-button-submit").addEventListener("click", () => {
        // POST TODO to server
        if(state.id == null){
            fetch('https://jsonplaceholder.typicode.com/todos', {
                method: 'POST',
                body: JSON.stringify(state),
                headers: {
                    "Content-type": "application/json; charset=UTF-8"
                }
            })
            .then(response => response.json())
            .then(json => {
                console.log("POST Response data:", json);
                // Add TODO to table
                table.addData(json);
                clearInputs();
            })
        }
        
        // PATCH TODO to server
        if(state.id != null){
            fetch(`https://jsonplaceholder.typicode.com/todos/${state.id}`, {
                method: 'PATCH',
                body: JSON.stringify(state),
                headers: {
                    "Content-type": "application/json; charset=UTF-8"
                }
            })
            .then(response => response.json())
            .then(json => {
                console.log("PATCH Response data:", json);
                // Update TODO in table
                table.updateData([json]);
                clearInputs();
            })
        }
    })
}
```