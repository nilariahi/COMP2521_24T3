# Weighted and Directed Graphs

1.  Give the adjacency matrix and adjacency list representation of the following graph:

    ![img](https://cgi.cse.unsw.edu.au/~cs2521/24T3/tut/7/weighted-graph-rep/weighted-graph.svg)

### Answer:

![IMG_0644](/Users/nilariahi/COMP2521_24T3/assets/IMG_0644.jpg)

# Graph Traversal

1.  Consider the breadth-first and depth-first traversal algorithms below and the following graph:

    ![img](https://cgi.cse.unsw.edu.au/~cs2521/24T3/tut/7/graph-traversals/graph.svg)

    ```pseudocode
    bfs(G, src):
        Input: graph G, vertex src
        
        create visited array, initialised to false
        create predecessor array, initialised to -1
        create queue Q
        
        mark src as visited
        enqueue src into Q
        
        while Q is not empty:
            dequeue v from Q
            
            for each neighbour w of v:
                if w has not been visited:
                    mark w as visited
                    set predecessor of w to v
                    enqueue w into Q
    ```

    ```pseudocode
    dfs(G, src):
        Input: graph G, vertex src
        
        create visited array, initialised to false
        create predecessor array, initialised to -1
        create stack S
        
        push src onto S
        
        while S is not empty:
            pop v from S
            if v has been visited:
                continue
            
            mark v as visited
            
            for each neighbour w of v:
                if w has not been visited:
                    set predecessor of w to v
                    push w onto S
    ```

    Trace the execution of the traversal algorithms, and show the state of the `visited` and `pred` arrays and the `Queue`(BFS) or `Stack` (DFS) at the end of each iteration, for each of the following function calls:

    ```pseudocode
    bfs(g, 0);
    bfs(g, 3);
    dfs(g, 0);
    dfs(g, 3);
    ```

### Answer

See solution slides here: [graph_traversal.pdf](graph_traversal.pdf) 

# Graph Problems

1.  What is the difference between a *connected* graph and a *complete* graph? Give simple examples of each.

    In a connected graph each vertex $v$ has a path to every other vertex $w$.

    In a complete graph each vertex $v$ has an edge to every other vextex $w$.

2.  Consider the following graph with multiple connected components:

    ![img](https://cgi.cse.unsw.edu.au/~cs2521/24T3/tut/7/connected-components/connected-components-1.png)

    And a vertex-indexed connected components array that might form part of the `Graph` representation structure for this graph:

    ```c
    cc[] = {0, 0, 0, 1, 1, 1, 0, 0, 0, 1}
    ```

    1.  Show how the `cc[]` array would change if edge d was removed

        No change.
    2.  Show how the `cc[]` array would change if edge b was removed

        Component 0 is split into two connectec components because edge b is a bridge. We can let component 0 include vertices $\set{0, 1, 7}$ and create a new component 2 that includes vertices $\set{2,6,8}$. So the `cc[]`array would look like this:

        ```c
        cc[] = {0, 0, 2, 1, 1, 1, 2, 0, 2, 1}
        ```

3.  The following question is an example of how graphs can be used to solve more abstract problems.

    Suppose you have $n$ variables $(x_1, x_2, ..., x_n)$, $m$ equalities of the form $x_i=x_j$ (where $i≠j$), and $k$ inequalities of the form $x_i≠x_j$ (where $i≠j$). Come up with an algorithm (described in plain English or pseudocode) to determine whether this system of equations is consistent or contradictory.

    For example, suppose we have three variables $(x_1, x_2, x_3)$, the equality $x_1=x_2$, and the two inequalities $x_1≠x_3$ and $x_2≠x_3$. These equations are consistent because we can, for example, assign $x_1=x_2=1$ and $x_3=2$, and all equations would be satisfied.

    For example, suppose we have three variables $(x_1,x_2,x_3)$, the two equalities $x_1=x_2$ and $x_1=x_3$, and the inequality $x_2≠x_3$​. These equations are contradictory because there is no way to assign values to variables such that all equations are satisfied..

    ### Answer

    -   Create an undirected, unweighted graph where the vertices are the $n$ variables, and for each equality $x_i=x_j$ insert an edge between vertex $x_i$ and $x_j$​.
    -   If two vertices (variables) are in the same connected component it means that they must be equal. Compute the connected components array for this graph.
    -   For each inequality $x_i \neq x_j$ check that $x_i$ and $x_j$ are in different connnected components. If they are in the same component then the equations are contradictory.

4.  What is the difference between a Hamiltonian path/circuit and an Euler path/circuit? Identify any Hamiltonian/Euler paths/circuits in the following graphs:

    ![img](https://cgi.cse.unsw.edu.au/~cs2521/24T3/tut/7/euler-hamilton/euler-hamilton.png)

    A Hamiltonian path visits each vertex in the graph exactly once. An Euler path visits each edge exactly once. A Hamiltonian/Euler circuit is a Hamiltonian/Euler path that starts and ends at the same vertex.

    -   Graph 1: Hamiltonian path, Euler path
    -   Graph 2:  Hamiltonian path + circuit, Euler path + circuit
    -   Graph 3: None
    -   Graph 4: Hamiltonian path + circuit

5.  Write a function to check whether a path, supplied as an array of `struct edge`s is an Euler path. Assume the function has interface:

    ```c
    bool isEulerPath(Graph g, struct edge e[], int nE)
    ```

    where `e[]` is an array of `nE` edges, in path order.

    ### Answer

    ```c
    // structs defined for the sake of the exercise
    typedef struct {
        int nV;         // #vertices
        int nE;         // #edges
        bool **edges;   // adj matrix
    } *Graph;
    
    // undirected, unweighted edge rep
    struct edge {
        int v;
        int w;
    };
    
    // Time complexity: O(E^2)
    bool isEulerPath(Graph g, struct edge e[], int nE) {
        if (nE != g->nE) return false;
    
        // do all edges in e[] exist in g
        for (int i = 0; i < nE; i++) {
            int v = e[i].v;
            int w = e[i].w;
            if (!g->edges[v][w]) return false;
        }
        
        // does e[] form a valid path. e.g. for two
        // adjacent edges (v1, w1) and (v2, w2), check
        // that w1 == v2
        for (int i = 0; i < nE - 1; i++) {
            if (e[i].w != e[i + 1].v) return false;
        }
    
        // does every edge in e[] appear exactly once
        for (int i = 0; i < nE; i++) {
            struct edge e1 = e[i];
            for (int j = i + 1; j < nE; j++) {
                struct edge e2 = e[j];
                // undirected edges so need to check both ways
                if (e1.v == e2.v && e1.w == e2.w) return false;
                if (e1.w == e2.v && e1.v == e2.w) return false;
            }
        }
    
        // e[] satisfies definition of Euler path if we got here
        return true;
    }
    
    ```

    