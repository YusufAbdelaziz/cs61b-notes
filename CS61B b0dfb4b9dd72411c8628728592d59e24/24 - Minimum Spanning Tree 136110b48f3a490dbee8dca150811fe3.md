# 24 - Minimum Spanning Tree

### Warm-up Problem: Cycle Detection.

![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled.png)

- Answer
    - First approach: You can use DFS and keep going until you see a marked vertex.
        - Note that you should not count the node you’ve came from. So going from 0 to 1, the vertex 1 should consider 0 as a neighbor that can be visited again because that is not a cycle.
        - This approach will work from any start vertex as long as the graph is connected.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%201.png)
        
    - Second approach: Use a WeightedQuickUnion (Go back to union-find lecture) object.
        - For each edge we check if two vertices are connected.
            - if not, union them.
            - if so, there’s a cycle.
        - Important thing to note here, calling `union(4, 6)` and `union(4, 5)` will put 4, 5, and 6 in the same set. How can we tell if there’s a cycle? we can check if 5 and 6 is connected before and after calling any of the function. This means that we `isConnected(5, 6)` is called twice and returns true, then there’re two ways to go from 5 to 6 which is basically a cycle.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%202.png)
        

### Spanning Trees: Definition, Usefulness, and how they differ from SPTs (Shortest Path Trees).

- What is a spanning tree?
    - A spanning tree T is subgraph of a graph G where **G** should be **undirected** and **T** should be:
        - **Connected**.
        - **Acyclic**.
        - **Includes all the vertices**.
    - The **first two properties** make it a **tree**, and the **latter** makes it a **spanning** one.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%203.png)
        
    - Examples of spanning trees:
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%204.png)
        
- What is a minimum spanning tree?
    - A minimum spanning tree is a spanning tree of minimum total weight.
    - Example: Directly connecting buildings by power lines.
    - Note that we don’t care at all about how does a spanning tree looks like (i.e. spindly or bushy), what we care most is to achieve the minimum total weight.
- Quick Quiz: How many spanning trees do you see?
    
    ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%205.png)
    
    - Answer
        - Only 3 is a spanning tree.
        - 1 is not because it’s disconnected.
        - 2 is not because there’s a cycle.
        - As mentioned, a spanning tree should be connected, acyclic and includes all vertices.
- How useful is MSTs?
    
    ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%206.png)
    
    - [Link](https://www.ics.uci.edu/~eppstein/gina/mst.html).
    - We’ll use it as a math problem. However, you should be aware of its applications.
- MSTs vs SPTs?
    - Before mentioning the difference, try to come up with a MST for this graph.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%207.png)
        
        - Answer
            - We will have 3 edges passing through A, B, D, and C.
                
                ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%208.png)
                
    - Now try to the the node at which the SPT is MST.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%209.png)
        
        - Answer.
            
            ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2010.png)
            
    - The main difference is:
        - A **SPT** depends on the **start vertex** because it tells you how to get from the source to everywhere.
        - A **MST** is a global property of a graph so a source vertex is irrelevant.
    - It may happen that the MST is the same as an SPT for a specific vertex like the previous quiz.
    - So to get MST from a SPT, you need two steps:
        1. Find the correct vertex.
        2. Run Dijkstra’s.
    - Now we have a quiz that consists of two problems:
        - What is the MST of this graph?
        - Is there a node whose SPT is the same as MST?
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2011.png)
        
        - Answer
            
            ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2012.png)
            
            ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2013.png)
            

### What is Cut Property in MST?

- Cut Property.
    - Before we explain how cut property is useful to find MST, we should mention some terms.
    - A ****cut**** is an assignment of a graph’s nodes to non-empty sets. So for this example, the graph is split to two non-empty sets which are white and gray.
    - A **************************crossing edge************************** is an edge which connects a node from one set to a node from the other set.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2014.png)
        
    - **Cut property**: *Given any cut, minimum weight crossing edge is **guaranteed** to be in the **MST**.*
    - A quick quiz:
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2015.png)
        
        - Answer
            - 0 - 2 is a crossing edge and it’s the minimum one and it’s guaranteed that it belongs to the MST.
- Cut Property Proof (By Contradiction).
    - Proof by contradiction is done by supposing that the minimum crossing edge e is not in the MST.
    - Note that adding e to the MST will create a cycle because the initial MST includes all vertices so adding another edge will result in a cycle.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2016.png)
        

### Prim’s Algorithm: What is it, Conceptual Steps and How does it utilizes Cut Property.

- What is Prim’s?
    - Prim’s is an algorithm that uses cut property to find the minimum spanning tree. It only works with **undirected** graphs.
- Prim’s algorithm steps.
    - Start from some arbitrary start node.
    - Repeatedly add shortest edge (mark it black or in code mark it in `edgeTo` array) that has one node inside the MST under construction.
    - Repeat until V-1 edges.
- How does Prim’s utilize cut property? with an example.
    - It utilizes the cut property by repeatedly adding shortest edges.
    - So taking an example, we start with a random start node.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2017.png)
        
    - We start with 0 and mark it in `edgeTo` array which is the same as marking the edge.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2018.png)
        
    - The cut property states that the minimum crossing edge (edge 0-2) is guaranteed to be part of the MST. So edge 0-2 is selected.
    - After that we have couple edges to select
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2019.png)
        
    - We’ll choose the minimum which is edge 2-4.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2020.png)
        
    - And we keep repeating the process until we get the MST.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2021.png)
        
    - It’s worth to note that there’s two edges with weight 2 so edges here are not unique. Selecting either 0-1 or 4-3 will work but both edges are not guaranteed to be in the MST.
    - Consider changing the edge 1-3 from 11 to 1 and see how that affects the MST.
    - There’s a chance that a graph may have multiple MSTs when there’re non-unique edges.
    

### Efficient Prim’s Algorithm.

- What is the problem of the natural implementation of the conceptual version of Prim’s algorithm?
    - It’s highly inefficient because every time we try to add an edge to the MST, we have to iterate over the remaining purple edges.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2022.png)
        
- What can we do to enhance the implementation?
    - We can use some cleverness and PQ to speed things up.
- Prim’s Example.
    - Insert all vertices into fringe PQ, storing vertices in order of distance from tree.
    - Repeat: Remove (closest) vertex v from PQ, and relax all edges pointing from v.
    - We start with 0 and remove it from the fringe. However, you can start literally from any vertex in the graph and you will eventually get the same MST.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2023.png)
        
    - We now relax edges from 0 and change them to the smaller values. It’s worth to note that we change it to the edge value not the distance from the source (since source in MST is meaningless) and that’s the difference between Prim’s and Dijkstra’s.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2024.png)
        
    - Now we remove the node with the smallest edge which is 2.
    - Now it’s important to mention that the removal of a node from the fringe is ********equivalent to******** adding it to the MST.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2025.png)
        
    - We keep repeatedly visiting nodes with smallest edges in the fringe and relax neighbor vertices if that is possible.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2026.png)
        
    - The whole demo is [here](https://docs.google.com/presentation/d/1GPizbySYMsUhnXSXKvbqV4UhPCvrt750MiqPPgU-eCY/edit#slide=id.g9a60b2f52_0_783).
    
- Prim’s vs. Dijkstra’s
    
    ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2027.png)
    
- Prim’s Runtime.
    
    ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2028.png)
    

### Kruskal’s Algorithm: What is it, How does it work, What is its Runtime?

- What is it?
    - Another algorithm similar to Prim’s to find the MST of a graph.
- What is the main idea behind it?
    - Basically sort all the edges in ascending order.
    - Add edges to the MST unless a cycle is created.
- Does Kruskal’s algorithm utilize cut property?
    - Yes, it uses it implicitly by preventing the creation of cycles and continuously adding edges in order.
- What is the difference between Prim’s and Kruskal’s?
    - The MST under construction in **Prim’s** is **connected**, whereas in **Kruskal’s it’s not**.
    - However, the final result is the same.
- Kruskal’s Conceptual demo.
    - Click [here](https://docs.google.com/presentation/d/1RhRSYs9Jbc335P24p7vR-6PLXZUl-1EmeDtqieL9ad8/edit#slide=id.g9a60b2f52_0_0).
- What is the proof that Kruskal’s algorithm is working?
    - Because it’s picking a cut, which is everything reachable along the MST from one of the vertices on the edge.
- Kruskal’s Implementation Demo.
    - As we mentioned earlier, we keep adding edges in ascending order, but we need to keep track of cycles.
    - That’s why we need to use WQU (WeightQuickUnion) data structure along with a PQ. The PQ will order the edges of the graph and store the edges **not** the vertices. **It’s worth to note** that you may not use PQ, you can just sort the edges in $O(E\log{E})$ time and you’ll eventually get the same runtime for the algorithm.
    - Before we add an edge to the MST, we check if the vertices are connected before, if not we union them and we add the edge to the MST.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2029.png)
        
    - After that, we go for 2-4 and check if they’re connected.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2030.png)
        
    - We keep going until we hit the edge 1-4.
        
        ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2031.png)
        
    - They’re already connected since they exist in the same set, so connecting them again means a cycle is created so we avoid that.
    - Check the remaining demo from [here](https://docs.google.com/presentation/d/1KpNiR7aLIEG9sm7HgX29nvf3yLD8_vdQEPa0ktQfuYc/edit#slide=id.g9a645a299_0_480).
- Kruskal’s Runtime.
    
    ![Untitled](24%20-%20Minimum%20Spanning%20Tree%20136110b48f3a490dbee8dca150811fe3/Untitled%2032.png)