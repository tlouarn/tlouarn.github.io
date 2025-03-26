---
title: Breadth-first search algorithm
subtitle: 
tags: algorithms python
---

In [Graph Theory 101](/2023-07-06-graph-theory-101), we introduced five concepts used to describe a graph: direction, weight, cycles, connectivity and density. In [Modeling graphs using Python](2023-07-06-modeling-graphs-using-python.md), we saw how to represent a graph using either an adjacency matrix or an adjacency list.

It is now time to learn how to traverse a connected graph. In this post, we introduce the Breadth-First Search (**BFS**) and the Depth-First Search (**DFS**) algorithms. We can run a BFS or a DFS on either an adjacency list or an adjacency matrix.

# BFS algorithm

Let us look at the below graph to understand how BFS works: from a given starting node, we list all the children. We then explore each child, and for each one of them, we list all its children. We then move another level down, etc.

![Visual overview of the BFS algorithm](tlouarn-breadth-first-search-algorithm-overview.drawio.svg)
*Figure 1: visual overview of the BFS algorithm*

In the above example, starting from A we list B and C. Moving to B, we list D and E. Moving to C, we list F and G. Moving to D, E, F and finally G, we realize they are leaf nodes so we don't list anything. When the list is empty, the algorithm stops.

Figure 1 is acyclic, but BFS needs to be able to explore cyclic graphs without getting trapped into infinite loops. To do so, we maintain a list of visited nodes and only add unvisited nodes to the "next nodes" list.

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
            print(f"visiting node {node}:", end=" ")

            neighbors = graph[node]
            for neighbor in neighbors:
                if neighbor not in visited:
                    queue.append(neighbor)
                    print(f"{neighbor}", end=" ")

            print("")
```

What we do with BFS is very versatile: we can use BFS to explore all connected nodes, or to look for a specific node. We can use it to add edges etc.
