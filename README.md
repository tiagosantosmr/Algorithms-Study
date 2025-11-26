# Genealogical Tree - Lowest Common Ancestors

## Problem Description

Given a directed acyclic graph (DAG) representing a genealogical tree and two vertices `v1` and `v2`, this program:

1. **Validates** whether the graph forms a valid genealogical tree
2. **Finds** all Lowest Common Ancestors (LCAs) between `v1` and `v2`

### What is a Genealogical Tree?

A genealogical tree is a directed graph where:
- Each node represents a person
- Edges point from parent to child (if `(x, y)` is an edge, then `x` is a parent of `y`)
- Each node can have **at most 2 parents** (representing biological parents)
- A node can have 0, 1, or 2 parents
- The graph must be **acyclic** (no cycles allowed)

### Definitions

- **Ancestor**: Node `P1` is an ancestor of node `P2` if there exists a path from `P1` to `P2` in the tree
- **Lowest Common Ancestor (LCA)**: Node `P3` is an LCA of `P1` and `P2` if:
  - `P3` is an ancestor of both `P1` and `P2`
  - No descendant of `P3` is also a common ancestor of `P1` and `P2`

**Note**: Unlike traditional binary trees, genealogical trees can have **multiple LCAs** for the same pair of nodes.

## Algorithm Approach

The solution uses a multi-phase approach:

1. **Graph Validation**:
   - Ensures each node has at most 2 parents
   - Detects cycles using DFS with color-coding
   - Rejects self-loops (edges from a node to itself)

2. **Ancestor Marking**:
   - Performs DFS starting from `v1`, marking all ancestors in **BLUE**
   - Performs DFS starting from `v2`, marking all ancestors in **YELLOW**
   - Nodes visited by both DFS become **GREEN** (common ancestors)

3. **LCA Extraction**:
   - Identifies all GREEN nodes (common ancestors)
   - Marks parent nodes of common ancestors as **BLACK**
   - LCAs are the GREEN nodes that remain unmarked (have no common ancestor descendants)

## Input Format

The input file contains:

```
v1 v2
n m
x1 y1
x2 y2
...
xm ym
```

Where:
- `v1`, `v2`: Target vertex identifiers (between 1 and n)
- `n`: Number of vertices (n ≥ 1)
- `m`: Number of edges (m ≥ 0)
- Each subsequent line `x y` indicates that `y` is a child of `x` (i.e., `x` is a parent of `y`)

Vertex identifiers are integers from 1 to n.

## Output Format

- **`0`**: If the graph is not a valid genealogical tree
- **Sequence of integers**: All LCAs sorted in ascending order, separated by spaces, ending with a space
- **`-`**: If no common ancestors exist between `v1` and `v2`

## Examples

### Example 1

**Input:**
```
5 6
8 9
1 2
1 3
2 6
2 7
3 8
4 3
4 5
```

**Output:**
```
2 4 
```

**Explanation**: Nodes 2 and 4 are both LCAs of nodes 5 and 6.

### Example 2

**Input:**
```
5 2
8 9
1 2
1 3
2 6
2 7
3 8
4 3
4 5
7 5
8 6
```

**Output:**
```
2
```

**Explanation**: Only node 2 is an LCA of nodes 5 and 2.

### Example 3

**Input:**
```
2 4
8 9
1 2
1 3
2 6
2 7
3 8
4 3
4 5
7 5
8 6
```

**Output:**
```
-
```

**Explanation**: Nodes 2 and 4 have no common ancestors.

## Compilation and Execution

### Compile

```bash
g++ -std=c++11 -O3 -Wall p2.cpp -lm -o p2
```

### Run

```bash
./p2 < input.txt
```

Or using standard input:

```bash
echo "5 6
8 9
1 2
1 3
2 6
2 7
3 8
4 3
4 5" | ./p2
```

## Test Generator

The repository includes `randGeneoTree.cpp`, a utility to generate random test cases:

```bash
g++ randGeneoTree.cpp -o randGeneoTree
./randGeneoTree <vertices> <probability> [seed]
```

- `vertices`: Number of vertices in the graph
- `probability`: Probability (0.0-1.0) of creating an edge (u,v) where u < v
- `seed`: Optional random seed for reproducibility

**Example:**
```bash
./randGeneoTree 10 0.3 42 | ./p2
```

## Implementation Details

- **Data Structure**: Adjacency list representation using a transpose graph (each node stores its parents)
- **Time Complexity**: O(V + E) where V is vertices and E is edges
- **Space Complexity**: O(V)

## Authors

- Valentim Santos (99343)
- Tiago Santos (99333)
