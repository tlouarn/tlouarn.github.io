---
title: Breadth-first search algorithm
subtitle: 
tags: algorithms python
---

In [Graph Theory 101](/2023-07-06-graph-theory-101), we introduced five concepts used to describe a graph: direction, weight, cycles, connectivity and density. In [Modeling graphs using Python](2023-07-06-modeling-graphs-using-python.md), we saw how to represent a graph using either an adjacency matrix or an adjacency list.

It is now time to learn how to traverse a connected graph. In this post, we introduce the Breadth-First Search (**BFS**) and the Depth-First Search (**DFS**) algorithms. BFS progresses layer by layer, listing all the children of a given node. In contrast, DFS would delve deep into a given branch all the way to the leaf before backtrackig and exploring the next branch. We can run a BFS or a DFS on either an adjacency list or an adjacency matrix.

# Example

In this article we will consider the example graph below to explain BFS and DFS.

![Example of connected tree](/assets/images/tlouarn-breadth-first-search-algorithm-example-connected-tree.drawio.svg)
*Figure 1: example connected tree*

Let us build this graph as an adjacency list using Python:

```python
from collections import defaultdict

# Input
edges = [["A", "B"], ["A", "C"], ["B", "D"], ["B", "E"], ["C", "F"], ["C", "G"]]

# Build adjacency list
graph = defaultdict(list)
for edge in edges:
    graph[edge[0]].append(edge[1])
    graph[edge[1]].append(edge[0])
```

# BFS algorithm

Let us look at the below graph to understand how BFS works: from a given starting node, we list all the children. We then explore each child, and for each one of them, we list all its children. We then move another level down, etc.

![Visual overview of the BFS algorithm](/assets/images/tlouarn-breadth-first-search-algorithm-overview.drawio.svg)
*Figure 2: visual overview of the BFS algorithm*

In the above example, starting from A we list B and C. Moving to B, we list D and E. Moving to C, we list F and G. Moving to D, E, F and finally G, we realize they are leaf nodes so we don't list anything. When the list is empty, the algorithm stops.

Figure 1 is acyclic, but BFS needs to be able to explore cyclic graphs without getting trapped into infinite loops. To do so, we maintain a list of visited nodes and only add unvisited nodes to the "next nodes" list.

To implement BFS, we use a **queue** data structure (`deque` object in Python).

```python
from collections import deque

AdjacencyList = dict[set]

def bfs(graph: AdjacencyList, node: int) -> None:
    visited = set()
    queue = deque()
    queue.append(node)

    while queue:
        node = queue.popleft()
        if node not in visited:
            visited.add(node)
            print(f"{node}:", end=" ")

            neighbors = graph[node]
            for neighbor in neighbors:
                if neighbor not in visited:
                    queue.append(neighbor)

```

Let us run the code on our example to see the order in which the nodes were explored:
```shell
A B C D E F G
```

What we do with BFS is very versatile: we can use BFS to explore all connected nodes, or to look for a specific node. We can use it to add edges etc.

# DFS algorithm

Iterative implementation

```python




```