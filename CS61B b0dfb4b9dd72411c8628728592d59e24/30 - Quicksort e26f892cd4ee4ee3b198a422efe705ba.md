# 30 - Quicksort

### Partitioning.

- What is the idea behind partitioning?
    - Given an array `a[]`, you randomly select an element in it called `x` where `x = a[i]`.
    - To partition an array, you have to:
        - Move `x` to position `j` which may be the same as `i` (original place).
        - All entries to the left of `x` are less than or equal to `x`.
        - Whereas all entries on the right of `x` are bigger than or equal to `x`.
    - The item selected (which is called the pivot) will eventually be at the correct place.

### Quicksort Algorithm.

- Steps + Demo.
    - The steps of **Quick Sort** are as follows:
        - Partition on leftmost item.
        - Quick sort on left half.
        - Quick sort on right half.
    - You keep quick sorting until you get a partition of size 0.
    - So for example, given this array:
        
        ![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled.png)
        
    - We’ll pick 32 (leftmost item) as the pivot. After that we scan the array and use another array for copying. We copy the items that are less than 32 on the start of the array, whereas the item itself (or its duplicates) are placed after these items followed by the items that are larger than 32.
        
        ![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled%201.png)
        
    - Now left partition the left half by picking 15 as pivot. We’ll do the same procedure above.
        
        ![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled%202.png)
        
    - Same goes with remaining partitions.
    - Full demo is [here](https://docs.google.com/presentation/d/1QjAs-zx1i0_XWlLqsKtexb-iueao9jNLkN-gW9QxAD0/edit#slide=id.g12aa5d5cd8_0_108).
- Quick Sort Efficiency.
    - Empirically, quick sort is the fast sorting algorithm in common situations.

### Quicksort Performance Caveats.

- Best Case.
    - Pivot is always in the middle. So given N items, you problem is keep quick sorting N/2, N/4 and so on.
    - Runtime will ultimately be $\Theta(N\log{N})$.
        
        ![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled%203.png)
        
        Note that H is the height of the size of the arrays (Check [Master Theorem](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)) which is discussed in CS170).
        
- Worst Case.
    - Worst case happens when he pivot always lands at the beginning of the array.
    - This happens when the array is:
        - Already sorting or almost sorted.
        - With all duplicates.
    - Running time at that case is $\Theta(N^2)$.
        
        ![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled%204.png)
        
- Quick sort performance and comparison with Mergesort.
    
    ![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled%205.png)
    
- Quick sort performance arguments: Why does quick sort run in $\Theta(N\log{N})$ on average?
    - [Check](https://docs.google.com/presentation/d/1_7VuU3cynSGKbvzHk0PfK65HeHFT-jDp3XrbihRXetw/edit#slide=id.g4661758db_1583) the arguments starting from slide 32.
- Empirical Quicksort Runtimes.
    
    ![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled%206.png)
    
    - Click [here](http://www.informit.com/articles/article.aspx?p=2017754&seqNum=7) for more info.
- On what does the performance of quick sort depends?
    - It depends on:
        - How you select your pivot.
        - How you partition around that pivot.
        - Other optimizations you might add to speed things up.
    - Bad choices results in $\Theta(N^2)$
- Tips on avoiding the worst case.
    - Shuffle before starting.
    - Go through entire array, find the median, use that as the pivot.
    - Scan through one tie and check if it’s already sorted, don’t sort.

### Summary.

![Untitled](30%20-%20Quicksort%20e26f892cd4ee4ee3b198a422efe705ba/Untitled%207.png)

### Exercises.

- [Here](https://cs61b-2.gitbook.io/cs61b-textbook/30.-quicksort/30.5-exercises).