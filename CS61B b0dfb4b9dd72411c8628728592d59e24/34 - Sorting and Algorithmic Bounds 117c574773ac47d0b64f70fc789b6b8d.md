# 34  - Sorting and Algorithmic Bounds

### Sorting Summary + What are going to do?

- Summary
    - We talked about various sorting algorithms like:
        - Insertion Sort.
        - Selection Sort.
        - MergeSort.
        - QuickSort.
        - HeapSort.
    - Each with its own advantage and disadvantage.
    - We also discussed about an importing feature in sorting algorithms which was stability.
    - A sorting algorithm is said to be **stable** if the **order of equivalent items is preserved**.
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled.png)
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%201.png)
        
    - Note that in Java, `Arrays.sort(arr)` method uses two different algorithms (actually three):
        - TimSort (which is a combination is MergeSort and Insertion Sort) and it’s used with objects.
        - QuickSort (Dual-Pivot QuickSort which is the fastest variation of quick sort) for primitives.
    - Why is that? The reason is that maintaining the order of equivalent items of objects is more sensible than primitives. You can’t distinguish between a 5 and another 5 and it won’t matter whichever comes first. However, this is not the case with objects as you can see with the rows of the table, you may need to maintain the order of equivalent objects.
    - Although sorting can be used to obviously putting things in order. It can be used to solve other tasks in non-trivial ways:
        - Sorting improves duplicate finding from a naïve $N^2$to $N \log{N}$.
        - Sorting improves 3SUM from a naïve $N^3$ to $N^2$.
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%202.png)
        
- Objective.
    - We want to discuss how hard it is to sort.

### A Math Problem out of Nowhere.

- Try to solve this problem:
    
    ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%203.png)
    
    - Answer.
        - Yes it does. For any arbitrary N, say 10, we can see that $N!$ is larger than $(\frac{N}{2})^{(\frac{N}{2})}$
        .
            
            ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%204.png)
            
- Give this another one a shot:
    
    ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%205.png)
    
    - Answer.
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%206.png)
        
- Last problem:
    
    ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%207.png)
    
    - Answer
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%208.png)
        
- You might think that the last two problems are contradicting. So here’s a question:
    
    ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%209.png)
    
- Note that Omega $\Omega$ refers informally as $>=$. It’s same as if we say that A ≥ B and B ≥ A this means that A = B so the answer is **C**. Note that Theta $\Theta$ is a tight bound which informally means equality. So both A and B are the same.
    
    ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2010.png)
    
- So to summarize, both $\log{N!}$ and $N\log{N}$ grow at the same rate ******************************asymptotically******************************. We’ll discuss in a while why we did all these math problems.

### Theoretical Bounds of Sorting.

- What is the best time we can get for sorting?
    - We already discovered multiple sorting algorithms and the runtime of the fastest one (QuickSort) was $\Theta(N\log{N})$.
    - Can we build an algorithm that is faster than these algorithms?
    - Suppose we have an algorithm and we’ll call it as the ultimate comparison sort ********TUCS******** (compares between elements using Java’s `compareTo` method).
    - We’ll assume that this is the fastest algorithm possible. Also, we’ll let $R(N)$ which is the runtime of TUCS and the worst case runtime is in $\Theta$ notation.
    - We can say a couple statements here:
        - Worst case runtime of TUCS, R(N) is $O(N\log{N})$. Why? because we said earlier that TUCS is the fastest sorting algorithm so it’s **upper bounded** by $O(N\log{N})$ so it should at least perform as quick sort or better.
        - We can also say that it’s **lower bounded** is $\Omega(N)$. Why? because you should at least scan through the array (supposing no other actions than comparisons is made).
            
            ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2011.png)
            
- A Game of Cat, Mouse, and Dog (Comparisons).
    - Let's say we knew that Box A weighs less than Box B, and that Box B weighs less than Box C. Could we tell which is which?
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2012.png)
        
    - We can find a sorted order by just considering these two inequalities. What if Box A weighs more than Box B, and that Box B weighs more than Box C? Could we still find out which is which?
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2013.png)
        
    - The answer turns out to be yes! So far, we have been able to solve this game with just two inequalities! Let's go ahead and try a third case scenario. Could we know which is which if Box A weighs less than Box B, but Box B weighs more than Box C?
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2014.png)
        
    - The answer turns out to be no. It turns out to have two possible orderings:
        - a: mouse, c: cat, b: dog (sorted order: acb)
        - c: mouse, a: cat, b: dog (sorted order: cab)
    - So while we were on a really great streak of solving the game with only two inequalities, we will need a third to solve all possibilities of the game. If we add the inequality a < c then this problem goes away and becomes solvable.
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2015.png)
        
    - Now that we've created this table, we can create a decision tree to help us solve this sorting game of cat and mouse (and dog).
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2016.png)
        
    - But how can we make this generalizable for all sorting? We know that each leaf in this tree is a unique possible answer to how boxes should be sorted.
    - So the number of ways the boxes can be sorted should turn out to be the number of leaves in the tree. That then begs the question of how many ways can N boxes be sorted? The answer turns out to be N! ways as there are N! permutations of a given set of elements.
    - So how many questions do we need to ask in order to know how to sort the boxes? We would need to find the number of levels we must go through to get our answer in the leaf.  Since it's a binary tree the minimum amount levels turn out $\log{N!}$ levels to reach a leaf. (Note that $\log$ means (log base 2)).
    - So, using this game we have found that we must ask at least $\lg{(N!)}$ questions about inequalities to find a proper way to sort it. Thus our **lower bound** for $R(N)$ is $\Omega{(\lg{(N!)})}$.
        
        ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2017.png)
        
- Sorting Optimality.
    
    ![Untitled](34%20-%20Sorting%20and%20Algorithmic%20Bounds%20117c574773ac47d0b64f70fc789b6b8d/Untitled%2018.png)
    

### Summary.

- [Here](https://cs61b-2.gitbook.io/cs61b-textbook/34.-sorting-and-algorithmic-bounds/34.4-summary).