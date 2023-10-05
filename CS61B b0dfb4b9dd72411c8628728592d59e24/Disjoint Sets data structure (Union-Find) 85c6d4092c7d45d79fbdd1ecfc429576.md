# Disjoint Sets data structure (Union-Find).

- Two sets are named disjoint sets if they have no elements in common. So the data structure keeps track of fixed number of elements partitioned into a number of disjoint sets.
- It mainly has two operations.
    1. connect(x, y) → which connect element x to y (also called union).
    2. isConnected(x, y) → which checks if element x and y are connected (both are in the same set).
- Useful in some algorithm implementation like Kruskal’s algorithm.
- Design process steps and implementation of each step.
    - Intuitive Idea (List of Sets).
        - First intuitive idea would be using a list of sets. That said, this idea really slow and the code would be also complex.
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled.png)
            
        - We also notice that we constructing our list of sets, it’d take $\theta(N)$ (remember that theta is tightly bound asymptotic which means exact equality.).
        - Also the connect and isConnected operations are $O(N)$ because you might achieve better results than $N$ (remember that big-O is “less than or equal”, so you might achieve N or anything less that it like $\log{n}$.).
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%201.png)
            
            Note that creating N distinct sets initially takes $\theta(N)$
            
    - Quick Find - Traditional approach but not good yet (A single list).
        - We move on to use list of integers. Each entry in the list represents the number of the set that item belongs to.
        - So basically, the index is the item itself, and the value at that index is the id of the set it belongs to.
        - So the list here serves as a map to map each index (the item) to its set id (the value of that index).
        - Notice that the values are set to the newly added set. So we added set number 5 to set number 4, and they all have now number 5 as a single set.
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%202.png)
            
        - Quick implementation →
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%203.png)
            
        - How much does this approach cost ?
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%204.png)
            
            It’s still too slow to use, so we should find a better approach.
            
    - Quick Union
        - Previously, our data structure was great at checking if two elements belong to the same set (isConnected), but connect two elements from distinct sets was pretty slow.
        - We’ll build upon the previous approach, but we’d choose the values of the array so that connect is fast.
        - Important Question !
            - Hard question: How could we change our set representation so that combining two sets into their union requires changing **one** value?
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%205.png)
                
            - Answer
                - Assign each item a parent instead of an id. So this results in a tree-like shape.
                - You may use -1 as a value to indicate that an item doesn’t have a parent, or you might use the index as a value (this convention is used in the book).
                    
                    ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%206.png)
                    
            - Another question about connecting between two numbers.
                - Based on the previous answer, we want to connect item 5 and item 2, how can we achieve that ?
                    
                    ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%207.png)
                    
                - Answer.
                    - We need to find the root of the item 5 and the root of item 2, and we assign the root of 5 to the value of root of 2.
                        
                        ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%208.png)
                        
                    - Another correct but not optimal approach is to add the parent 3 as a child of 2 but this would make a taller tree.
        - A problem arises from the previous questions, that finding the root of an item might be quite expensive.
        - An example.
            - Consider this example, and notice that you need to call root(x) method to find the root of an element.
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%209.png)
                
            - Answer
                - Both would be $\theta(N)$.
                - Since your tree is long, this would take time in checking whether two items are connected. For example, you might need to check if 4 is connected to 3, so you go up until you find 0 as the root of both, so you spent N operations for item 4 (N = 5) and N - 1 for item 3 which is ( N = 4).
                - For connect method, this would also take time to check the root of the long tree.
        - Quick union implementation →
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2010.png)
            
        - Unfortunately, with this implementation, **find** worst case runtime takes $\theta(N)$ which results that isConnected and connect method both have the same runtime (both of them use **find** internally).
        - Additionally, things get worse when the tree becomes too tall which results in worse performance than QuickFind.
        - So in worst case, we’ve done worse than QuickFind, but our connect operation might be really fast in case of making the tree balanced regularly, so we’re on the right track.
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2011.png)
            
    - Weighted Quick Union.
        - To improve **connect** method, we have to make our tree shorter so that **find** method takes less time.
        - So whenever we call **connect**, we always link the root of the ***smaller tree*** to the ***larger tree***. This will result in trees of a maximum height of $\log{N}$, where N is the number of elements in our disjoint sets. Notice that we said smaller not shorter, we are concerned with the weight of each tree not their height and we’ll get to that when we talk about the implementation.
        - Important Question !
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2012.png)
            
            - Answer
                - The above one is better, recall that we want to minimize the height of the tree so that ***find*** doesn’t take much time.
                
        - Implementation Details.
            - We’d use the same ***parent*** array and ***isConnected*** method as before, but we must change the ***connect*** method.
            - First approach to keep tracking of the weight of each tree, is to use values other than -1 in parent array for root nodes to track size. So for example the index 0 must have - 5 as its value.
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2013.png)
                
            - So whenever you connect, you can compare the number of elements each root has connection to.
            - The second approach is to use a ***size*** array which store the size of each node, but it’s more expensive space-wise.
            
        - Performance Analysis and summary.
            - Let’s start with two. You could possibly have at least a tree of height 1.
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2014.png)
                
            - So what if I want to add two more elements ? You’d get a tree of height 2
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2015.png)
                
            - How about connecting four elements to the root element ? You’d get a tree of height 3.
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2016.png)
                
            - So you might now notice that the worst tree height is $\Theta(\log{N})$.
            - Eventually we now have our ***connect*** and ***isConnected*** methods much faster with a runtime as $O(\log{N})$, but why ? Because find method would need a runtime of $O(\log{N})$ to climb a tree of height $\log{N}$.
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2017.png)
                
        - Why weighted quick union not heighted quick union?
            - Keep tracking of each height is much more complex than the weight. In addition to that, the weighted quick union is asymptotically the same as heighted quick union ($\Theta{(\log{N})}$.
            - So we’re just making our code complex with no performance gain.
            
    - Weighted Quick Union with Path Compression
        - In our last way of improving the performance, we can make our tree more flat while checking of two elements are in the same set using **isConnected** method.
        - How’s that possible ? Well, we can check the ancestor of each element and link these elements directly to the root. So we’re trying here to check if both 10 and 15 are connected. Whenever you’re climbing to find the root, change the value of each descendant so they’re all connected to the root directly.
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2018.png)
            
        - So since 15, 11, 5, and 1 are connected to 0, we can make the first three elements to be connected directly to 0. The same goes with 10 on the other side.
            
            ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2019.png)
            
        - Eventually, this leads to a union/connected operations that are very very close to amortized constant time $\alpha(1)$ (amortized means average).
        - So if you have M operations and N inputs, you’d achieve $O(N + M\alpha(N))$. Note that the N at the beginning is for initializing the disjoint sets.
        - You said constant, but what is $\alpha(N)$ ? This is called the inverse Ackermann function, which grows really slowly, so you can consider it as a constant
        - Ackermann Function.
            - M operations on N nodes is $O(N + M\space log^*{N})$.
            - Without digging into too much details, $log^* {N}$ is the iterated log, it’s the number of times you need to apply log to n to go below 1.
                
                ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2020.png)
                
            - So notice that for any realistic input, $log^*$ is less than 5.
    - Performance Summary.
        
        ![Untitled](Disjoint%20Sets%20data%20structure%20(Union-Find)%2085c6d4092c7d45d79fbdd1ecfc429576/Untitled%2021.png)