# 39 - Compression, Complexity, and P=NP?

## ****Models of Compression.****

### Comparing Compression Algorithms.

- We discussed two models of compression:
    - Given bitstream, pass it to a compression algorithm that will produce a compressed bits. After that, you can give the compressed bits to the decompression algorithm which will produce the original bitstream.
    - Second method is to store the algorithm itself with the compressed bits. Using an interpreter, you can decompress the compressed bits to get the algorithm. Using the algorithm you can therefore decompress the compressed bitstream.
- We can use multiple methods of compression like Huffman or LZW.
- An interesting question pops up: For a given bitstream, what is the best algorithm for compression?
- We already proved that universal compression is impossible. One way to answer this questions is trying different standard tools and see which yields the smallest size:\
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled.png)
    
- One might argue, however, that the best possible compression algorithm for `mobydick.txt` is simply as follows:
    
    ```powershell
    D(B):
        if input == 0:
            return "Call me Ishamel. ...."
        else:
            return the text as is
    ```
    
- Using this as our decompression function, we can condense all of Moby Dick into a single bit!****
- However, there is a problem with this approach. If we include the code for the decompression algorithm as part of the compressed model (recall compression model 2 from the previous chapter), we see that Moby Dick is not compressed to one bit, but actually requires *more* bits than the original text! (because we’re basically hardcoding the text file inside the decompression algorithm).
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%201.png)
    

### Decompression Algorithms.

- Ultimately, the goal of a compression algorithm is to find short sequences of bits that generate desired longer sequences of bits. Formally stated, our problem is as follows:
- Given a sequence of bits `B`, find a shorter sequence `DA + C(B)` that produces `B` when fed into an interpreter. `DA` represents the bits for the decompression algorithm, while `C(B)` represents the compressed version of `B`.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%202.png)
    
- Using Huffman coding, you can reduce the number of bits as follows:
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%203.png)
    

### Model 1 vs. Model 2 Compression.

![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%204.png)

- Note that in model 2 we added the decompression algorithm with the compressed bits, so the pink numbers are the size of the algorithm encoded.

### Improving Compression.

- Recall the `HugPlant` example from the previous chapter. Using Huffman encoding, we can achieve a compression ratio of 25%.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%205.png)
    
- However, using another algorithm we'll call `MysteryX` for now, we can compress `HugPlant.bmp` down to 29,432 bits! This achieves a 0.35% compression ratio. Out of the $2^{8389594}$ possible bitstreams of length 8,389,5948,389,5948,389,594, only one in $2^{8360151}$ can achieve such a compression ratio.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%206.png)
    
- What is `MysteryX`? Well, it's simply the Java code `HugPlant.java` written to generate the `.bmp` file! Going back to the model of self-extracting bits, we see the power of code and interpreters in compression. This leads us to two interesting questions:
    - **comprehensible compression:** given a target bitstream `B`, can we create an algorithm that outputs useful, readable Java code?
        
        ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%207.png)
        
    - **optimal compression**: given a target bitstream `B`, can we find the *shortest* possible program that outputs this bitstream?
        
        ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%208.png)
        

## **Optimal Compression, Kolmogorov Complexity.**

### Kolmogorov Complexity

- We define the **Kolmogorov complexity** of a bitstream `B` to be the **shortest** bitstream $C_B$ that outputs **`B`.**
- Let the *Java-Kolmogorov complexity $K_J(B)$* be the shortest Java program that generates `B`.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%209.png)
    
- Note that for any bitstream $B$, $K(B)$ definitely exists. However, finding and proving $K(B)$ might be difficult or even impossible.

### Languages and Complexity.

- An important thing to note is that Kolmogorov complexity is language-independent.
- To run any program in one language in another, all I have to do is write an interpreter.
- For example, if I want to run a Python program that is not easily translatable to Java, I could instead just write a Java interpreter to read the text of the Python program and run it.
- In this case, $K_J(B) \le K_P(B) + I$, where $I$ is the length of the interpreter (a constant value).
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2010.png)
    
- This highlights a very deep fact about Kolmogorov complexity: most bitstreams are fundamentally incompressible no matter which language we choose for our compression algorithm.
- Consider a bitstream of 1,000,000 bits. Out of all compression algorithms possible, only 1 in $2^{4999999}$ bitstreams have a change of being compressed by more than 50% (499,999 bits or less).

### Uncomputability.

- Another important fact regarding Kolmogorov complexity is that it is impossible to compute. A proof of this fact is provided [here](https://en.wikipedia.org/w/index.php?title=Kolmogorov_complexity#Uncomputability_of_Kolmogorov_complexity).
- Practically, this means that it is **impossible** to write a "perfect" (optimal) compression algorithm, since we can't even compute the length of the shortest program!
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2011.png)
    

## Space/Time-Bounded Compression.

### Space-Bounded Compression.

- As described in the previous chapter, it is impossible to write the "perfect" compression algorithm that requires the fewest bits to output some bitstream $B$.
- However, what about the problem of space-bounded compression? In this problem, we take in two inputs: a bitstream $B$ and a target size $S$. The goal, then, is to find a program of length $\le S$ that outputs $B$.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2012.png)
    
- It turns out that such a problem is also uncomputable. If it were, then we could simply binary search on different values of S to find the optimal compression program size, which is impossible as shown in the previous section.

### Space-Time Bounded Compression, Can we be more efficient?

- What if we take our problem from above, and add a constraint that we can run at most $T$ lines of bytecode?
- It might seem unintuitive, but this kind of problem is actually solvable. We will use the following algorithm:
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2013.png)
    
    ```python
    for length L = 1....S:
        for each possible program P of length L:
            while (P is running && !(B is outputted) && lines_executed < T):
                run the next line of P
    ```
    
- The runtime of this algorithm is $O(T*2^S)$, and in the end, it will either output some program `P` that has the correct output and is bounded by $T$ and $S$, or return that no such program is possible.
- The runtime above is exponential in $S$. Thus, we might ask if it's possible to solve the space-time-bounded compression problem *efficiently*. As we'll see in the next chapter, this depends on our definition of efficiency.

## P = NP?

### Reductions.

- It turns out that space-time-bounded compression reduces to 3SAT, INDSET, LONGESTPATH, and many other hard problems. (The actual proof of such reductions is incredibly complex and omitted from this textbook).
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2014.png)
    
- The reason that space-time-compression can be turned into longest paths (or any other problem mentioned above) is that all these problems are part of a **complexity class** known as **NP**.
- **A property of such problem is that any NP problem can be reduced to any NP-complete problem, including longest paths**.
- In subsequent section, we will briefly cover what P, NP, and complexity classes are. However, most of these topics are far beyond the scope of this textbook or course, and would be better served by taking an upper-level algorithms course.

### P and NP.

- All yes/no problems can be divided into two main classes:
    - P: efficiently solvable problems.
    - NP: problems with solutions that are efficiently verifiable. This means that given an answer to the problem, you can efficiently check whether the answer is correct or not.
- Examples of problems in P include (**note that P is a subset of NP**):
    - Is this array sorted?
        - This can be solved by sorting the array using any sorting algorithm, and verified by checking that adjacent elements are increasing.
    - Does this array have duplicates?
        - This can be solved with a double for-loop, and verified in a similar manner.
- Examples of problems not in NP include:
    - Is this the best chest move I can make next?
        - There is no efficient way to verify that a chess move is indeed optimal, unless you draw out all possibilities for all subsequent moves.
    - What is the longest path?
        - This is not a yes-no question.

### Shocking Fact.

- Every single NP problem reduces to 3SAT.
    - This includes Bounded Space/Time Compression.
- In other words, **any decision problem for which a yes answer can be efficiently verified** can be transformed into a 3SAT problem.
    - This transformation is also “efficient” (polynomial time).

### Open Question in CS.

- Question posed Stephen Cook in 1971: Are all NP problems also P problems?
    - In other words, are all problems with efficiently verifiable solutions also efficiently solvable?
    - Often stated as “Does P = NP?”
- One reason to think yes:
    - Easy to check any given answer.
        - Maybe with the right pruning rules you can zero in on the answer?
- Note that this is an **unsolved problem**.
- Check CS170 for much more formal treatment of this problem.

### NP-Completeness.

- An unexpected property of NP problems is that every NP problem reduces to every NP-complete problem. This reduction is also "efficient", in that the problem can be transformed (pre-processed and post-processed) in polynomial time.
- This also means that solving any NP-complete problem essentially solves all problems in NP. As of today, there are tens of thousands of known NP-complete problems, but none of them have been solved yet.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2015.png)
    

### P = NP?

- An open question in computer science is whether P = NP; in other words, are all problems with efficiently verifiable problems (NP) also efficiently solvable (P)?
- One reason to suggest that P = NP might be true is that checking an answer is always efficient. Thus, given the right pruning, could we efficiently zero in on an answer?

### Does short = comprehensibility?

- We hoped to get a comprehensible compression.
- In case of the HugPlant we discussed earlier, this may be feasible but not necessarily holds for all short programs.
- So for example, if we fed some Stock Market data to the “Comprehensible Compression” algorithm, we should get some useful source code that helps in understanding how the world works.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2016.png)
    
- Turns out this is not true at all. Simple programs may surprisingly produce complex outputs.
- Consider this complex object, you can get short Java program but it won’t be easily understandable.
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2017.png)
    
    ![Untitled](39%20-%20Compression,%20Complexity,%20and%20P=NP%20fb4c72376cf54caa8a1b11876f486908/Untitled%2018.png)
    

## Exercises.

- Click [here](https://cs61b-2.gitbook.io/cs61b-textbook/39.-compression-complexity-p-np/39.5-exercises).