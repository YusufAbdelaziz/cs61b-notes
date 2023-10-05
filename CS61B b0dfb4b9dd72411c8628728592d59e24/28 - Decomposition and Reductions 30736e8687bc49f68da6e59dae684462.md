# 28 - Decomposition and Reductions.

### Recalls, Goal of the lecture, What’s the point of all this, Graph problems so far.

- Recalls.
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled.png)
    
- Why should we know about Algorithms and Data structure? why do companies want to pursue people who have it?
    - Fast computers means fast programs, less angry users.
    - Using these data structures is a great way to get efficient code (and a great way to write code efficiently).
        - It’s good to know they work.
        - Relatively easy to use and implement.
    - Critical thinking -- I think I’m molding your brain in some subtle way that is very important.
        - We’re learning to manage complexity, letting us build more significant things.
    - The specific material in this class is what people expect you to know when interviewing.
- Goal of the lecture
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%201.png)
    
- Graph Problems so far.
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%202.png)
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%203.png)
    
    - To check the demos, see the slide number 6 [here](https://docs.google.com/presentation/d/1yrFdtRFZB9rHTNOMwB1qt9PjWK7hDhbhF3F2KXZU9hM/edit#slide=id.g55ec4e88d2_1_1293).

### Topological Sorting.

- What is the definition of Topological Sorting? Do all graphs can be topologically sorted?
    - Topological sorting is an ordering of a **DAG**'s vertices such that for every directed edge ***u***→***v***, ***u*** comes before ***v*** in the ordering.
    - Note that it doesn’t work on cyclic graphs since topological sort is not defined. The same goes with undirected graphs, since each edge in such graphs will create a cycle.
    - So **topological sorting only works on DAGs**.
- What is graph linearization?
    - It’s redrawing the graph so that the vertices are all in one line.
    - Topological sort is sometimes called **linearization** of the graph.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%204.png)
        
- How to find the topological ordering of a set of nodes (aka tasks that depends on one another)?
    - Given this graph:
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%205.png)
        
    - To find the topological ordering, you can do the following:
        - Perform a DFS traversal from every vertex with indegree 0 (has no in-arrows), NOT clearing markings in between traversals.
        - Record DFS postorder in a list.
        - Topological ordering is given by the reverse of that list (reverse postorder).
    - In other words, start any node that doesn’t have arrows in. Do all those first. Then every time you do one of those, you somehow check off that that arrow is already done. Now have a new set of nodes that has all requirements met, do those next.
- The steps of the topological sorting with an example (**How it works**).
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%206.png)
    
    - **Start with a node with indegree 0**, ****keep going until you finish the node you’re exploring.
    - **Remember** that we’re doing a post-order DFS, so after you finish exploring a path, you start going in reverse and add it to the list.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%207.png)
        
        - Note that we didn’t add 0 and add 3 because we weren’t done yet with exploring all nodes related to 0.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%208.png)
        
        - We select the next indegree 0 node which is 2 and keep doing the same
    - Reversing the post-order DFS list will result in the topological ordering.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%209.png)
        
- What is the point of telling you this algorithm?
    - The point is giving you a different flavor like an idea that you probably never had.
    - So using some algorithms we already explore (like DFS in this case) will probably help you in discovering solutions for other problems.
    - Another example would be to find all of connected components of a graph.
    - An algorithm to solve this problem is called Kosaraju’s Algorithm. Check [this](https://www.youtube.com/watch?app=desktop&v=RpgcYiky7uw&ab_channel=TusharRoy-CodingMadeSimple) video to get some insights about how it works.
- Why is it called Topological Sorting?
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2010.png)
    
    - The reason it’s called topological sort: Can think of this process as sorting our nodes so they appear in an order consistent with edges, e.g. [2, 5, 6, 0, 3, 1, 4, 7].
    - When nodes are sorted in diagram, arrows all point rightwards.
- Why should you be aware when people tells you “run DFS”?
    - Because you have to clarify whether you should do a restart or not.
    - In DepthFirstSearchPaths where we try to find the paths from a source vertex to all other vertices, we didn’t perform restart.
    - On the other hand, for Topological sort, we did a restart for every vertex with indegree 0.
- Try to find the topological ordering of this graph.
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2011.png)
    
    - Answer
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2012.png)
        
- Does Topological Sorting works on all graphs?
    - No, it only works on DAGs (Directed Acyclic Graphs).
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2013.png)
        
- The **runtime** of topological sorting.
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2014.png)
    
    - It’s basically the running time of DFS.

### Shortest Paths on DAGs.

- Shortest Paths Warmup: Dijkstra’s Algorithm.
    - Try to solve this:
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2015.png)
        
        - Answer.
            - Remember that we add all nodes to priority queue with their priority set to Infinity. Remember that it’s a min-heap so smaller numbers are near the root and big numbers are farther.
                
                ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2016.png)
                
            - How would to tell the order will Dijkstra’s algorithm visit the vertices given only the graph with magenta numbers (cost to reach the node)?
                - You can tell by the order of the cost, so Dijkstra’s will clearly visit node 1 before 3 because of the costs and so on.
- What the problem with Dijkstra’s?
    - Dijkstra’s is a greedy algorithm which means it tries to move towards the minimum cost without taking in consideration the implications of moving to a minimum cost node. This raises an issue with Dijkstra’s when weights are negative.
    - Consider this example, when we reach node 2, Dijkstra’s will do a correct relaxation (which is basically updating the accumulative cost to reach the node) to node 4 BUT it won’t continue with relaxing node 5 because it’s already popped out of the PQ.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2017.png)
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2018.png)
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2019.png)
        
    - So here, 5’s new cost should be -12 instead of 3.
- How to fix Dijkstra’s issue? (DAG **shortest path algorithm steps**).
    - We can fix it using an algorithm we just discussed earlier.
    - Try to think of this:
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2020.png)
        
        - Answer
            
            ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2021.png)
            
            ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2022.png)
            
            ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2023.png)
            
    - We can visit vertices in topological order. Considering the previous question, the answer was as follows:
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2024.png)
        
    - We have done the first step which is to **find the topological order**.
    - Second thing is **visit the vertices in topological order** and relax edges as we go.
    - So we’ll add the topological order in the fringe, and we start visit the vertices.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2025.png)
        
    - We start with 0, we relax node 3 and 1 and set the distance for both to 1
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2026.png)
        
    - We keep going until we finish with relaxing all edges. But let’s take a look at node 2, this is where Dijkstra’s failed.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2027.png)
        
    - Trick here is that visiting 4 will result for an edge relaxation for 5’s edge.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2028.png)
        
    - Full demo is [here](https://docs.google.com/presentation/d/1CfnLS3FSXV8X2sXPTravZGXeBUUkcFQv7Uf2iGWGUfs/edit#slide=id.g55eae46bdb_0_456).
- [Runtime](https://docs.google.com/presentation/d/1CfnLS3FSXV8X2sXPTravZGXeBUUkcFQv7Uf2iGWGUfs/edit#slide=id.g55eae46bdb_0_456) of DAG shortest paths algorithm (DAGSP).
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2029.png)
    
- What should we do with negative weights for non-DAGs?
    - You can use an extension algorithm for Dijkstra’s called [Bellman-Ford.](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm) It’s a more general algorithm and it’s running time is $O(|V|\space.\space |E|)$. It’s slower but more general to solve all kind of graphs.

### Longest Paths.

- The Longest Paths Problem.
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2030.png)
    
    - One of the most important unsolved problems in mathematics. Note that this is a **cyclic graph**, check nodes 4, 6, and 3 which could result in a cycle.
    - The best known algorithm is exponential which sucks.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2031.png)
        
- The Longest Paths Problem on DAGs (**steps**).
    - Try solve this:
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2032.png)
        
        - Answer
            
            ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2033.png)
            
    - What I had in mind is that I can use Dijkstra’s but with a max-heap PQ instead of min-heap. However this will fail in case we have negative edges.
    - So a cleaner and more complete solution would be to create a new graph $G'$ that has the same graph but with edges being flipped.
    - After that, we can run DAGSP on $G'$ yielding result X.
    - Lastly, we flip signs of all values in `X.distTo`. Note that `X.edgeTo` is already correct.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2034.png)
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2035.png)
        

### Graph Problems Discussed Summary.

![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2036.png)

### Reductions and Decomposition.

- What is a reduction? (Informal Definition).
    - A reduction is the process of using algorithms as building blocks for other algorithms.
    - Algorithms by Dasgupta, Papadimitriou, and Vazirani defines a reduction informally as follows:
        - “If any subroutine for task Q can be used to solve P, we say P reduces to Q.”
    - Can also define the idea formally, but **way** beyond the scope of our class.
        - If you’re curious, you can read more about [Karp and Cook reductions](https://en.wikipedia.org/wiki/Polynomial-time_reduction).
- Example on reductions.
    - Consider the LPT problem on DAGs. We can call the algorithm to find LPT as DAG-LPT. DAG-LPT uses DAG-SPT behind the scenes. Recall that we flip the signs of the weights, run DAG-SPT and flip `X.distTo` array.
        
        ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2037.png)
        
    - Since DAG-SPT can be used to solve DAG-LPT, we say that “DAG-LPT reduces to DAG-SPT”.
    - As a real-world analog, suppose we want to climb a hill. There are many ways to do this:
        - “Climbing a hill” reduces to “riding a ski lift.”
        - “Climbing a hill” reduces to “being shot out of a cannon.”
        - “Climbing a hill” reduces to “riding a bike up the hill.”
- Richer examples of a reduction.
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2038.png)
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2039.png)
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2040.png)
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2041.png)
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2042.png)
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2043.png)
    
    - Worth to note that the [book](https://cs61b-2.gitbook.io/cs61b-textbook/28.-reductions-and-decomposition/28.4-reductions-and-decomposition) clarifies the problem and explains it better.
- Reductions and Decompositions .
    
    ![Untitled](28%20-%20Decomposition%20and%20Reductions%2030736e8687bc49f68da6e59dae684462/Untitled%2044.png)
    

### Exercise from CS61 Textbook.

- Click [here](https://cs61b-2.gitbook.io/cs61b-textbook/28.-reductions-and-decomposition/28.5-exercises).