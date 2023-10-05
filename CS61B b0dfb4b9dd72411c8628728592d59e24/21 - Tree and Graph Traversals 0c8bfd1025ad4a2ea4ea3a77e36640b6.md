# 21 - Tree and Graph Traversals

### Intro to Graphs

- Tree Definition (Recap slides).
    
    ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled.png)
    
    ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%201.png)
    
    ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%202.png)
    

### Tree Traversal.

- Consider a case that you want to calculate the total size of all files inside a folder called 61b.
    
    ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%203.png)
    
- You might at first call this “**tree iteration**” but “**iteration**” word usually means scanning through a linear data structure like a list.
- So “**traversal**” is a more proper word and there’re different ways or “**orders**” in which we might visit nodes.
- Each order is useful in different cases.

### Tree Traversal Orderings.

- Level Order.
    - Visit top-to-bottom, left-to-right.
        
        ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%204.png)
        
        - Ordering will be : ****************DBFACEG.****************
- Depth First Traversals (Preorder, Inorder, Postorder).
    - There’s a difference between visiting a node and traversing a node.
    - Visiting a node means accessing or examining its data, while traversing a node means moving from one node to another according to a specific algorithm or path.
    - Preorder.
        
        ```java
        public void preOrder(BSTNode x){
        	if(x == null) return;
        	System.out.println(x.key);
        	preOrder(x.left);
        	preOrder(x.right);
        }
        
        ```
        
        - The general idea is to visit a node (print the key of the node), and then traversing its children from left to right recursively.
        
        ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%204.png)
        
        - Traversing is D B A C F E G.
    - Inorder.
        
        ```java
        public void preOrder(BSTNode x){
        	if(x == null) return;
        	preOrder(x.left);
        	System.out.println(x.key);
        	preOrder(x.right);
        }
        
        ```
        
        - Traverse left child, visit, and then traverse right child.
            
            ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%204.png)
            
        - Traversal is : A B C D E F G.
    - Postorder
        
        ```java
        public void preOrder(BSTNode x){
        	if(x == null) return;
        	preOrder(x.left);
        	preOrder(x.right);
        	System.out.println(x.key);
        }
        
        ```
        
        - Traverse left, traverse right, then visit.
        
        ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%204.png)
        
        - Traversing Order : A C B E G F D.
    - Useful trick to memorize these types of traversals.
        
        ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%205.png)
        
- What good are all these traversals ?
    - Preorder traversal seems convenient when printing directory listing:
        
        ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%206.png)
        
    - Whereas Postorder traversal is pretty handy for gathering file sizes.
        
        ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%207.png)
        

### Graphs

- Intro - What is a graph, can we consider a tree as a special kind of a graph ?
    - A graph consists of :
        - A set of nodes (aka vertices).
        - A set of edges connected these nodes.
    - So a tree is a special kind of graph (cyclic references were not allowed and there’s only one path between any two nodes).
        
        ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%208.png)
        
- Simple Graph.
    - A simple graph is a graph with :
        - No edges that connect a vertex to itself, i.e. no “loops”
        - No two edges that connect the same vertices, i.e. no “parallel edges”.
            
            ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%209.png)
            
    - In this course, a graph denotes to a simple graph unless otherwise explicitly stated.
- Graph Types.
    
    ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%2010.png)
    
- Graph Terminology.
    
    ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%2011.png)
    
- Graph problems.
    - Some well known graph problems and their common names :
        - **s-t Path** : Is there a path between vertices **s** and **t**?
        - **Connectivity** : Is the graph connected i.e. is there a path between all vertices?
        - **Biconnectivity** : Is there a vertex whose removal disconnects the graph?
        - **Shortest s-t Path** : What is the shortest path between vertices s and t?
        - **Cycle Detection :** Does the graph contain any cycles?
        - **Euler Tour** : Is there a cycle that uses every edge exactly once?
        - **Hamiltonian Tour :** Is there a cycle that uses every vertex exactly once?
        - **Planarity :** Can you draw the graph on paper with no crossing edges?
        - **Isomorphism :** Are two graphs isomorphic (the same graph in disguise)?
- How tricky graph problems can be?
    - We have two famous examples.
    - The first is **[Euler Tour](https://en.wikipedia.org/wiki/Euler_tour_technique)** which was solved back in 1873 and the solution runs in O(E) where E is the number of edges in the graph.
    - The second one is **[Hamiltonian Tour](https://en.wikipedia.org/wiki/Hamiltonian_path_problem#:~:text=The%20Hamiltonian%20cycle%20problem%20is,if%20there%20is%20no%20Hamiltonian)** which has been not solved efficiently yet. The best known algorithm for this problem runs in exponential times.
- Graph Traversals.
    - DFS - Depth First Search.
        - DFS is based on the idea of exploring a neighbor’s entire subgraph before moving on to the next neighbor. So you **go as deep as possible**.
            
            ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%2012.png)
            
        - So we first visit the starter node and we should have an array to store visited nodes (marking them). We keep traversing throughout the graph until we’ve reached the destination node.
        - We can also have a HashTable or an array to store a route from the start to the destination nodes.
        - Check for [this](https://docs.google.com/presentation/d/1lTo8LZUGi3XQ1VlOmBUF9KkJTW_JWsw_DOPq8VBiI3A/edit#slide=id.g76e0dad85_2_380) illustration for a DFS and all possible routes from each node to the other.
    - DFS Preorder and DFS Postorder.
        - The DFS we used was called **DFS Preorder**. It’s called so as we make actions (marking a node as visited) before traversing to other neighbor nodes, quite similar to preorder traversal of trees. Notice that there might be multiple DFS preorder depending on the fork node you select. So in case of node 1, you might go to 4 or 2 so that would vary the DFS preorder traversal.
            
            ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%2013.png)
            
        - We might also have DFS Postorder which means that **action** happens **after** DFS calls to neighbors. So in this case, we finish traversing and then print the node, that’s why you might think that the prints are flipped.
            
            ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%2014.png)
            
    - BFS - Breadth First Search.
        - This kind of traversal is analogous to “level order” in tree traversal.
        - The search is **wide** unlike deep DFS.
            
            ![Untitled](21%20-%20Tree%20and%20Graph%20Traversals%200c8bfd1025ad4a2ea4ea3a77e36640b6/Untitled%2015.png)
            
            So the traversal would be (starting from 0) 0 1 24 53 68 7.