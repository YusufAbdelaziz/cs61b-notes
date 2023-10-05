# 35 - Radix Sorts

### Warm-up: Digit-By-Digit Sort.

- A procedure that uses a sort (e.g. Merge Sort, Quicksort, Counting Sort).
- Using the word “sort” is arguably a misnomer.
    - Digit-by-digit sorting is a process that uses another sort as a subroutine.
- Given a list of integers, suppose we first sort by only the rightmost digit (least significant digit).
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled.png)
    
- Now could you guess what happens with the last four elements?
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%201.png)
    
- It actually depends upon the stability of the sorting algorithm you use.
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%202.png)
    
- The same goes for the second digit. We can also generalize this for n digits.

![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%203.png)

![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%204.png)

### Counting Sort.

- We try to get around the comparison-based sorting algorithms whose worst-case runtime is $\Theta(N\log{N})$.
- From an asymptotic perspective, that means no matter how clever we are, we can never beat Merge Sort’s worst case runtime of Θ(N log N).
- Assume we have a table whose keys are unique integers from 0 to 11.
- The idea is to create a new array and copy item with key i into ith entry of the new array.
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%205.png)
    
- This is called **counting sort** in which we can sort N items in $\Theta(N)$ worst case time. Avoiding yes/no questions lets us dodge MergeSort’s worst case runtime.
- Note that the simplest case is when the keys are between 0 to N-1.
- Notwithstanding we may face some complex cases:
    - Non-unique keys.
    - Non-consecutive keys.
    - Non-numerical keys.
- The general steps of Counting Sort that works with even complex cases are:
    - Count number of occurrences of each item.
    - Iterate through list, using count array to decide where to put everything.
- You can find a demo [here](https://docs.google.com/presentation/d/1vmVKHRSwb5WN1rHvktplbPGecHChxOwWa7ovRuiLzbA/edit#slide=id.g582f6c5a07_0_0).
- **The bottom line**: We can using Counting Sort to sort N objects in $\Theta(N)$ time.
- Note that counting sort is kind of **distribution sorts** (like sleep sort) in which you distribute items depending on the space.
- The running time of counting sort for R alphabet (range of values) and N keys is $\Theta(N+R).$
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%206.png)
    

### LSD Radix Sort.

- The main idea of LSD is to sort the elements starting from the LSD (Least Significant Digit).
- It’s the same idea of counting sort but you do it for the rightmost digit and then you continue with other digits until you find the correct order.
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%207.png)
    
- Notice in the above picture that we had a completely random input of numbers 22, 34, 41, etc. Then in the second box, we had sorted by right most digit, so 41, 41, 31 would come first as 1 is the lowest rightmost digit. Next would come 32, 22, 12, as 2 is the second least rightmost digit. Notice that we still have to take care of sorting 32, 22, and 12 which is why we move to the second right most digit in our last box which has everything sorted.
- Since we’re doing multiple passes of counting sort on each digit, the runtime is therefore affected.
- The total runtime is $\Theta(W N+ W R)$ where N is the number of items, R is the  size of alphabet and W is the **********width********** of the digits.
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%208.png)
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%209.png)
    

### MSD Radix Sort.

- Basic idea: Just like LSD, but sort from leftmost digit towards the right.
- Sorting from leftmost digit then middle digit, then rightmost digit will not result in a correct order.
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%2010.png)
    
- To fix that, you can partition each time you sort using a digit. This means you won’t move, for example, the first two items but you may change their relevant order.
    
    ![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%2011.png)
    
- If you end up with a partition that is less than 15 you can use insertion sort but we’d avoid that now.
- The runtimes are:
    - Best case → $\Theta{(N+R)}$ which happens when we finish in a single counting sort pass.
    - Worst case → $\Theta{(WN+WR)}$ which happens when we look for every digit, degenerating to LSD sort.

### Summary.

![Untitled](35%20-%20Radix%20Sorts%20f6ebf8cb7da54cc6b4fe432241fc5a04/Untitled%2012.png)

- Summary is [here](https://cs61b-2.gitbook.io/cs61b-textbook/35.-radix-sorts/35.4-summary).

### Exercises.

- Click [here](https://cs61b-2.gitbook.io/cs61b-textbook/35.-radix-sorts/35.5-exercises).