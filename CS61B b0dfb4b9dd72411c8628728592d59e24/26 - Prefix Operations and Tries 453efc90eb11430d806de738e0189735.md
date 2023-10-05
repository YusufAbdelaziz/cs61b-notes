# 26 - Prefix Operations and Tries.

### Recalls and what are we going to to do.

- We mentioned that an **ADT** (**Abstract Data Type**) defines the operations that a data type can operate.
- Whereas an **implementation** is how these operations are implemented in code.
- So for example, a List ADT can be implemented using a Linked List or a Resizing Array (ArrayList).
- We’re going to talk about a new way (implementation) to build a set/map.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled.png)
    

### BST and Hash Table Set Runtimes and Special Cases.

- BSTs and Hash Tables performance were pretty fast.
- However, knowing a special property that our keys may have can sometimes get even better implementation.
- Example: Suppose we know our keys are always single ASCII characters. e.g. ‘a’, ‘b’, ‘!’.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%201.png)
    
- Special Case 1: Character Keyed Map.
    - If our keys are always ASCII characters, we can use the fact that they’ll be translated to its ASCII value. Note that the **ASCII table** serves as a **hashing function** here so it translates the character into a valid index.
    - So if we want to store the entry (A, 360), the A is translated to ASCII equivalent which is 97 so we go to the 97th element as store 360 in it.
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%202.png)
        
    - Note thar we ignore the fact that we waste some memory here (memory is occupied but not utilized) unlike balanced BSTs which scales with the number of active keys.
- Special Case 2: String Keyed Map.
    - Our keys in this case are always strings.
    - There’s a special data structure known as Trie that stores each letter of the string as a node in a tree.
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%203.png)
        
- It’s worth to note that the key should be **comparable** when a balanced BST is used and **hashable** when a hash table is used.

### Tries: What is it, Inventing a Trie, Tries as Maps.

- What is a Trie?
    - A Trie is a data structure that consists of a tree of nodes.
    - Each node stores only one **letter**.
    - Nodes can be shared by multiple keys (recall the the keys are strings).
    - Very useful DS for auto-completion.
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%204.png)
        
- Inventing a Trie.
    - Trie Insertion.
        - Recall the definition, try to use your intuition to insert the remaining items.
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%205.png)
            
            - Answer
                
                ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%206.png)
                
                ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%207.png)
                
    - Trie Nodes Distinction.
        - Another quick quiz, how can we distinguish that our set contains the previous strings but not “aw”, “awl” or “sa”?
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%208.png)
            
            - Answer.
                - You may have a field at each node to indicate whether this node is the complete character that completes a word.
                    
                    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%209.png)
                    
                - So nodes ‘a’, ‘s’, ‘d’, ‘m’, ‘p’, and ‘e’ should set this property to true and all other nodes set it to false.
    - Trie Search Hits and Misses.
        - Recall the Trie we created before, now we want to check if our Trie contains these words:
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2010.png)
            
        - Starting with “sam”, we keep traversing until we hit a blue node (a node that marks the end of a word in our Trie). So we got through ‘s’, ‘a’ and finally ‘m’ which is a blue node and we successfully return true.
        - The same goes with the remaining `contains` calls.
        - We return false in case the final node is white or the node doesn’t exist.
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2011.png)
            
- Tries as Maps
    - Tries can be Maps, they don’t have to be only Sets like we discussed before.
    - So a node won’t just store a single letter, but also a value associated to that letter.
    - Note that the value will be associated to blue nodes only (aka final nodes).
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2012.png)
        

### Trie Implementation.

- Basic Trie Implementation.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2013.png)
    
    - In this example, each node stores a character and a map.
    - This map associates a character to a node. So if I’m at node ‘a’, I can have up to 128 different children (because we’re supporting ASCII). So the children of ‘a’ are ‘d’, ‘m’, and ‘p’. If you want to retrieve any of them you can use the map to get the node by its key (character).
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2014.png)
        
    - This is obviously redundant and a memory waste as we only utilize three places and left 125 unused links
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2015.png)
        
    - Besides the memory wastage, we don’t need to store the character in the node itself because the parent node stores that already in the map.
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2016.png)
        
    - So what is the performance of this implementation?
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2017.png)
        
        - Answer
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2018.png)
            
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2019.png)
    
- Alternate Child (Child in a Trie of course :D) Tracking Strategies.
    - Alternate Idea #1: The Hash-Table Based Trie.
        - We can use a hash table as data structure to store children, where each character is the key and the node is the value.
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2020.png)
            
        - Having four buckets initially is way better than having redundant links. However, we can still do better.
    - Alternate Idea #2: The BST-Based Trie.
        - Links can be stored using BST as follows.
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2021.png)
            
- Performance comparison between the three implementation.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2022.png)
    
    - Note that O(R) in the hast table implementation is not realistic as we shouldn’t get a collision. However, if somehow all letters are stuck in a single bucket, this gonna be a search operation in a linked list.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2023.png)
    

### Trie String Operations.

- String Specific Operations.
    - Tries have a speed improvement over other data structures like BSTs or hash tables. It shines really on string specific operations like ********************************prefix matching.********************************
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2024.png)
        
    
- Prefix Matching Operations.
    - Tries is pretty neat with prefix matching operations whereas other data structures will struggle with these operations.
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2025.png)
        
- Challenging Warmup Exercise: Collecting Trie Keys.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2026.png)
    
    - Hint
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2027.png)
        
    - Answer
        - We check if n is the key (final char that constitutes a word), if that’s the case then we add the string s to the result list and we then call the method on each child in the node n. We keep doing that recursively until we visit all nodes.
            
            ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2028.png)
            
    - Check slide 43 [here](https://docs.google.com/presentation/d/1Xvm1BB6WeUwYrwRe8QmcRscD0ivqOlLyWBhYQrDxzHQ/edit#slide=id.g528d808e96_0_3539).
- Another cool usage of tries: Get a list of keys that starts with a specific prefix.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2029.png)
    
    - Answer
        
        ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2030.png)
        
    

### Tries: Autocomplete.

![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2031.png)

- Note that each string has a value that may be generated from ML algorithms.
- Autocomplete example for top three matches.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2032.png)
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2033.png)
    
- Here comes a problem: what should we do with short strings?
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2034.png)
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2035.png)
    
- We can make a little modification for more efficient autocomplete by creating **radix tree** or **radix trie** by merging multiple nodes into a single word. Note that it won’t be discussed in this course.
    
    ![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2036.png)
    

### Summary.

![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2037.png)

![Untitled](26%20-%20Prefix%20Operations%20and%20Tries%20453efc90eb11430d806de738e0189735/Untitled%2038.png)

- Suffix Trees ([Link](https://en.wikipedia.org/wiki/Suffix_tree)).
- DAWG ([Link](https://en.wikipedia.org/wiki/Deterministic_acyclic_finite_state_automaton)).
- Won’t discuss in our course.

### Metacognitive Question From CS61B Textbook.

- Compare the worst-case number of character comparisons required to insert a word into an LLRB, hash table, and R-way trie. Let R be the size of the alphabet, and N be the number of items in the trie, and L be the maximum length of any word.
- Answer
    - **LLRB**: We always insert at the bottom of the LLRB, so there are  $\Theta(\log N)$ comparisons to figure out where the new node goes. Each word comparison takes up to L character comparisons. Thus, there are $\Theta(L \log N)$ comparisons.
    - **Hash table**: In the worst case, all items hash to the same bucket. On an insertion, we must compare a word to all other words in the bucket for equality. Assuming this bucket has N items, this take $\Theta(LN)$ comparisons.
    - **R-way trie**: In the worst case, we follow or create L nodes to the end of the word. Thus, there are at most $\Theta(L)$ comparisons.