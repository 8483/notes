# Swap numbers

```js
[a, b] = [b, a];
```

# Sum

```js
array.reduce((accumulator, item) => accumulator + item.value, 0);
```

# Max value of object property in array

```js
Math.max(...array.map((object) => object[property]));
```

# Longest string in an array

```js
array.reduce((a, b) => (a.length > b.length ? a : b), "");
```
