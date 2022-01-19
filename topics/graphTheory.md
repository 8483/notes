# Graphs

Graphs are collections of nodes (vertices) and edges.

Nodes are things, edges are connections, and graphs describe the relationships.

![Graphs](../pics/graphTheory/graphTheory_graphs.jpg)

# Adjacency list

The best way to represent graphs programatically is with a `hash map` data structure i.e. `object` in Javascript, `dictionary` in Python, `unordered map` in C#/Java.

```js
let graph = {
    a: ["b", "c"], // neighbors
    b: ["d"],
    c: ["e"],
    d: [],
    e: ["b"],
    f: ["d"],
};
```

# Traversal algorithms

They show if we can travel to some node.

![Traversal](../pics/graphTheory/graphTheory_traversal.jpg)

## Depth first traversal

Exploring one direction as far as possible before switching directions.

Best data structure: **Stack** (FILO)

![Depth traversal](../pics/graphTheory/graphTheory_traversal_depth.jpg)

### Iterative

```js
function depthFirst(graph, source) {
    const stact = [source];

    while (stack.length > 0) {
        const current = stack.pop();
        console.log(current);

        for (let neighbor of graph[current]) {
            stack.push(neighbor);
        }
    }
}

depthFirst(graph, "a"); // abdfce
```

### Recursive

```js
function depthFirst(graph, source) {
    console.log(source);
    for (let neighbor of graph[source]) {
        depthFirst(graph, neighbor);
    }
}

depthFirst(graph, "a"); // abdfce
```

## Breadth first traversal

Explore all directions evenly.

Best data structure: **Queue** (FIFO)

![Breadth](../pics/graphTheory/graphTheory_traversal_breadth.jpg)

### Iterative

```js
function breadthFirst(graph, source) {
    const queue = [source];

    while (queue.length > 0) {
        const current = queue.shift();
        console.log(current);

        for (let neighbor of graph[current]) {
            queue.push(neighbor);
        }
    }
}

depthFirst(graph, "a"); // abcedf
```
