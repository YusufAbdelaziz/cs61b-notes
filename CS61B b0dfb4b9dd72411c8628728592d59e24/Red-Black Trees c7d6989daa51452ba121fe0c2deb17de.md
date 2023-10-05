# Red-Black Trees.

- Intro
    - We’ve introduced B-Trees (4-3-2 and 2-3 trees) and we saw that there’re beautifully balanced trees.
    - Why looking for alternatives ? In fact, the B-trees we discussed have some performance issues and other issues include :
        - Maintaining different node types is painful. Some nodes would have 2 children, other could have 3 or 4.
        - Interconversion of nodes between 2-nodes and 3-nodes (recall that when we add an item next to items that exist in a node, we rearrange its children node and we had to add some children nodes between the items).
        - Walking up the tree to split nodes.
            
            ```java
            // A fantasy 2-3 code via Kevin Wayne.
            	
            public void put(Key key, Value val) {
               Node x = root;
               while (x.getTheCorrectChildKey(key) != null) {
                  x = x.getTheCorrectChildKey();
                  if (x.is4Node()) { x.split(); }
               }
               if (x.is2Node()) { x.make3Node(key, val); }
               if (x.is3Node()) { x.make4Node(key, val); }
            }
            ```
            
    
- What is a Rotation ?
    - Given number 1, 2, and 3, you’d find five possible BSTs.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled.png)
        
    - You might notice that each BST you get depends on the insertion order.
    - For N items, you’d get [Catalan(N)](https://en.wikipedia.org/wiki/Catalan_number) different BSTs.
    - So what’s the point of that ? The main point is that you can change the configuration you got by **rotation** and get another configuration.
    - So from a spindly BST, you can go to any other configuration (bushy one) in  $2n-6$ rotations. Details from [here](https://medium.com/@liuamyj/its-triangles-all-the-way-down-part-1-17f932f4c438).
    - The main idea of rotation is that we pick a specific node, and either rotate it to the right or to the left.
    - Let’s see an example of left rotation. We’re going to rotate node G to the left.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%201.png)
        
    - rotateLeft(G) : Let x be the right child of G (which is P in our case). Make G the new left child of x. Remember to preserve the search tree property, no changes should happen to the semantics of the tree.
    - We can think of left rotation as temporarily merging G and P, and then sending G down and left.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%202.png)
        
    - Notice that the rotation results in an increase in tree height !
    - rotateRight(P) : Let x be the left of child P (G in this case) Make P the new right child of x.
    - We can think of right rotation as temporarily merging G and P, then sending P down and right.
    - So given this tree, try to rotate P to the right.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%203.png)
        
        - Answer
            
            ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%201.png)
            
            - Notice that we managed to decrease the height !
        
- How to balance a BST with rotations ?
    - Given this BST, try to give the sequence of rotation operations that balances the tree of the left.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%204.png)
        
        Note : There’re multiple answers
        
        - Answer
            - One possible answer :
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%205.png)
                
    - Rotation has cool properties :
        - It can shorten (or lengthen) a tree.
        - It also preserves search tree property.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%206.png)
        
    - So we can deduce from the following that rotation helps in balancing BSTs. Check out [this](https://docs.google.com/presentation/d/1pfkQENfIBwiThGGFVO5xvlVp7XAUONI2BwBqYxib0A4/edit#slide=id.g465b5392c_00) demo to see how to balance a bigger tree (we’ll get into how to select the right node to rotate at).
    - In case your tree is spindly (like a linked list) and you decided to do the balancing process later, you’d need O(n) operations to balance the whole tree.
    - This is redundant, we want to maintain the balance of the tree while building it.
    - Implementations of `rotateRight` and `rotateLeft` courtesy of
        
        ```java
        private Node rotateRight(Node h) {
            // assert (h != null) && isRed(h.left);
            Node x = h.left;
            h.left = x.right;
            x.right = h;
            return x;
        }
        
        // make a right-leaning link lean to the left
        private Node rotateLeft(Node h) {
            // assert (h != null) && isRed(h.right);
            Node x = h.right;
            h.right = x.left;
            x.left = h;
            return x;
        }
        ```
        
- Red-Black Trees - Definition.
    - So far we’ve discussed two types of trees :
        - **Binary Search Trees**: Can be balanced using rotation but we’ve not discussed about the algorithm yet.
        - **2-3 Trees:** Balanced by construction which means no rotations required.
    - Let’s try to build a BST that is **structurally** identical to a 2-3 tree.
    - Representing a 2-3 tree with 2 nodes is identical to a traditional BST.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%207.png)
        
    - However, representing a 2-3 tree with 3 nodes is tricky.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%208.png)
        
    - One possibility is to use a dummy node as parent of these two items in the node **d f.**
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%209.png)
        
    - The problem with this approach is that there’s always a wasted link which is the right leaf of **d**.
    - A better possibility to follow is to make the bigger item as the parent and the smaller item as **left child.** It’s important to note that this approach is used as an implementation of **TreeSet.**
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2010.png)
        
    - This kind of trees are considered to be a valid representation of 2-3 tree.
    - Since we add a left link as a relation between the two nodes, it’s called a **Left Leaning Red Black Binary Search Tree** or **LLRBBST (LLRB is more simpler)** for short.
    - Some notes to consider with LLRB :
        - LLRBs are just BSTs.
        - There’s a 1-1 correspondences between an LLRB and an equivalent 2-3 tree. This means that any 2-3 tree has one and only one LLRB equivalent. This is also called one-to-one mapping or **bijection**.
        - The red is just a convenient fiction. Red links don’t do anything special.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2011.png)
        
    - Try to come up with a solution to this conversion
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2012.png)
        
        - Solution
            
            ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2013.png)
            
- Red-Black Trees - Properties.
    - Let’s first try this problem. Take your time, it’s a challenging one.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2014.png)
        
        - Solution
            - First one on the left is an **invalid** 2-3 B-tree. The reason for that you can’t get 2-3 B-tree of this structure. Essentially, nodes A, B, and C are being together which would look like below. This is a 4-3-2 B-tree which has 4 children nodes not 2 or 3 like 2-3 B-tree. To get a balanced BST, the B should go up and both A and C nodes are the 2 children of B.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2015.png)
                
            - The second one has a problem which is that when we convert it into 2-3 B-tree, we get an unbalanced tree. R**emember, every leaf in 2-3 B-tree must have the same distance from the root (revisit B-tree invariants ⬆)**. So C’s depth 2 and X has depth 1. Another problem is that AB has only a single child.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2016.png)
                
            - The third one has the same issues as the second one, you can’t get a valid 2-3 B-tree representation with leaves of different depths.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2017.png)
                
            - The last one has a valid correspondence of a 2-3 B-tree. Each leaf’s depth is 1.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2018.png)
                
    - Try this one.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2019.png)
        
        - Solution
            
            ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2020.png)
            
    - In addition to the previous question, we notice that the maximum height of an LLRB corresponding to a 2-3 tree of height $H$ is $H(Black) + H + 1 (Red) =2H+1$.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2021.png)
        
    - So from the previous B-tree suppose L and P nodes have two items inside, so we’d have 3 black links and 4 red links. This results in 2H + 1 which is $2*3 + 1=7$.
    - LLRB has some handy properties :
        - **No node has two red links (otherwise it’d be analogous to a 4 node, which is not allowed in 2-3 trees).**
        - **Every path from root to a null reference (which means a leaf’s child which is null) has same number of black links (because 2-3 trees have the same number of links to every leaf). Therefore, LLRBs are balanced**
            
            ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2022.png)
            
    - We can notice that A has 2 black links from the root in 2-3 B-tree and the LLRB representation.
    - You can now go back and resolve the first quiz using the properties above.
- Red-Black Trees - Insertion (How to construct a LLRB).
    - So here comes the big question, how to create LLRBs ? Should we just create 2-3 B-tree and then convert it into LLRB?
    - This invalidates the point of LLRB. We said that 2-3 B-tree are hard to implement and maintain. So what we’re going to do is when items are inserted items to a normal BST, we do some number of rotations to maintain 1-1 correspondence with equivalent 2-3 B-tree.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2023.png)
        
    - Our main goal in inserting items to a LLRB, we should preserve the correspondence using tree rotations.
    - A trivial question would pop up. Should we use a red or black link when inserting ?
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2024.png)
        
        - Solution.
            
            ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2025.png)
            
    - Another question appears. Suppose we have leaf E and we insert S with a red link. What is the problem below ? How can we maintain 1-1 correspondence ?
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2026.png)
        
        - Solution
            - Our LLRB are called Left Leaning Red Black Trees so right links aren’t allowed. Therefore, we should rotate E to the left.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2027.png)
                
    - We’re introduce a new rule which allows representing 4-nodes as BST nodes with two **red links**. This state is only **temporary**, so we’re violating left leaning **temporarily.**
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2028.png)
        
    - So here’s a follow-up question :
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2029.png)
        
        - Solution
            - You should rotate Z to the right.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2030.png)
                
    - Now here comes the most difficult exercise so far.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2031.png)
        
        - Hint
            - We don’t need any kind of rotations.
        - Solution
            - It’s silly, but you need to flip the colors >_<.
            - This doesn’t change the shape or structure of the BST.
            - We’re doing this to maintain perfect 1-1 mapping with 2-3 B-tree.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2032.png)
                
    - Now, you might think that we’ve been playing with rotations and flipping colors and you have no idea how is that relating to creating a LLRB.
    - Well, we’ve went through process of inventing a red-black BST.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2033.png)
        
    - Last question about cascading operations.
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2034.png)
        
        - Solution
            - From the above summary, we have a **Left Leaning Violation** that needs a left rotation of node B.
                
                ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2035.png)
                
    - In case you want to build an intuition of creating LLRB, check this example and its solution
        
        ![Untitled](Red-Black%20Trees%20c7d6989daa51452ba121fe0c2deb17de/Untitled%2036.png)
        
        - [Solution](https://docs.google.com/presentation/d/1jgOgvx8tyu_LQ5Y21k4wYLffwp84putW8iD7_EerQmI/edit#slide=id.g463de7561_042)
- Red-Black Trees - Performance.
    - Since LLRB is a correspondence of 2-3 tree, then both have the same runtime analysis.
    - LLRB tree has height $O(log{N})$.
    - `contains` takes $O(\log{N})$.
    - `insert` is $O(\log{N})$. Insertion running time is a little bit interesting since it’s a process of two operations :
        - To add a new node, you need $O(\log{N})$.
        - $O(\log{N})$ rotation and color flip operations per insert.
    - Delete operation is not interesting to cover and it’s a little bit complex.
    - LLRB is like BST in terms of performance but unlike B-trees in terms of implementation (easier to implement).
    - Turning BST into LLRB is relatively easy in terms of implementation.
        
        ```java
        private Node put(Node h, Key key, Value val) {
            if (h == null) { return new Node(key, val, RED); }
        
            int cmp = key.compareTo(h.key);
            if (cmp < 0)      { h.left  = put(h.left,  key, val); }
            else if (cmp > 0) { h.right = put(h.right, key, val); }
            else              { h.val   = val;                    }
        
            if (isRed(h.right) && !isRed(h.left))      { h = rotateLeft(h);  }
            if (isRed(h.left)  &&  isRed(h.left.left)) { h = rotateRight(h); }
            if (isRed(h.left)  &&  isRed(h.right))     { flipColors(h);      } 
        
            return h;
        }
        ```
        
    - Check Algs4 full implementation from [here](https://algs4.cs.princeton.edu/33balanced/RedBlackBST.java.html).
    - We’re more interested on the concepts not the details.
- Summary.
    - We’ve talked about using search trees to implement sets/maps.
    - **Binary search trees** are simple, but they are subject to imbalance.
    - **2-3 Trees (B Trees)** are balanced, but painful to implement and relatively slow.
    - **LLRBs** insertion is simple to implement (but delete is hard).
        - Works by maintaining mathematical bijection with a 2-3 trees.
    - Java’s [TreeMap](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/999dbd4192d0f819cb5224f26e9e7fa75ca6f289/src/java.base/share/classes/java/util/TreeMap.java) is a red-black tree (not left leaning).
        - Maintains correspondence with 2-3-4 tree (is not a 1-1 correspondence).
        - Allows glue links on either side (see [Red-Black Tree](http://en.wikipedia.org/wiki/Red%E2%80%93black_tree)).
        - More complex implementation, but significantly (?) faster.
    - There are many other types of search trees out there.
        - Other self balancing trees: AVL trees, splay trees, treaps, etc. There are at least hundreds of different such trees.
    - And there are other efficient ways to implement sets and maps entirely.
        - Other linked structures: Skip lists are linked lists with express lanes.
        - Other ideas entirely: Hashing is the most common alternative. We’ll discuss this very important idea in our next lecture.