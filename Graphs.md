#leetcode #dsa 

> Reference : [Striver Graph Playlist](https://www.youtube.com/playlist?list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn)

### Terminologies 
**Vertex / Node** – A fundamental unit of a graph representing an entity or point.
**Edge** – A connection between two vertices that represents a relationship or path.
**Directed Graph** – A graph where edges have a direction, meaning the connection goes from one vertex to another specifically (A → B).
**Undirected Graph** – A graph where edges do not have direction, meaning the connection is mutual (A — B).
**Weighted Graph** – A graph where each edge has an associated numeric value (weight), often representing cost, distance, or time.
**Unweighted Graph** – A graph where all edges are treated equally without weights.
**Adjacency List** – A representation that stores each vertex along with the list of vertices it is connected to.
**Adjacency Matrix** – A 2D matrix used to represent edges between vertices in tabular form.
**Degree of a Vertex** – The number of edges incident to a vertex. In directed graphs, it’s split into in-degree and out-degree.
**Component of Graph** – A graph which has multiple disconnected clusters
**In-degree** – The number of incoming edges to a vertex (how many vertices point to it).
**Out-degree**  – The number of outgoing edges from a vertex (how many vertices it points to).
**Total degree** = sum of degrees of all vertices 
	For undirected graphs, total degree = *2 x No. of total edges in graph*
### Types of Graphs
![[Types of Graphs.png|550]]

### Graph Representation

#### 1. Adjacency Matrix
- 2D array `int[][] mat` where `mat[u][v] = 1` (or weight) if edge exists, else 0 (or INF)
- Instead of `int` matrix, we can also use `boolean` for undirected graph
#### 2. Adjacency List
- for each vertex keep a list of neighbors in a `List<List<Integer>>` 
- for weighted graphs, you can create a custom class that stores edge weights too

### BFS Traversal


