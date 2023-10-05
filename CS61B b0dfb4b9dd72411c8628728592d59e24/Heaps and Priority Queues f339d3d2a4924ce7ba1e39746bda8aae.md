# Heaps and Priority Queues.

- Priority Queues
    - Intro
        - Previously with BSTs, we cared about searching operations for all items in the tree.
        - But what if we only care about tracking or quickly finding the smallest or largest element instead of quickly searching ? Therefore we’ll introduce the priority queue ADT.
        - PQ has two types :
            - Min PQ.
            - Max PQ.
        - Apparently, the first type keeps tracking for the smallest item in the tree and the second keeps tracking of the largest item in the tree.
        - An example of Min PQ ADT is shown here :
            
            ```java
            /** (Min) Priority Queue: Allowing tracking and removal of 
              * the smallest item in a priority queue. */
            public interface MinPQ<Item> {
                /** Adds the item to the priority queue. */
                public void add(Item x);
                /** Returns the smallest item in the priority queue. */
                public Item getSmallest();
                /** Removes the smallest item from the priority queue. */
                public Item removeSmallest();
                /** Returns the size of the priority queue. */
                public int size();
            }
            ```
            
    - MinPQ - Implementation
        
        ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled.png)
        
        - For BST implementation, recall that you don’t have the ability to add any duplicate items. It’s worth to note that BST is the most efficient data structure to implement PQ ADT.
        - We still need to find a better data structure to implement PQ.
- Heaps
    - Intro.
        - BSTs has the most efficient data structure to implement a PQ but we have two concerns :
            - Keeping the BST bushy.
            - Store duplicates inside the BST.
        - So we have to make some modifications to a BST.
        - We’ll introduce the concept of **binary min-heap**, which is basically **a binary tree that is complete and obeys min-heap property**.
        - **Min-Heap** property states **that every node is less than or equal to both of its children.**
        - A binary tree is called **complete** when t**he missing items only are at the bottom level and all nodes are as far left as possible.**
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%201.png)
            
        - The previous figure demonstrates what a min-heap looks like. Let’s see if you’ve understood that well with this question :
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%202.png)
            
            - Answer
                
                ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%203.png)
                
                - Notice that the second one is incomplete since it’s missing items at the top level not only at the bottom level.
                - While the last one is missing items at the bottom level and all nodes are at to the left.
        - Heaps are created for the sake of creating a priority queue.
    - PQ API implementation with Heaps.
        - Now let’s try to implement the priority queue API using heaps. First thing we want to implement is the `add(x)` method. Try to come up with an algorithm to inserting items to the heap.
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%204.png)
            
            - Solution
                - You can find an animated solution [here](https://docs.google.com/presentation/d/1VEd2Pm_3OuvkC1M8T5XAhsBTQFxVHs386L79hktkDRg/pub?start=false&loop=false&delayms=3000&slide=id.g11ecaeaf56_0_0).
                - The bottom line is to add the new node to the bottom (from left bottom to right bottom) and keep moving upwards to keep min-heap property valid.
        - How about removing the smallest element ?
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%205.png)
            
            - Answer
                - You’d have to switch the recently added node(node number 6) with the root node (node number 1).
                - Sink your way down to the hierarchy, yielding to the most qualified node.
                - Check [here](https://docs.google.com/presentation/d/1VEd2Pm_3OuvkC1M8T5XAhsBTQFxVHs386L79hktkDRg/pub?start=false&loop=false&delayms=3000&slide=id.g11ecaeaf56_0_374) to see animated version of removing a node.
                    
                    ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%206.png)
                    
        - So as a summary of PQ implementation (using Heaps) :
            - `add`: Add to the end of heap temporarily. Swim up the hierarchy to the proper place.
                - Swimming involves swapping nodes if child < parent
            - `getSmallest`: Return the root of the heap (This is guaranteed to be the minimum by our *min-heap* property
            - `removeSmallest`: Swap the last item in the heap into the root. Sink down the hierarchy to the proper place.
                - Sinking involves swapping nodes if parent > child. Swap with the smallest (**largest child in case of max-heap**) child to preserve *min-heap* property.
        - So how can we implement PQ in Java ? This is what we’ll try to figure out in the next section.
- Tree Representations is Java - Approaches to represent trees.
    - Approach 1a, 1b, and 1c: Create mapping from node to children.
        - `Tree1A` is the first approach which supposes that we have a fixed-width nodes.
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%207.png)
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%208.png)
            
            ```java
            public class Tree1A<Key> {
            	Key k;
            	Tree1A left;
              Tree1A middle;
              Tree1A right;
            	....
            }
            ```
            
        - `Tree1B` is the second approach which supposes that we have an array of links to children nodes. This approach has an advantage that allows us to store a dynamic number of children, but the downside is that the performance becomes worse as we have to follow to links to get to a child instead of one. Also the code will be awkward.
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%209.png)
            
            ```java
            public class Tree1B<Key> {
            	Key k;
            	Tree1B[] children;
            	....
            }
            ```
            
        
        - `Tree1C` supposes that we each node has a reference to its child ********and******** a reference for its sibling.
            
            ```java
            public class Tree1C<Key> {
              Key k;
              Tree1C favoredChild;
              Tree1C sibling;
              ...
            }
            ```
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2010.png)
            
    - Approach 2: Store keys in an array and store parent IDs in an array.
        - This is similar to what we did with disjoint sets.
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2011.png)
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2012.png)
            
            ```java
            public class Tree2<Key> {
            	Key[] keys;
            	int[] parents;
            	...
            }
            ```
            
        - So in this approach, we have a keys array that stores the `values` (keys) of each node, and a `parents` array which stores the relationship between a node and another.
        - So looking at the bigger tree, we see that node m(index 11) has a parent which is 5, and looking at index 5 (node p), we see that the parent is 2 (node v) and lastly the parent of index 2 is 0 which is the root (k).
        
    - Approach 3: Store keys in an array. Don’t store structure anywhere (valid only with complete trees).
        - We saw before that the `keys` array stores each level of the nodes, so we can not only store the keys but also know the structure as well.
        - This simply assumes that the tree is complete and therefore only works with complete trees.
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2013.png)
            
            ```java
            public class Tree3<Key> {
               Key[] keys;
               ...
            }
            ```
            
        - Challenge time !
            
            ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2014.png)
            
            - The `swim` method should take an index K that makes a node swim (moves upward). So for example, if we pass 5 as an argument (node P), we’d need to find the parent of node P using `parent` method and swap both node V with node P and keep doing the same until node P goes upwards.
            - Now try to come up with an implementation to `parent` method.
                - Answer
                    
                    ```java
                    public int parent(int k) {
                    	if(k < 1) return 0;
                    	return (k - 1) / 2; // Java round numbers down 2.5 -> 2.
                    }
                    ```
                    
                
    - Summary
        
        ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2015.png)
        
- PQ Implementation Considerations.
    - The book’s implementation is similar to the third approach but leaving 0 as an empty spot.
        
        ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2016.png)
        
        ![Untitled](Heaps%20and%20Priority%20Queues%20f339d3d2a4924ce7ba1e39746bda8aae/Untitled%2017.png)
        
    - Some implementation questions :
    1. How will the PQ know how to order the items? Say we had a PQ of dogs, would it order by weight or breed?
        - Answer
            - You can do so by only accepting objects that implements `Comparable` interface.
                
                ```java
                /** (Min) Priority Queue: Allowing tracking and removal of the
                  * smallest item in a priority queue. */
                public interface MinPQ<Item implements Comparable<Item>> {
                	/** Adds the item to the priority queue. */
                	public void add(Item x);
                	/** Returns the smallest item in the priority queue. */
                	public Item getSmallest();
                	/** Removes the smallest item from the priority queue. */
                	public Item removeSmallest();
                	/** Returns the size of the priority queue. */
                	public int size();
                }
                ```
                
    2. Is there a way to allow for a flexibility of orderings?
        - Answer
            - You can do so by accepting an object (`ItemsComparator`) which implements `Comparator` interface (this interface compares two objects, whereas the previous one `Comparable` compares an object with another one).
            - In this way, you can have both max heap and min heap with a single class.
                
                ```java
                public interface PQ<E> {
                	private Comparator<E> comparator;
                	PQ(Comparator<E> comparator){
                		this.comparator = comparator;
                	}
                
                	/* Returns the root element without removing it
                 (whether it's the smallest or the largest depending on the comparator)*/
                	public E peak();
                
                	public void add();
                	/* Returns the root node as well as removing it */
                	public E poll();
                }
                ```