# Useful Algorithms

```javascript
let data = [ 
  { id: 1, name: "a" },
  { id: 2, name: "b" },
  { id: 3, name: "c" },
  { id: 4, name: "d" },
  { id: 5, name: "e" }
]
```

## Shuffle array
```javascript
function shuffleArray(array) {
    for (var i = array.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
    return array;
}

let shuffled = shuffleArray(data);
console.log(shuffled);
```

## Sort array
```javascript
// true desc, false asc
let dynamicSort = function(field, reverse, primer){

    let key = primer ? 
    function(x) {return primer(x[field])} : 
    function(x) {return x[field]};

    reverse = !reverse ? 1 : -1;

    return function (a, b) {
        return a = key(a), b = key(b), reverse * ((a > b) - (b > a));
    };
}

let sorted = data.sort(dynamicSort("id", false));
console.log(sorted);
```

