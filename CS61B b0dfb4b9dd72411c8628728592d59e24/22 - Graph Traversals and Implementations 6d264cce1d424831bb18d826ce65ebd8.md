# 22 - Graph Traversals and Implementations

- Review on Tree and Graph Traversals.
    
    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled.png)
    
    Note that the difference between tree and graph traversals is that **tree traversals** are **unique** but **graphs aren’t**.
    
- BFS - Overview.
    - BFS - Algorithm
        - Try to come up with the algorithm to implement BFS.
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%201.png)
            
            - Answer
                
                ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%202.png)
                
                - Checkout [this](https://docs.google.com/presentation/d/1JoYCelH4YE6IkSMq_LfTJMzJ00WxDj7rEa49gYmAtc4/edit#slide=id.g76e0dad85_2_380) demo.
                - A *fringe* is just a term we use for the data structure we are using to store the nodes on the frontier of our traversal's discovery process (the next nodes it is waiting to look at). For BFS, we use a queue for our fringe.
        - Note that we’re sure that BFS works because every item in the queue is always at distance k or k +1 for some k. k changes every time you proceed with an element. This means that first item in the queue is closer to explore to the source node than other items in the queue. Check the answer of the previous example and see the demo.
    - One use of BFS.
        - A cool problem to use BFS with is called Six-Degree of Kevin Bacon.
        - The challenge is to link any actor in Hollywood to Kevin Bacon using at most 6 intermediate actors.
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%203.png)
            
        - There’s actually a [website](https://oracleofbacon.org/) that shows how Kevin Bacon is linked to almost every actor in Hollywood.
- DFS - Overview.
    - DFS - Algorithm
        - So to implement DFS, what kind of data structure should we use ?
            - Answer.
                - Our fringe would be a **stack**.
        - It’s worth to note that DFS and BFS differ not just in the used data structure, but also the order of marking the nodes (aka visiting the nodes).
        - For DFS we mark nodes only once we visit a node - aka pop it from the fringe. As a result, it's possible to have multiple instances of the same node on the stack at a time if that node has been queued but not visited yet. With BFS we mark nodes as soon as we add them to the fringe so this is not possible.
        - Recursive DFS implements this naturally via the recursive stack frames; iterative DFS implements it manually:
            
            ```java
            Initialize the fringe, an empty stack
                push the starting vertex on the fringe
                while fringe is not empty:
                    pop a vertex off the fringe
                    if vertex is not marked:
                        mark the vertex
                        visit vertex
                        for each neighbor of vertex:
                            if neighbor not marked:
                                push neighbor to fringe
            ```
            
- Graph API.
    
    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%204.png)
    
    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%205.png)
    
    - So this is an API decision which is followed in our optional book. We’ll use integer nodes instead of labels and we can use a map to store the relation between labels and integers.
    
    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%206.png)
    
    - We may want to add a method that returns the number of degrees for a node (aka # of edges outgoing from a node).
    
    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%207.png)
    
    - Now try to come up with a print method that prints a given graph.
        
        ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%208.png)
        
        - Answer
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%209.png)
            
- Graph Representations.
    - Adjacency Matrix.
        - It’s a matrix that describes whether two nodes are connected via an edge (connection is set to 1) or not (connection value set to 0).
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2010.png)
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2011.png)
            
        - This is a simple approach that it’ll easily work. However, notice that you may iterate over nodes that you don’t have any edges with. So say you’ve got 1000 nodes and a node is only connected to two other nodes, that would be tedious to loop over 997 nodes (in case the node is not connected to itself) that you’re not even connected with.
        - Quiz time !
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2012.png)
            
            - Answer.
                
                ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2013.png)
                
    - Edge Sets.
        
        ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2014.png)
        
    - Adjacency lists.
        - Most popular approach to represent graphs.
        - You’d need a hash table, each key would be the node and the value would the list of nodes that are connected to that node.
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2015.png)
            
        - In case you want to have undirected graph, you can just go to node 2 and store [0, 1] as values (you’d need to do the same with 1 too).
        - Quiz time !
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2016.png)
            
            - Answer (Tricky).
                
                ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2017.png)
                
                Well, that was none of the answers so try again 😀.
                
                - Real answer
                    
                    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2018.png)
                    
                
        - An explanation →
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2019.png)
            
    - Runtime comparison between implementations.
        
        ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2020.png)
        
- DFS and BFS Implementations.
    - DFS.
        - This implementation approach is used by Princeton’s library in which you decouple the graph from the algorithm. In other words, you have a separate graph object that just holds that graph and does no searching, and an implementation object called **********Paths,********** that takes graphs, which can implement the searching algorithm using either BFS or DFS.
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2021.png)
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2022.png)
            
        - Now go and review DepthFirstPaths [demo](https://docs.google.com/presentation/d/1lTo8LZUGi3XQ1VlOmBUF9KkJTW_JWsw_DOPq8VBiI3A/edit#slide=id.g76e536eb1_0_294). Afterwards, we’ll continue discussing about implementation details and runtime.
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2023.png)
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2024.png)
            
        - Now here comes the challenge, try to come up with an O bound for the runtime for DepthFirstPaths constructor.
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2025.png)
            
            - Answer (Tricky question).
                
                ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2026.png)
                
        - Now building upon the previous question, here’s a harder one :
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2027.png)
            
            - Answer
                
                ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2028.png)
                
        - Lastly, the runtime is →
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2029.png)
            
    - BFS.
        
        ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2030.png)
        
        - This is what BFS implementation would look like. Notice that we used a queue, whereas in DFS we used stack (we were depending upon call stack to be precise).
    - Which algorithm should we consider when facing a problem (possible path to every vertex, shortest path) ?
        
        ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2031.png)
        
    - How does selecting a different graph representation may worsen the running time of the algorithms?
        - So consider using adjacency matrix instead of adjacency list, what will be the runtime ?
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2032.png)
            
            - Answer
                
                ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2033.png)
                
        - Since it’ll be quadratic time, hence the overall runtime for each algorithm →
            
            ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2034.png)
            
- Summary
    
    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2035.png)
    
    ![Untitled](22%20-%20Graph%20Traversals%20and%20Implementations%206d264cce1d424831bb18d826ce65ebd8/Untitled%2036.png)