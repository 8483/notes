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
                    console.log(`Row ${row.getData().id} clicked!`);
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