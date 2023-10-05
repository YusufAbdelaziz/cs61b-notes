# 23 - Shortest Paths

### Recalls.

- We discussed two methods to do the following:
    - Find a **path** from given vertex s to all reachable vertices in a graph.
    - Find a **************************shortest path************************** from a given vertex s to every reachable vertex in the graph.
- ******BFS****** finds you the shortest path whereas DFS doesn’t.
- **Both** can give you a **path from given vertex** to all reachable vertices but as mentioned, **BFS can** also get **shortest path**.
- There’s a difference in efficiency **space-wise**.
    - **DFS** performs poorly for **spindly graphs** (graphs with too many nodes so we’ll end up making
    - **BFS** performs poorly for **bushy graphs** (all nodes are all connected which means our queue gets used a lot).
- They both have the same running time which was $O(V +E)$ in case adj. list is used as a representation (revisit previous lectures if you don’t remember).
- It’s worth to note that we developed a shortest path algorithm that works well on graph with no edge labels (weights) which means it tries to find the shortest path with the fewest number of edges.
- That’s not always the correct definition of shortest. Graphs may have weights on their edges. For example, an edge A-B that has 5 as weight is considered to be farther than another edge A-C that has 4.

### What is the problem with BFS when finding a shortest path?

- As we mentioned, BFS returns path with shortest number of edges irrespective of the weight on the edges.
- So it won’t work properly with navigation applications like Google Maps to find a shortest path.
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled.png)
    

### Dijkstra’s Algorithm.

- Quick Challenge: Find the shortest path from source s to target vertex t.
    - Try to find the shortest path by eye:
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%201.png)
        
        - Answer
            - The path is 0 → 1 → 4 → 5. The total cost is 2 + 3 + 4 = 9.
                
                ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%202.png)
                
- Another one: Find shortest paths from source vertex s to every other vertex.
    - Try again but this time try to come up with a shortest path that visits all vertices.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%203.png)
        
        - Answer.
            - The **tree** resulted is a union of all shortest paths from the source vertex to every other vertex.
                
                ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%204.png)
                
- Now try to come up with the number of edges in the Shortest Paths Tree (SPT).
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%205.png)
    
    - Answer
        - **Shortest Paths Tree will always have V - 1 edges**
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%206.png)
        
- Dijkstra’s Algorithm: Best First Search Steps with Example
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%207.png)
    
    - When we visit a node, we try to select the next node to visit based on the best possible edge which has less weight.
    - So we start with node A that has 0 weights as it’s our starting node. **Note** that we update the distances for the neighbor nodes **before** we visit the next one.
    - We then traverse to C since its edge’s weight is 1 which is better than B’s edge.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%208.png)
        
    - Note that we haven’t visited B **yet**. Now we have got two options, D or B.
    - We **update** the cost of **D** and **B** **first** and then decide which one is the best to visit.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%209.png)
        
    - We now visit B since it’s the best option. After that, we update the cost to visit D which will be 4 instead of 6.
    - Lastly we end our algorithm by visiting the last node D and voila! we’re done.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2010.png)
        
        - **Shortest Paths Tree will always have V - 1 edges**
- Does Dijkstra’s Algorithm work with undirected or directed graphs?
    - It actually works with both.
    - [Here’s](https://stackoverflow.com/questions/38190592/is-dijkstras-algorithm-for-directed-or-undirected-graphs) why.
- Dijkstra’s Algorithm Steps And Example.
    - You’ll first need a Priority Queue (have a visit to it in case you don’t remember it).
    - Add the starting vertex *s* with priority 0 and add all other vertices with priority $\infin$.
    - While the priority queue is not empty: pop a vertex out of the priority queue, and **relax** all of the edges going out from the vertex.
    - Relaxation here means to check if there’s a potential update for better distance of a node. Note that we don’t relax edges that point to already visited nodes.
    - So let’s take an example to understand further what does that mean.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2011.png)
        
        - Initially, all vertices’ priority are $\infin$.
        - We start to check the neighbors of vertex *s* which are 1 and 2. We see if vertex 1’s edge weight which is 2 is less than $\infin$ which is true, we update the priority of the vertex in the PQ. The same goes with vertex 2. Recall that updating or removing requires $\log{N}$ time.
            
            ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2012.png)
            
        - Now we pop vertex 2 because it has higher priority and see its neighbors.
            
            ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2013.png)
            
        - Now we go to visit vertex 1 because it has higher priority than other vertices.
        - Note that we don’t perform a **relaxation** (which means to change the edgeTo) for vertex 2 because going from 1 will cost 5 + 2 = 7 > 1.
            
            ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2014.png)
            
        - We keep going and we then visit 4.
            
            ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2015.png)
            
        - Now we can relax the edge of 2 but the cost is 1 + 5 = 6 which is less than 1 so we avoid relaxation (updating).
        - After that, we can now update 6 and 5 because 5 + 4 = 9 < 16.
            
            ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2016.png)
            
        - We go to visit 5 but we can’t do much. We jump to 6.
        - From 6, I can relax the edge to 3 and make it 11 instead of 13.
            
            ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2017.png)
            
        - Now we’ll visit 3, and we also ignore updating 4 since 11 + 2 > 5.
            
            ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2018.png)
            
        - IMPORTANT note here: Any edge pointing to an already visited node (white), the relaxation will always fail.
        - Click [here](https://docs.google.com/presentation/d/1_bw2z1ggUkquPdhl7gwdVBoTaoJmaZdpkV6MoAgxlJc/pub?start=false&loop=false&delayms=3000&slide=id.g771336078_0_957) for the slides of the example.

### Dijkstra’s Correctness and Runtime.

- Pseudocode of the algorithm
    - The pseudocode of the algorithm is as follows:
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2019.png)
        
- Properties of the algorithm.
    - Some properties or invariants of Dijkstra’s algorithm:
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2020.png)
        
- Dijkstra’s Guaranteed Optimality.
    - Note that you don’t actually need PQ as you can use the best node to visit manually. However, PQ helps in ordering the best node to visit next.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2021.png)
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2022.png)
        
- Dijkstra’s Algorithm Runtime.
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2023.png)
    

### Negative Edges.

- The relaxation we made for already visited nodes will unfortunately fail.
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2024.png)
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2025.png)
    
- **Bellman-Ford** Algorithm can be used to tackle negative edges.

### A*

- Dijkstra’s Problem with Navigation Systems.
    - Consider a navigation application through which we try to find the best route from Denver to New York.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2026.png)
        
    - Will Dijkstra’s algorithm find the shortest path? Yes.
    - Will it be efficient? No.
    - The reason of that is it’ll explore every place within a range of miles of Denver before it locates NYC.
- How to do better?
    - We want to bias the algorithm eastwards since we have a single target to reach.
- Introducing A*
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2027.png)
    
- A* Example.
    - The main idea of A* is that we have a heuristic function that helps or guides us towards our goal so we take it into consideration when we guess which node to visit next.
    - The heuristics here are coming from the edges (the smallest one like in node 1) moving from a node.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2028.png)
        
        - Click [here](https://docs.google.com/presentation/d/177bRUTdCa60fjExdr9eO04NHm0MRfPtCzvEup1iMccM/edit#slide=id.g771336078_0_180) to see the demo.
- What are heuristics in A*?
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2029.png)
    
    - We can use latitude and longitude and compute a line from a node to the goal.
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2030.png)
    
- So what’s the difference?
    - With the help of heuristics, we can find the direct path to the goal without exploring other nodes that we don’t have interest with.
    - Check [this](http://qiao.github.io/PathFinding.js/visual/) cool site for illustration.
    
- What happens if we have bad heuristics (i.e. constant ones)?
    - We’ll end up with using Dijkstra’s algorithm.
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2031.png)
        
        ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2032.png)
        

### Summary: Shortest Paths Problems.

- If you want to get from one place to another: Dijkstra’s is your way but A* is more efficient.
- If you want to know the paths for all other nodes: Use Dijkstra’s.
    
    ![Untitled](23%20-%20Shortest%20Paths%200e2c420c02344cd9913ee4adaaed7a5d/Untitled%2033.png)