There isn't too much to say about graphs that wouldn't already be covered by different algorithms.

I'll go over the different graph representations though.

### Adjacency Matrix
Adjacency matrices are better for dense graphs where the number of edges is close to the number of vertices squared.

It's more efficient for algorithms that frequently check if there is an edge between two vertices, such as [Floyd-Warshall](), [Prim's](), [Seidel's], or [Warshall's](), or in [graph coloring]() and [clique detection]().

- Add/Remove Edge: $O(1)$

- Add/Remove Vertex: $O(|V|^2)$

- Get Adjacent/Incident: $O(|V|)$

```python
# A graph with 4 vertices
graph = [[0, 1, 1, 0],  # Connections from vertex 0
         [0, 0, 1, 0],  # Connections from vertex 1
         [1, 0, 0, 1],  # Connections from vertex 2
         [0, 0, 0, 1]]  # Connections from vertex 3, self-loop at vertex 3
```

### Adjacency List
Adjacency lists are for sparse graphs where the number of edges is much less than the number of vertices squared.

It's more efficient for algorithms that need to iterate over the neighbors of a given vertex, such as [DFS](), [BFS](), [Dijkstra's](), [A* Search](), [Bellman-Ford](), [Tarjan's](), [Hopcroft-Karp](), [Edmonds-Karp](), or [Ford-Fulkerson]().

- Add/Remove Edge: $O(deg(V))$

- Add/Remove Vertex: $O(1)$

- Get Adjacent: $O(|V|)$

- Get Incident: $O(1)$

```python
# A graph with 4 vertices
graph = {0: [1, 2],    # Vertex 0 is connected to Vertex 1 and 2
         1: [2],       # Vertex 1 is connected to Vertex 2
         2: [0, 3],    # Vertex 2 is connected to Vertex 0 and 3
         3: [3]}       # Vertex 3 has a self-loop

```

### Edge List
Edge lists are useful when the graph is extremely sparse or you're primarily interested in the edges themselves.

It's more efficient for algorithms focused on edge properties or where edges need to be sorted or processed as a collection, such as with [Kruskal's]() or [Borůvka's]()

Edge lists are much less common though, and they're not able to store isolated vertices.

- Most operations: $O(|E|)$

```python
# A graph with edges represented as (src, dest)
graph = [(0, 1), (0, 2), (1, 2), (2, 0), (2, 3), (3, 3)]
```
