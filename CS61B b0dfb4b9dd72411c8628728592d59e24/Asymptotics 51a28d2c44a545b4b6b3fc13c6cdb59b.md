# Asymptotics.

- Important sums to know in the course.
    - What is the sum of $1+2+3+4+...+Q$ ?
        - It’s $\frac{Q(Q+1)}{2}$
        - This is called [sum of first natural numbers](https://en.wikipedia.org/wiki/1_%2B_2_%2B_3_%2B_4_%2B_%E2%8B%AF).
    - What is the sum of $1+2+4+8+...Q$ ?
        - It’s $2Q-1$.
        - It’s called [sum of first powers of 2](https://en.wikipedia.org/wiki/1_%2B_2_%2B_4_%2B_8_%2B_%E2%8B%AF).
- Does there exist a magic shortcut to analyze an algorithm ?
    - Usually no, but there’re some tricks of a master theorem that covers a variety of cases.
    - In algorithms analysis, we have the master theorem which analyzes the asymptotic behavior of divide-and-conquer algorithms.
    - But we might follow some general techniques to find a runtime of an algorithm :
        - Find exact sum (using one of the sums mentioned above).
        - Write out examples (like we did with N and $C(N)$).
        - Draw pictures (like we did in the geometric hint or plotting similar functions).
        
- Tree Recursion.
    - Consider first this function
        
        ```java
        public static int f3(int n) {
           if (n <= 1) 
              return 1;
           return f3(n-1) + f3(n-1);
        }
        ```
        
    - Try and think about the runtime.
        - Answer
            - It’s $2^{N}$.
        - Reasons
            - You can think of this algorithm in two ways.
            - The intuitive method.
                - You can try to change the argument **n** and each time you try to know the number of calls that happen to the function (this is our cost model).
                    
                    ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled.png)
                    
                - You can notice that the number of function calls is $2^{N-1}$  so the running time is $\theta({2^{N}})$.
            - The algebraic method.
                - You can try to find the sum of function calls.
                    
                    $$
                    C(N) =1+2+4+...+2^{N-1}
                    $$
                    
                - This is known from the previous picture of the previous method.
                - Now we can know the simple expression using sum of first powers of 2 which is $2Q-1$ where Q is $2^N$ so you’d get $2^N-1$.
                - So you’d also get a running time of $\theta({2^N})$.
            - Recurrence relation.
                - In this method, you can count the number of calls to f3 by finding an recurrence relation for C(N).
                - So if C(1) = 1.
                - Then C(N) = 2C(N-1) + 1.
                - Notice that this is out of 61B scope but here’s the remaining of the method.
                    
                    ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%201.png)
                    
- Back in 2006, a bug has been found in Java’s implementation of binary search.
    - The main problem was due overflow problem, so when you’re doing (high + low) / 2, it causes a bug in case you’re array is quite large (larger than $2^{30}$).
    - You can fix that by using >>> operator or other methods described [here](../CS61B%20b0dfb4b9dd72411c8628728592d59e24.md).
    - [What is the difference between >>> and >>.](https://stackoverflow.com/questions/19058859/what-does-mean-in-java#:~:text=The%20%3E%3E%3E%20operator%20is%20the,operand%2C%20or%20just%202%20here.)
- Binary Search nuances.
    - Recall that in binary search, you search for an element in a list of sorted items, you start by comparing the middle item with the item we’re looking for. If the item is smaller, then go to left, otherwise go to right.
    - Binary search runtime is not exactly $\log_2{N}$ as each step doesn’t cut it exactly in half, so if the array is of even length, there’s no middle and a round up/down is needed to be done and hence you might remove either a smaller or larger portion.
    - The exact number of binary search function calls (as it’s our cost model) would be
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%202.png)
        
    - So how did we know that? following this pattern would guide us to this formula.
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%203.png)
        
    - The overall runtime is (1 is insignificant).
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%204.png)
        
    - Since this looks somehow complex, we can simplify the runtime using these properties :
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%205.png)
        
    - So the simplified runtime is :
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%206.png)
        
    - There’s another approach to prove that the running time is $\theta(\log{N})$ which is recurrence relation, which we used to provide the running time of the f3 function which was mentioned in tree recursion section. That is, this approach is out of the scope of this course, so we’d omit it.
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%207.png)
        
- Merge Sort analysis.
    - You need to know two things, Selection sort and merging two arrays.
        - Selection Sort.
            - In selection sort, you loop over the array to find the minimum value, and place it at the beginning.
            - You repeat this process on the remaining array.
            - The running time is quadratic which is quite slow.
                
                ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%208.png)
                
        - How to merge two sorted arrays and the running time of doing so.
            - If you have two sorted arrays and you want to merge them into a single big sorted array, you can either append the two arrays and re-sort, or you can take advantage of the sorted state of each array (which is obviously a better approach to follow).
            - So the smallest element of our result array must be either one of the smallest elements of each array, so we need to compare the smallest of each arrays to know which one to choose for our result array. And we continue on the same way, but we continue from where we stopped. So if you chose the first element of the first array over the equivalent element in the other array, then the next comparison would be between the second element of the first array and the first element of the second array.
            - Keep repeating until you have no elements left, in case one of the arrays has been exhausted and there’re no comparison to do, then just copy the other array into our result array.
            - The running time would be linear as our writing operations to the result list take only a single operation to write a single element
    - The starter idea is to split array into two halves and start sorting them using selection sort and you can then merge the two. You’d get better running time which is $N+2*(\frac{N}{2})^2$ that is faster than pure $N^2$
    - You can keep splitting until you have a single item, after that you can return the recursion and start merging. However, you notice that sorting a single item doesn’t make any sense, so you’d just keep merging (and while you’re merging, you’re also sorting the subarrays) the subarrays all the way up until you come up with the large sorted array.
    - So merge sort steps are :
        - If the list is size 1, return. Otherwise:
        - Mergesort the left half
        - Mergesort the right half
        - Merge the results
    - So consider this example, in which you’re splitting the array into halves and you’re making layers all the way down. So each layer takes $O(N)$ for merging and you have $\log_2{N}$ layers to merge which gives you an overall running time of $N\log{N}$
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%209.png)
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2010.png)
        
    - Extra → Recurrence relation
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2011.png)
        
- Two level of nested loops doesn’t necessarily means the running time is quadratic, check the example to know that a detailed analysis of an algorithm is always important in some cases.
    - First example.
        - We have this two-nested loops and we want to know its runtime.
            
            ```java
            public static void printParty(int N) {
               for (int i = 1; i <= N; i = i * 2) {
                  for (int j = 0; j < i; j += 1) {
                     System.out.println("hello");   
                     int ZUG = 1 + 1;
                  }
               }
            }
            ```
            
        - First hint.
            - We can start thinking of the geometric approach, and plot the number of “hello” prints since it’s our cost model.
            - When N is 1, “hello” is printed only 1 time.
            - But when N is 2, “hello” is printed 3 times, because the outer loop would be executed twice. The first time prints “hello” for 1 time, and the second one prints “hello” for 3 times.
            - And we continue with this approach until N = 18
                
                ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2012.png)
                
        - Second hint.
            - We can also plot $0.5N$ and $4N$ as bounds to our function.
                
                ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2013.png)
                
            - We notice that our function (number of “hello” prints) is the staircase line) is between the upper dashed line (4N) and lower dashed line (0.5N).
            - Can you guess the running time ?
        - Third hint.
            - We can solve our function (number of hello statements prints) mathematically.
                
                $C(N)=1+2+4+...+N=2N−1\space (again\space if\space N\space is\space a\space power\space of\space 2).$ 
                
                For example, If N = 8 → 1 + 2 + 4 + 8 = 15 = 2*8 - 11+2+4+8=15=2∗8−1
                
            - The previous formula is called ***The sum of first powers of 2***.
            - Can you now know the runtime ?
        - Answer
            - Now it becomes obvious that the running time is $\theta{(N)}$.
            - You can deduce that analyzing an algorithm is not always obvious and easy, and requires careful thought
- Big-O vs Big Theta vs Big Omega.
    - Consider these two quizzes :
        - Quiz 1
            - Try to know the order of growth of this function :
            
            ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2014.png)
            
            - Answer
                - It’s $\theta(1)$ because there’s a bug since j = 0 would exit the function call since the first element equals to itself.
        - Quiz 2
            - Try with this one (similar with the previous but with the bug being fixed).
                
                ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2015.png)
                
            - Answer
                - It’d be something else (depends on the input to be precise). So if the the first and the second elements are the same, then it’d be $\theta(1)$, and if there’re no duplicate elements, then it’d cost $\theta(N^2)$.
    - From the previous quizzes, you need to be aware of what Big Theta expresses. It expresses the exact order of growth for runtime in terms of N (usually the input size).
    - If the runtime depends on other factors than just N, may need different Big Theta for every interesting condition. So in case of dup4 function, you need to specify each case individually (best case which is constant, and worst case which is quadratic).
    - You may just use big O notation and avoid qualifying our statements : The run time of dup4 is $O(N^2)$ which means in can be faster but it’s at worst quadratic.
    - So recall that Big O implies to “less than or equal” where as big theta implies to “equality only”.
    - Big theta is more informative than Big O. So for example saying mergesort is $O(N \log{N})$ is true but not as strong as “mergesort is $\Theta(N\log{N})$”.
    - Similar of saying “He ran a mile in 3 minutes and 35 seconds” which is more informative than “He ran a mile in less than 6 minutes”.
    - Big-O doesn’t mean “worst case”. You may have a function that its $\Theta(N)$ and you might classify it as $O(N^2)$ which is mathematically correct but its runtime in reality is linear. The following snippet prints an array’s length in constant time, but you can still classify this as quadratic (since big O means less than or equal).
        
        ```java
        public static void printLength(int [] arr){
        	System.out.println(arr.length);
        }
        ```
        
    - Despite the caveats, you can still use Big-O in conversations and interviews.
    - Some information that big theta offers might be useful but not necessary, so you can say “binary search takes $O(\log{N})$” rather than “binary search takes $\Theta(\log{N})$ in the worst case”, that is a little verbose.
    - Sometimes we don’t know the exact runtime, but we use Big O to provide us with an upper bound.
    - Big Omega.
        - Big Omega $\Omega$ can be thought as “greater than or equal”.
            
            ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2016.png)
            
        - This notation is useful to prove big theta runtime.
        - It's used to prove the **difficulty of a problem**. For example, ANY duplicate-finding algorithm must be $\Omega(N)$, because the algorithm must at least look at each element.
    - Summary :
        
        ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2017.png)
        
    
- Performance Definition.
    
    ![Untitled](Asymptotics%2051a28d2c44a545b4b6b3fc13c6cdb59b/Untitled%2018.png)