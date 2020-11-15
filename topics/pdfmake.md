```html
<!DOCTYPE html>
<html>
    <head>
        <title>PDFmake Demo</title>
        <meta charset="UTF-8" />
        <script src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.68/pdfmake.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.68/vfs_fonts.js"></script>
    </head>

    <body>
        <div id="app"></div>
        <script type="module" src="/app.min.js"></script>
    </body>
</html>
```

```js
module.exports = function generatePDF(type, data, title) {
    let content = [];

    switch (type) {
        case "activities":
            content = getContentActivities(data);
            break;

        case "deals":
            content = getContentDeals(data);
            break;

        case "companies":
            content = getContentCompanies(data);
            break;

        default:
            break;
    }

    // margin [left, top, right, bottom]

    let docDefinition = {
        pageSize: "A4",
        // [left, top, right, bottom] or [horizontal, vertical] or just a number for equal margins
        pageMargins: [50, 50, 50, 50],
        header: (currentPage, pageCount, pageSize) => {
            return {
                stack: [
                    {
                        columns: [
                            { text: title, fontSize: 12, color: "#888", margin: [50, 20, 0, 0], width: 280, alignment: "left" },
                            { text: getTimestamp(), fontSize: 12, color: "#888", margin: [0, 20, 50, 0], alignment: "right" },
                        ],
                    },
                    // { text: "\n" },
                    // {
                    //     canvas: [
                    //         {
                    //             type: "line",
                    //             x1: 50,
                    //             y1: 0,
                    //             x2: 510,
                    //             y2: 0,
                    //             lineWidth: 1,
                    //         },
                    //     ],
                    // },
                ],
            };
        },
        footer: (currentPage, pageCount) => {
            return { text: `${currentPage.toString()} / ${pageCount}`, alignment: "center" };
        },
        content: content,
        styles: {
            header: {
                fontSize: 10,
                bold: true,
                alignment: "center",
            },
            body: {
                fontSize: 10,
                margin: [0, 15, 0, 0],
            },
        },
    };

    const pdfDocGenerator = pdfMake.createPdf(docDefinition);
    pdfDocGenerator.getDataUrl((dataUrl) => {
        let iframe = `<iframe width="100%" height="100%" src="${dataUrl}" style="box-sizing: border-box; border: 0px;"></iframe>`;
        let newWindow = window.open();

        newWindow.document.open();
        newWindow.document.write(iframe);
        newWindow.document.close();
        newWindow.document.body.style.cssText = `
        margin: 0px;
        `;
        newWindow.document.title = title;
    });
};

function getTimestamp() {
    let now = new Date();
    let timestamp = `${now.toISOString().split("T")[0]} ${now.toLocaleTimeString("en-GB")}`; // 2020-04-18 14_03_05
    return timestamp;
}

function getContentActivities(data) {
    let content = data.map((activity, i) => {
        return {
            stack: [
                {
                    columns: [
                        {
                            text: `${i}. ${activity.activityType}`,
                            fontSize: 18,
                            bold: true,
                        },
                        {
                            text: activity.completed ? "Завршено" : "Незавршено",
                            bold: true,
                            color: activity.completed ? "#71c273" : "#cc544e",
                        },
                        `${activity.startDate} - ${activity.startTime}`,
                    ],
                },
                " ",
                activity.description,
                " ",
                activity.note,
                " ",
                {
                    text: activity.connections,
                    bold: true,
                },
                " ",
                activity.assignees,
                " ",
                " ",
                {
                    canvas: [
                        {
                            type: "line",
                            x1: 0,
                            y1: 0,
                            x2: 510,
                            y2: 0,
                            lineWidth: 1,
                        },
                    ],
                },
                " ",
                " ",
            ],
        };
    });

    return [...content];
}

function getContentDeals(data) {
    let pdfBody = [];

    data.map((item, i) => {
        pdfBody.push([
            { text: i + 1, style: "body" },
            { text: item.statusName, style: "body" },
            { text: item.name, style: "body" },
            { text: item.companyName, style: "body" },
            { text: item.userName, style: "body" },
            { text: item.note, style: "body" },
            // { text: item.product, style: "body", margin: [0, 10, 0, 0] },
            // { text: item.discountPrice.toLocaleString(), style: "body", bold: true }, //border: [false, false, true, true],
        ]);
    });

    return [
        {
            style: "tableExample",
            table: {
                headerRows: 1,
                body: [
                    [
                        { text: "#", style: "tableHeader", style: "header" },
                        { text: "Статус", style: "tableHeader", style: "header" },
                        { text: "Зделка", style: "tableHeader", style: "header" },
                        { text: "Клиент", style: "tableHeader", style: "header" },
                        { text: "Одговорен", style: "tableHeader", style: "header" },
                        { text: "Коментар", style: "tableHeader", style: "header" },
                    ],
                    ...pdfBody,
                ],
            },
            layout: "lightHorizontalLines",
            // layout: {
            //     defaultBorder: false,
            // }
        },
    ];
}

function getContentCompanies(data) {
    let pdfBody = [];

    data.map((item, i) => {
        pdfBody.push([
            { text: i + 1, style: "body" },
            { text: item.name, style: "body" },
            { text: item.activitiesCount, style: "body" },
        ]);
    });

    return [
        {
            style: "tableExample",
            table: {
                headerRows: 1,
                body: [
                    [
                        { text: "#", style: "tableHeader", style: "header" },
                        { text: "Клиент", style: "tableHeader", style: "header" },
                        { text: "Активности", style: "tableHeader", style: "header" },
                    ],
                    ...pdfBody,
                ],
            },
            layout: "lightHorizontalLines",
            // layout: {
            //     defaultBorder: false,
            // }
        },
    ];
}

let pdfButton = document.getElementById("button-pdf");
pdfButton.addEventListener("click", () => {
    // getData(true) returns the filtered data
    generatePDF("activities", activitiesTable.getData(true), `Report name - ${store.tenant}`);
});
```
