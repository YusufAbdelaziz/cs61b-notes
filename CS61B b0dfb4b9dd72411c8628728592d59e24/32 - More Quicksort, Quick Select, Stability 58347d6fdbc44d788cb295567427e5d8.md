# 32 - More Quicksort, Quick Select, Stability

### Philosophies to Avoid Worst Case

- Quick Sort Review.
    - The main idea of quick sort is to select a pivot (which was the leftmost item) and partition the array such that the items on the left are ≤ the pivot and items on the right are ≥ the pivot.
    - Runtime is $\Theta(N\log{N})$ in best and average cases (where the pivot lands in the middle) and $\Theta({N^2})$. Worst case happens when the array is sorted, almost sorted or is full of duplicates.
- Four philosophies to avoid worst case for Quicksort.
    - Four philosophies:
        1. **Randomness**: Pick a random pivot or shuffle before sorting.
        2. **Smarter pivot selection**: Calculate or approximate the median.
        3. **Introspection**: Switch to a safer sort if recursion goes to deep.
        4. **Preprocess the array**: Could analyze array to see if Quicksort will be slow. No obvious way to do this, though (can’t just check if array is sorted, almost sorted arrays are almost slow).
- Philosophy 1: Randomness.
    - Worst case may happen due to:
        - Bad ordering: Array already in sorted (almost sorted).
        - Bad Elements: For of duplicates.
    - We can deal with bad ordering via:
        - Strategy #1: Pick pivots randomly.
        - Strategy #2: Shuffle before you sort.
    - The second strategy requires care in partitioning code to avoid $\Theta{(N^2)}$ behavior on arrays of duplicates.
- Philosophy 2: Smarter Pivot Selection.
    - 2a - Calculate Median Approximately (Constant Time).
        - For any pivot selection procedure that is:
            - Deterministic.
            - Constant Time.
        - So we can calculate an approximate median of an array. You can for example select the first, middle, and last item and calculate the median of the three.
        - This procedure is deterministic because we basically picking the first, middle, last elements and runs in constant time because we use these elements to get the median of them using a constant calculation.
        - Note that any pivot selection procedure where you take a small sample deterministically and run a constant time operation, there’s an input that may cause QuickSort to run in quadratic time $O(N^2)$.
            - See McIlroy’s “[A Killer Adversary for Quicksort](http://www.cs.dartmouth.edu/~doug/mdmspe.pdf)”
    - 2b - Calculate Median Exactly (Linear Time).
        - We could calculate the actual median (not an approximation) in **linear time**.
        - However, the “Exact median QuickSort” is safe and it’s worst case is $\Theta(N\log{N})$ but it’s slower than mergesort.
        - We’ll discuss how to select the exact median of the array.
- Philosophy 3: Introspection.
    - Not used in practice but nice to know. It’s used in machine learning algorithms.
    - The main idea is to track recursion depth. So if it exceeds some critical value (say $10\ln{N}$), switch to merge sort.
    - It’s a reasonable approach but not super common in practice.
- Which philosophy does Java use to avoid worst case?
    - Java uses 2a which calculates an approximation of the median of an array..

### QuickSort Algorithm Partitioning Schemes Improvements and Quicksort Flavors vs. MergeSort

- QuickSort L3S vs. MergeSort.
    - The version of QuickSort is called L3S which means:
        - The pivot is always the **leftmost** element.
        - Partition algorithm is a **3-scan** one (scan for reds, white, and blue elements).
        - **Shuffle** the elements before sorting.
    - Empirically, QuickSort L3S is worse than MergeSort.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled.png)
        
- Hoare’s In-place Partitioning Scheme + Demo.
    - Tony originally proposed a scheme where two pointers walk towards each other.
        - Left pointer loves small items.
        - Right pointer loves large items.
        - Big idea: Walk towards each other, swapping anything they don’t like.
            - End result is that things on left are “small” and things on the right are “large”.
    - So let’s briefly walk through a demo:
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%201.png)
        
    - We’ll create **L** and **G** points at left and right ends. Note that the leftmost element is the pivot so the **L** pointer points to the second element.
    - Note that **L** (less) pointer loves small items, and hates large or equal items.
    - Also **G** (greater) pointer loves large items and hates small or equal items.
    - The equality here is done with respect to the pivot.
    - The algorithm is as follows:
        - Walk pointers towards each other, stopping on a hated item.
            - When both pointers have stopped, swap and move pointers by one.
        - When pointers cross, you’re done.
        - Swap the pivot with pointer **G**.
    - [Demo](https://docs.google.com/presentation/d/1DOnWS59PJOa-LaBfttPRseIpwLGefZkn450TMSSUiQY/pub?start=false&loop=false&delayms=3000&slide=id.g12b16fb6b6_0_295)
        - So following the algorithm, L loves 15 so it keeps moving until it hits 19 which it hates.
            
            ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%202.png)
            
        - The same goes with G, it hates items that are equal to the pivot so we swap G with L and move both pointers by one.
            
            ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%203.png)
            
        - We keep going until L finishes all the small items.
            
            ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%204.png)
            
            ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%205.png)
            
        - Recall that the two pointers **have to stop** to make a **swap**. This means that G keeps moving until it cross L.
            
            ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%206.png)
            
        - Once G cross L, we swap G with the pivot and finally we’re done with the in-place partitioning. **Note that** we choose to swap the pivot with G because swapping it with L will break the invariant (small items on the left, large items on the right).
            
            ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%207.png)
            
- QuickSort LTHS vs. MergeSort.
    
    ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%208.png)
    
    - The quick version request is [here](https://web.archive.org/web/20100428064017/http://permalink.gmane.org/gmane.comp.java.openjdk.core-libs.devel/2628).
    - The improvements of partitioning algorithms result in a better constant coefficient which results in a faster quick sort algorithm overall.
- Why don’t we like randomness?
    - Randomness as a philosophy to avoid worst case behavior is not recommended.
    - This is because:
        - Random number comes from somewhere out of the algorithm which means that it’s invisible to the user.
        - More importantly reason: We can’t replicate what is going on with the algorithm. You have to options: First is to accept the randomness. Second you have to provide a seed. However both seem sort of strange.
- Philosophy 2b - Calculating exact median using BFPRT.
    
    ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%209.png)
    
- QuickSort PickTH vs. MergeSort.
    
    ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2010.png)
    
- Extra → Partitioning Algorithms.
    - [Lomuto’s Scheme](https://www.geeksforgeeks.org/hoares-vs-lomuto-partition-scheme-quicksort/) → Slower than Hoare’s but easier to implement.
    - [3-Way QuickSort](https://www.geeksforgeeks.org/3-way-quicksort-dutch-national-flag/) → Used with redundant elements (array with repeated elements). So instead of picking a single element as a pivot and partition around it, try to find its other occurrences. This scheme uses [Dutch National Flag Algorithm](https://www.geeksforgeeks.org/sort-an-array-of-0s-1s-and-2s/).

### Quick Select (Median Finding Algorithm).

- Selection Problem.
    - Calculating the exact median is pretty helpful to partition the array around it.
    - However, the PICK algorithm was really slow to use.
    - Partitioning itself can be used to find the exact median. This is the best known identification algorithm.
- Quick Select Algorithm Steps.
    
    ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2011.png)
    
    - The goal is to find the median. The median here should be of index 4 ( 9 / 2 ).
    - We start picking the pivot as leftmost item.
    - Thereafter, we use Hoare’s algorithm for partitioning.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2012.png)
        
    - Note here the pivot lands at index 2 which is not yet the median. Here comes the main idea, we don’t have to take a look at the left part. The median exists at the right part.
    - Partition the subproblem. Repeat the process.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2013.png)
        
    - Finally, stop when the pivot is the median.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2014.png)
        
- Quick Select Algorithm Worst Case.
    - Worst case happens when the array is sorted. The running time for worst case is $\Theta(N^2)$.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2015.png)
        
- Quick Select Average Case.
    - On average, Quick Select will take $\Theta(N)$ time.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2016.png)
        
- Could we use Quick Select as a pivot selection algorithm for QuickSort?
    - Technically yes, but impractical and results in a quite slow algorithm.
    - It’s also weird to do partitioning to find the median to select it as a pivot and to do partitioning again around that pivot.

### Stability, Adaptiveness, and Optimization.

- Stability.
    - A sort is said to be **stable** if **order of equivalent items** is preserved.
    - The following is an example of a stable sort. After sorting by section, notice how Bas, Jana, Jouni, and Rosella are in the same order as before sorting.
    - If we want records sorted by section and then by name within each section, we can sort by name and then by section as below.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2017.png)
        
    - The following example is an unstable sort. It can make things really annoying! If we want records sorted by section and then by name within each section, we can't just sort by name and then by section as before. After an unstable sort, the previous ordering is not maintained.
        
        ![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2018.png)
        
- Sorting Algorithms Stability.
    - Insertion sort is indeed stable because equivalent elements move past their equivalent brethren.
    - MergeSort is stable.
    - HeapSort is not stable.
    - QuickSort can be stable depending on its partitioning scheme, but its stability cannot be assumed since many of its popular partitioning schemes, like Hoare, are unstable.
- What does it mean for a sorting algorithm to be adaptive and optimized?
    - Adaptiveness - A sort that is adaptive exploits the existing order of the array. Examples are InsertionSort, SmoothSort, and TimSort.
    - Switch to Insertion Sort - When a subproblem reaches size 15 or lower, use insertion sort. It is very very fast for inputs of small sizes.
    - Exploit restrictions on set of keys - For example, if the number of keys is some constant, we can use this constraint to sort faster by applying 3-way QuickSort.
    - Switch from QuickSort - If the recursion goes too deep, switch to a different type of sort.

### Summary.

![Untitled](32%20-%20More%20Quicksort,%20Quick%20Select,%20Stability%2058347d6fdbc44d788cb295567427e5d8/Untitled%2019.png)

- CS61B [book’s summary](https://cs61b-2.gitbook.io/cs61b-textbook/32.-more-quick-sort-sorting-summary/32.4-summary) is great.

### Exercises.

- [Here](https://cs61b-2.gitbook.io/cs61b-textbook/32.-more-quick-sort-sorting-summary/32.5-exercises).