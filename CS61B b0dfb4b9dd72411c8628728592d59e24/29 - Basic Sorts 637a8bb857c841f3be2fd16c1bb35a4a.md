# 29 - Basic Sorts.

### The Sorting Problem.

- What is sorting? (Informally).
    - Informally: Given items, put them in order.
- Why sorting is useful?
    - Equivalent items are adjacent, allowing rapid duplicate finding.
    - Items are in increasing order, allowing binary search.
    - Can be converted into various balanced data structures (e.g. BSTs, KdTrees).
- What is sorting? (Formally).
    
    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled.png)
    
    - Note that the < symbol denotes an ordering relation.
- Example: String Length.
    
    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%201.png)
    
    - Note that the ordering relation is given in form of `compareTo` method in case your object extends from `Comparable` or `compare` in case you’re creating a comparator.
    - Note that the `equals` method in `String` class compares the character of strings to check if they’re identical (as Java’s `=` operator can’t be overridden).
    
    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%202.png)
    
- Sorting but with an alternative viewpoint.
    - Given a sequence, each pair of element in that sequence that are out of order with respect to a specific ordering relation is called an **inversion**.
    - Considering this sequence 0 1 1 2 3 4 8 6 9 5 7. There’re 6 inversions.
    - At most, there’re 55 inversions ($nCr$) where ***n*** is the number of elements and ***r*** is the number of items in each inversion which is 2.
    - We can think of sorting as a way to reduce the number of inversions to 0.
    - Having 0 inversions mean that we don’t need to switch elements so the sequence becomes sorted.
    - Some sorting algorithm may worsen the number of inversion temporarily (like the best-known sorting algorithm) but eventually the sequence gets sorted correctly.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%203.png)
        

### Performance Definition (Recall).

![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%204.png)

- Note that space complexity denotes the auxiliary space used by the algorithm. So any inputs (arrays, variables, etc.) are not included in the space complexity.

### Selection Sort & Heapsort.

- Selection Sort.
    - Steps + Running Time.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%205.png)
        
        - [Demo](https://docs.google.com/presentation/d/1p6g3r9BpwTARjUylA0V0yspP2temzHNJEJjCG41I4r0/edit#slide=id.g463de7561_042).
- Heapsort.
    - What’s the idea behind it.
        - Instead of rescanning entire array looking for minimum, maintain a heap so that getting the minimum is fast!
        - For reasons that will become clear soon, we’ll use a max-oriented heap. A min-heap will work but we can make use of a trick later when a max-heap is used.
    - Naïve approach.
        - Steps and demo.
            - Insert all items into a max heap, and discard input array. Create output array.
            - Repeat N times:
                - Delete largest item from the max heap.
                - Put largest item at the end of the unused part of the output array.
            - Check the demo [here](https://docs.google.com/presentation/d/1HVteFyWOxBW4mmUgkDnpUoTkWexiHt7Ei30Qolbc_I4/edit#slide=id.g12a23f896d_0_35).
            - Couple things to note from the demo:
                - You could use a min-heap and insert the elements at the start.
                - Recall that you can access the left and right children using `2i` and `2i+1` respectively. Parent can be accessed using `i/2` where ***i*** is the node number.
                - Insertion takes $\Theta(\log{n})$.
        - Running time.
            - Try to come up with the running time.
                
                ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%206.png)
                
                - Answer.
                    - $\Theta(N\log{N})$.
            - The runtime analysis is as follows:
                
                ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%207.png)
                
            - As mentioned above, the PQ itself will need $\Theta(N)$ auxiliary memory to store the data as well as another output array which is another $\Theta(N)$ which leaves us with a total space-complexity $\Theta(N)$. This is worse than selection sort space-wise.
            - However, we can do better. Let’s go with in place heapsort.
    - In-place Heapsort.
        - Steps + Demo.
            - So instead of creating a new array, we can make in-place heapification for the input array.
            - Note that since we’re using the input array to make in-place sorting, the zero at the first won’t exist so we’ll use the same formulas for getting left and right children but add 1 (`2i+1` and `2i+2`).
                
                ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%208.png)
                
            - Steps.
                - The full demo can be found [here](https://docs.google.com/presentation/d/1SzcQC48OB9agStD0dFRgccU-tyjD6m3esrSC-GLxmNc/edit#slide=id.g463de7561_042).
                - We first start with constructing a tree structure which is not yet a heap. We keep heapifying in reverse order so we start from the bottom right (using the array from right to left).
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%209.png)
                    
                - We start sinking 17 which has no effect, however, we keep going to see if we can sink each node in these subheaps (by saying subheaps I mean the smaller tree branches because a heap is a recursive data structure).
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2010.png)
                    
                - Sinking the third 17 would do nothing. However, we just mark the three 17 and make them blue to indicate that they belong to the same heap and the third 17 is the root.
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2011.png)
                    
                - Now let’s head to 2. 2 sinking will replace the biggest child which is 41 (recall this is a max-heap). The same goes with 15.
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2012.png)
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2013.png)
                    
                - The right and left branches are guaranteed to be heaps, however, the overall structure is not a heap yet. So we’ll go for 32 and replace it with either left or right child. In this case we’ll replace it with 41.
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2014.png)
                    
                - It worth to mention that this algorithm has four permutation. Sink in order or in reverse order, or swim in order or in reverse order. Two of them are not working (sink in order and swim in reverse order). The other too works but the one we discussed is more efficient.
                - **We’re not done yet**, this is a heap represented in array not the sorted array the user expects. So what we can do is to delete the front item (the root of the heap) and send it at the back to the array.
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2015.png)
                    
                - Recall that in heaps when an item is removed, we take the last item and place it as a new root and start sinking this item to place it in the correct position.
                - Note that we’ve used a **max-heap** because it will help us to do the trick (pushing the root to the end of the array).
                - Now try to think when 26 is removed, how will our array look like?
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2016.png)
                    
                    - Answer
                        
                        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2017.png)
                        
                - After repeating the same process, this is the result.
                    
                    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2018.png)
                    
        - Runtime + Space complexity.
            
            ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2019.png)
            
            ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2020.png)
            
            - It’s worth to note that any recursive algorithm needs $\Theta(\log{N})$ space to keep track of recursive calls. But it’s just a slight difference from constant space.
            - **Keep in mind** that the best case runtime for heapsort is $\Theta{(N)}$. This is **only valid** in case of an array that is full of duplicates.
- Runtime summary for selection sort & heapsort.
    
    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2021.png)
    
    - Note that heapsort keep looking for different parts of the array which is impractical in modern computers (you can check the **principle of locality** in caching to get an insight why it’s a bad idea to access different parts of arrays) .

### Merge Sort.

- Top-Down vs. Bottom-Up
    - These are two methods of merge sort.
    - Top-Down starts by splitting the sequence into 2 roughly even halves. After that, we keep recursively sorting by check if the left half is bigger than the right half and so on. Lastly, we merge the two sorted sub-sequences or sub-array into a mono-sorted array.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2022.png)
        
    - Bottom-Up, which is a non-recursive method, starts the opposite way. It divided the array into pairs of length 2. Each pair is then sorted and merged until we get the final result.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2023.png)
        
    - Click [here](https://www.baeldung.com/cs/merge-sort-top-down-vs-bottom-up) for more info.
    - Top-Down approach will be discussed.
- Steps + Demo.
    - Full demo is [here](https://docs.google.com/presentation/d/1h-gS13kKWSKd_5gt2FPXLYigFY4jf5rBkNFl3qZzRRw/edit#slide=id.g12a3009c32_0_662).
    - Split items into 2 roughly even pieces.
    - Recursively call merge sort on each half.
    - Merge the two sorted halves to form the final result.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2024.png)
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2025.png)
        
    - The merge steps compares between the items from the left half and the right half. Hence an auxiliary array is used to give the final result.
    - In case a half is finished before the other, we just keep copying the one with more items in the auxiliary array.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2026.png)
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2027.png)
        
- Runtime.
    - Time Complexity → $\Theta(N\log{N})$.
    - Space Complexity → $\Theta(N)$.
    - Note that it’s possible to do in-place merge sort to avoid the auxiliary space, but the algorithm is **very complicated**, and runtime performance suffers by a significant constant factor.
- Sorts so far.
    
    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2028.png)
    

### Insertion Sort.

- Idea behind it (General Strategy).
    - Starting with an empty output sequence.
    - Add each item f rom input, inserting into output at right point.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2029.png)
        
- Naïve Approach.
    - Naïve approach creates an output array to store the sorted sequence.
    - The main idea is to add each item in the appropriate place.
    - Check the [demo](https://docs.google.com/presentation/d/181Lhn8jf4N-VG1BOkV4-Caj1wKcavkls8fnTKJlCuXc/pub?start=false&loop=false&delayms=3000&slide=id.g463de7561_042) here to understand how elements are added to the output array.
    - This is an inefficient approach but that’s the idea of insertion sort. As we may want to insert item at the front so we need to move everything over. So if output array contains **k** items, worst cost to insert a single item is **k**.
- Efficient Approach.
    - More efficient approach is to keep swapping.
    - The main idea is to have 2 pointers `i` and `j`.
    - `i` holds the index of the **travelling item** and `j` is used to track the current spot of the travelling item and keeps going backwards to find the correct spot.
    - We swap an item backwards until the traveler is in the right place among all previously examined items.
    - So let’s check the [demo](https://docs.google.com/presentation/d/10b9aRqpGJu8pUk8OpfqUIEEm8ou-zmmC7b_BE5wgNg0/edit#slide=id.g463de7561_042) →
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2030.png)
        
    - Both `i` and `j` are gonna point to first element 32. Since there’re no element on the right, nothing really happens.
    - Let’s go to 15. Now `j` should go back to check for the correct spot which is before 32.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2031.png)
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2032.png)
        
    - Going for 2, we do the same procedure. Note that we keep swapping so we swap 2 with 32 and then 2 with 15. This means that we temporarily break the invariant but it will stored once `j` settles for the correct spot.
    - The same thing goes with all elements →
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2033.png)
        
- More examples.
    
    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2034.png)
    
- Insertion Sort Efficiency on Almost Sorted and Small Arrays
    
    ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2035.png)
    
    - Insertion sort is the most efficient algorithm for sorting in case the data is almost sorted. It’s running time in this case is $\Theta(N)$.
    - Note that the Java [implementation](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/6-b14/java/util/Arrays.java#Arrays.mergeSort%28java.lang.Object%5B%5D%2Cjava.lang.Object%5B%5D%2Cint%2Cint%2Cint%29) of mergesort (which is Tim Sort) uses insertion sort when the split becomes less than 15 items.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2036.png)
        
- Runtime.
    - Lower bound is $\Omega(N)$ which includes looping over all elements to check if they’re in correct spots.
    - Upper bound is $O(N^2)$ as you may go through the array twice if the array is sorted in reverse order.
        
        ![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2037.png)
        

### Summary.

![Untitled](29%20-%20Basic%20Sorts%20637a8bb857c841f3be2fd16c1bb35a4a/Untitled%2038.png)

- Heapsort runtime is $\Theta(N)$ in case the array is full of duplicates.

### Textbook Exercises.

- Check [here](https://cs61b-2.gitbook.io/cs61b-textbook/29.-basic-sorts/29.6-exercises).