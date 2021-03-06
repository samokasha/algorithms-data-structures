# Graph

## What is a Graph?

A graph is a non-linear data structure consisting of vertices connected by edges.

Graphs can be:

- Weighted or Unweighted
  - A value is assigned (or not assigned) to the distances between vertices
- Directed or Undirected
  - Directions are assigned (or not assigned) between vertices

![Unweighted Undirected Graph](graph_1.jpg)
![Directed Graph](graph_2.jpg) ![Weighted Directed Graph](graph_3.jpg)

Graphs can be used for building:

- Maps
- Social Networks
- Recommendation systems

## The Graph: Adjacency Matrix vs Adjacency List

The graph and its vertices and edges can be represented in the following ways:

- **Adjacency Matrix**
  - A matrix maps a vertex's relationship to _every other vertex_. This can be accomplished by indicating 0 for no edge (connection) between vertices or 1 to indicate an edge exists between vertices.
  * Since all relationships are mapped, whether there is a connection or not, an adjacency matrix can be faster than an adjacency list for looking up a specific edge, although at a cost of taking up more memory space and being slower in iterating over all edges.
- **Adjacency List**
  - A list maps only each vertex's adjacent vertices (ie. neighbouring nodes).
  * Since only adjacent vertices are mapped for each vertex, iterating over all edges is faster than in an adjacency matrix but slower for looking up a specific edge.

**Note**: In this implementation below we will be using an **adjacency list**.

## Creating the Graph Class

```javascript
class Graph {
  constructor() {
    this.adjacencyList = {};
  }
}
```

The adjacency list holds all of the vertices and each vertex holds connections to other vertices (adjacent vertices).

## Adding Methods

### Adding a vertex

Adds a vertex to the graph.

#### Code

```javascript
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
```

### Adding an Edge

Adds an edge(or a connection) to the specified vertex.

#### Code

```javascript
  addEdge(vertex1, vertex2) {
    this.adjacencyList[vertex1].push(vertex2);
    this.adjacencyList[vertex2].push(vertex1);
  }
```

### Removing an Edge

Removes an edge from the specified vertex.

#### Implementation Overview

Filter out the edge from the specified vertex's adjacent vertices. Repeat on the filtered out vertex's adjacent vertices but this time filter out the vertex specified earlier.

#### Code

```javascript
  removeEdge(vertex1, vertex2) {
    this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(vertex => vertex !== vertex2)
    this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(vertex => vertex !== vertex1)
  }
```

### Remove Vertex

Removes a specified vertex.

#### Implementation Overview

First, remove the vertex from any other vertex's adjacency list. Then, delete the vertex.

#### Code

```javascript
  removeVertex(vertex) {
    this.adjacencyList[vertex].forEach(adjacent => this.removeEdge(adjacent, vertex));
    delete this.adjacencyList[vertex];
  }
```

## Traversal

### Depth First Traversal - Recursive

Traverses the graph by repeatedly visiting the next adjacent vertex first.

#### Implementation Overview

Create an array for the map the traversal path and an object to record visited that have been visited.

Create a traverse function which first checks to see if a vertex has been visited. If not it first pushes that vertex to the array mapping the traversal path then for each adjacent vertex, it checks if it has been visited. If it has not, then it calls itself recursively on that (adjacent) vertex.

Call the recursive traverse function. Then, return the array that mapped the traversal path.

#### Pseudocode

- Create an array called `path` to map the traversal path
- Create an object to map the visited vertices
- Create a helper function called `traverse` that:
  - If vertex is not in the graph, return `null`
  - Adds vertex to `path`
  - Adds vertex to `visited`
  - For each of the adjacent vertices:
    - If they have not been visited recursively call `traverse`
- Return `path`

#### Code

```javascript
  dfsRecursive(start) {
    const path = [];
    const visited = {};
    const adjacencyList = this.adjacencyList;

    (function traverse(vertex) {
      if (!vertex) return null;
      visited[vertex] = true;
      path.push(vertex);

      adjacencyList[vertex].forEach(adjacent => {
        if (!visited[adjacent]) {
          return traverse(adjacent);
        }
      });
    })(start);
    return path;
  }
```

### Depth First Search - Iterative

Traverses the graph by repeatedly visiting the next adjacent vertex first.

#### Implementation Overview

Implement a stack and add to it the starting vertex. Create a result array. Create an object to map over visited vertices.

While the stack is not empty, remove the last vertex from it and add that removed vertex's adjacent vertices into the stack. This pattern will continually process adjacent vertices first, resulting in depth first search.

Return the result array.

#### Code

```javascript
  dfsIterative(start) {
    const stack = [start];
    const visited = {};
    const path = [];
    visited[start] = true;
    let vertex;
    while (stack.length) {
      vertex = stack.pop();
      path.push(vertex);
      this.adjacencyList[vertex].forEach(adjacent => {
        if (!visited[adjacent]) {
          visited[adjacent] = true;
          stack.push(adjacent);
        }
      });
    }
    return path;
  }
```

### Breadth First Search

Traverses the graph by first visiting all siblings of the current vertex before traversing the adjacent vertices further.

#### Implementation Overview

Implement a queue and add to it the starting vertex. Create a result array. Create an object to map over visited vertices.

While the queue is not empty, remove the first vertex from it and add that removed vertex's adjacent vertices into the queue. This pattern will always process the current vertex before adjacent vertices, resulting in breadth first search.

Return the result array.

#### Code

```javascript
  bfsIterative(start) {
    const queue = [start];
    const visited = {};
    const path = [];
    visited[start] = true;
    let vertex;
    while (queue.length) {
      vertex = queue.shift();
      path.push(vertex);
      this.adjacencyList[vertex].forEach(adjacent => {
        if (!visited[adjacent]) {
          visited[adjacent] = true;
          queue.push(adjacent);
        }
      });
    }
    return path;
  }
```
