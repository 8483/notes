```html
<!DOCTYPE html>
<html>
    <head>
        <title>SheetJS Demo</title>
        <meta charset="UTF-8" />
        <script src="https://oss.sheetjs.com/sheetjs/xlsx.full.min.js"></script>
    </head>

    <body>
        <div id="app"></div>
        <script type="module" src="/app.min.js"></script>
    </body>
</html>
```

```js
let excelButton = document.getElementById("button-excel");
excelButton.addEventListener("click", () => {
    let now = new Date();
    let timestamp = `${now.toISOString().split("T")[0]} ${now.toLocaleTimeString("en-GB")}`; // 2020-04-18 14_03_05
    activitiesTable.download("xlsx", `Exel report - ${timestamp}.xlsx`);
});
```
