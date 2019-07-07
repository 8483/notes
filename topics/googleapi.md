# Sheets API

1. Go to console.developers.google.com
2. Create a project.
3. Go to console.developers.google.com/apis/credentials and create API key for the project.
4. Set the sheet to public for the request to work.

The sheet can be public i.e. `Anyone who has the link can view`, and be only editable privately. This avoids the OAuth part.


```javascript
let sheet = "c750833d5021f60a1b8ff8bf0a21bb9dc74cff12"
let range = "Sheet1"
let apiKey = "6e0ece719a48d5334369bd881b4324aa957e4407"

let url = `https://sheets.googleapis.com/v4/spreadsheets/${sheet}/values/${range}?key=${apiKey}`

fetch(url)
    .then(res => res.json())
    .then(data => {
        console.log(data)
    })
    .catch(err => err);
```

### Private sheet

With private user data you would need to use OAuth.

1. Go to console.developers.google.com/apis/library and pick Sheets.
2. Configure OAuth.

