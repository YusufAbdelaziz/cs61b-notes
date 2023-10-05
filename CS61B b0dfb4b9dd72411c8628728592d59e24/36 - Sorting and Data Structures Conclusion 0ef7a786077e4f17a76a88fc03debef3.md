# 36 - Sorting and Data Structures Conclusion

## Radix vs. Comparison Sorting.

### Runtime of MergeSort when comparing between strings.

- Generally speaking, MergeSort requires $\Theta(N\log{N})$ compares.
- For strings of length W:
    - It’ll cost $\Theta({N\log{N}})$ if each comparison takes constant time. For example, strings are all different in top character.
    - It’ll cost $\Theta(WN\log{N})$ time in case each comparison takes $\Theta(N)$. This happens in case strings are equal or almost equal.
- To compare properly between RadixSort and MergeSort, the structure of strings should be taken in account.

### Which sorting algorithm to use?

- It depends.
- When might LSD sort be faster?
    - Sufficiently large N.
    - If strings are very similar to each other.
        - Each Merge Sort comparison costs Θ(W) time.
        
        ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled.png)
        
- When might Merge Sort be faster?
    - If strings are highly dissimilar from each other.
        - Each Merge Sort comparison is very fast.
        
        ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%201.png)
        

### Cost Model Analysis.

- As we discussed earlier in the course, picking a cost model is a good way to compare between algorithms.
- A **cost model** is a ****model used in the analysis of algorithms to define what constitutes a single step in the execution of an algorithm.
- The cost model is just an operation that may represent the runtime of an algorithm.
- The cost model of both algorithms is the number of characters being examined.
- By “examined”, we mean:
    - Radix Sort: Calling `charAt` in order to count occurrences of each character.
    - Merge Sort: Calling `charAt` in order to compare two Strings.

### MSD Radix Sort vs MergeSort (Runtime Hypothesis).

- Just to note, there’s no difference between MSD and LSD if strings are equal.
- Suppose we have **100 strings** of **1000 characters each**.
- For MSD Radix Sort, in the worst case (all strings equal), every character is examined exactly once. Thus, we have exactly 100,000 total character examinations.
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%202.png)
    
- For MergeSort, assume that equal items results in always picking left pointer while merging:
    - Comparing A[0] to A[50]: 2000 character examinations.
    - Comparing A[1] to A[50]: 2000 character examinations.
    - … Comparing A[49] to A[50]: 2000 character examinations.
    - Total characters examined: 50 * 2000 = 100000.
    - Merging N strings of 1000 characters requires N/2 * 2000 = 1000N examinations.
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%203.png)
    
    - In total, we must examine approximately 1000N log2 N total characters.
        - 100000 + 50000*2 + 25000 * 4 + … = ~660,000 characters.
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%204.png)
    
- So to summarize:
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%205.png)
    

### Empirical Study: Radix Sort vs. Comparison Sorting

- Computational experiment for W = 100.
- [MSD](https://algs4.cs.princeton.edu/51radix/MSD.java.html) and [merge sort](https://algs4.cs.princeton.edu/22mergesort/MergeX.java.html) implementations are highly optimized versions taken from our optional algorithms textbook.
- Does our data match our runtime hypothesis? No! Why not?
    - Maybe when MSD makes partitions, the divide and conquer cost is not accounted for.
    - Is MergeSort optimized to do the TimSort optimization? Unclear.
    - Maybe this is caching performance, Josh mentioned MSD has bad cache performance.
    - Maybe string comparison is not linear time somehow (keep in mind every string compare does the SAME thing. Repeated work on real machines these days tends to get optimized away).
    - Our cost model isn’t representative of everything that is happening.
    - One particularly thorny issue: **The “Just In Time” Compiler.**
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%206.png)
    

## JIT (Just-In-Time) Compiler.

### What is it and an example.

- Java’s Just-In-Time Compiler secretly optimizes your code when it runs.
- The code you write is not necessarily the code that executes!
- As your code runs, the “interpreter” is watching everything that happens.
    - If some segment of code is called many times, the interpreter actually studies and re-implements your code based on what it learned by watching WHILE ITS RUNNING (!!).
        - Example: Performing calculations whose results are unused.
        - See [this video](https://www.youtube.com/watch?v=oH4_unx8eJQ) if you’re curious.
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%207.png)
    
- Consider creating a thousand linked list objects for 500 iterations.
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%208.png)
    
- From the data we see here:
    - First optimization → JIT not sure what to do.
    - Second optimization → JIT stops creating linked lists since we’re not actually using them.

### Which one is better? MSD vs. MergeSort

- It highly depends on JIT. If it’s enabled, then JIT helps MergeSort to perform better unlike MSD.
- However, turning off JIT will result in making MSD faster than MergeSort (not that fast, though).
- Note that turning off JIT will make both sorting algorithm much worse and slower.
- What this tells us: The JIT was somehow able to massively optimize the compareTo calls.
    - Makes some intuitive sense: Comparing “AAA...A” to “AAA...A” over and over is redundant.
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%209.png)
    
- At the end, MergeSort wins since JIT is usually turned on.
- Many other possible cases to consider:
    - Almost equal strings (maybe the trick used by the JIT won’t work?).
    - Randomized strings.
    - Real world data from some dataset of interest.

### Bottom Line: Algorithms Can Be Hard to Compare.

- Comparing algorithms that have the same order of growth can be a difficult task, as it requires conducting computational experiments to determine their relative performance.
- However, modern programming environments can introduce additional challenges to this process, as certain optimizations such as just-in-time (JIT) compilation in Java can impact the results of experiments.
- It is worth noting that even small optimizations to an algorithm can have a significant impact on its performance.
- For instance, a change to the Quicksort algorithm suggested by Vladimir Yaroslavskiy has been shown to provide notable improvements, as discussed briefly in the Quicksort lecture.
- Therefore, when comparing algorithms with similar growth rates, it is crucial to remain vigilant of potential optimizations and variations that may influence their performance.

### JIT Compilers Are Always Evolving

- The JIT is a fantastically complex and important piece of code.
- Active area of research and development in the field of compilers.
- The old JIT compiler called C2 is so complicated that its code base is AFAIK being abandoned: “However, C2 has been delivering diminishing returns in recent years and no major improvements have been implemented in the compiler in the last several years. Not only that, but the code in C2 has become very hard to maintain and extend, and it is very hard for any new engineer to get up to speed with the codebase, which is written in a specific dialect of C++.” (from [this site](https://www.infoq.com/articles/Graal-Java-JIT-Compiler))
- If you like this stuff, try taking CS164 and maybe even try to get involved in compiler research.

## Radix Sorting Integers.

- As we previously discussed, estimating the performance between radix sort and comparison based sort is very hard and has some factors that affect the performance of an algorithm drastically.
- However, for a very large N limit, Radix sort is simply faster (linear time).
    - Treating alphabet size as constant, LSD Sort has runtime Θ(WN).
    - Comparison sorts have runtime Θ(N log N) in the worst case.
- We worked with strings and we extensively depended on `charAt` method. To make LSD Radix Sort work with integer, we need a similar way to extract a digit from each integer to decide how to sort them.
- This can be achieved in two ways:
    - Could convert into a String and treat as a base 10 number. Since maximum Java int is 2,000,000,000, W is also 10.
    - Could modify LSD radix sort to work natively on integers.
        - Instead of using `charAt`, maybe write a helper method like `getDthDigit(int N, int d)`. Example: `getDthDigit(15009, 2) = 5`.
    - Note that we’re assuming unsigned integers (non-negative).
- We don’t have to only work with base 10 numbers. We could also extend the base to support base 16, 256, or 65536 number.
- **Note that changing the base would result in a change in the number of digits.** So for example, consider this example of converting from decimal system to hexadecimal.
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%2010.png)
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%2011.png)
    
- Bottom line: The only important thing to learn here is that this conversion to higher bases may result in fewer digits for our LSD and MSD Radix sorts to have faster traversals.
- However, there is a tradeoff between W, the size of each integer, and the size of the array we need to maintain the counts. The most optimal is actually base 256!
    
    ![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%2012.png)
    

## Sorting Summary

![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%2013.png)

![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%2014.png)

![Untitled](36%20-%20Sorting%20and%20Data%20Structures%20Conclusion%200ef7a786077e4f17a76a88fc03debef3/Untitled%2015.png)

## Summary.

- Check it [here](https://cs61b-2.gitbook.io/cs61b-textbook/36.-sorting-and-data-structures-conclusion/36.4-summary).

## Exercises.

- [Here](https://cs61b-2.gitbook.io/cs61b-textbook/36.-sorting-and-data-structures-conclusion/36.5-exercises).