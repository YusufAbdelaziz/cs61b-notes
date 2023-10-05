# Hashing

- Issues with previous data structures.
    - Well, we’ve looked at a few data structures for efficiently searching for the existence of items in the data structure.
    - We managed to make BST into balancing BST via 2-3 trees (LLRB).
    - Unfortunately, these data structures impose some limitations :
        - They require that items be **comparable**. How do you decide where a new item goes in a BST? You have to answer the question "are you smaller than or bigger than the root"? For some objects, this question may make no sense.
        - BSTs offer a complexity of $\Theta(\log{N})$. Is this good? Absolutely, however, we can do better !.
    - Now we need to find a way to implement Map and Set ADTs using other underlying data structure than BSTs.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled.png)
        
- A first attempt****: `DataIndexedIntegerSet`**
    - You can create a `DataIndexedIntegerSet` that stores booleans for each index.
    - Each index represents the value you’re storing. So in case you want to store 10, you go to index 10 and toggle the boolean to true.
        
        ```java
        public class DataIndexedIntegerSet {
            private boolean[] present;
        
            public DataIndexedIntegerSet() {
                present = new boolean[2000000000];
            }
        
            public void add(int x) {
                present[i] = true;
            }
        
            public boolean contains(int x) {
                return present[i];
            }
        ```
        
    - The running time for `add` and `contains` methods is constant $\Theta(1)$.
    - This approach has many drawbacks :
        - We’re wasting a lot of memory here, so we need about ~2 billion boolean to represent every possible integer. If we assume that a `boolean` takes 1 byte to store, the above needs `2GB` of space per `new DataIndexedIntegerSet()`. Moreover, the user may only insert a handful of items.
        - We need also to support other data types (objects) not just integers, thus generalization is needed.
- Generalizing the `**DataIndexedIntegerSet**` idea.
    - Now we’re trying to store any generic type and fetch it quickly ($\Theta(1)$)
    - In our `DataIndexedIntegerSet` class, suppose we want to add “cat”. Where should cat go to our list ? One way to think is to make the array index the same as alphabetic order indices.
    - This approach has two main problems :
        - Collisions with same words with same starting letter “car”.
        - We can’t store strings that start with numbers “455aabbcc”.
    - We can initially think of silly approach to avoid collision. We can use the same style of decimal representation. We have 27 number of English letters, hence our base is 27 unlike decimal (base 10). Recall that to represent number 7830, we do so as follows ⇒ $7830 = 7\times10^4+8\times10^3+3\times10^1+0\times10^0$.
    - So we can say that $"cat" =(3\times27^2+1\times27^1+20\times27^0) = 2234.$ Now we need to go to index number 2234 and store these three characters.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%201.png)
        
    - Using this method we can guarantee that each word is unique and has different value than any other word (same as decimal representation). **Note** ! we’re only talking about single lowercase words.
    - We need to create an `englishToInt` method that maps each English word into an index number.
    - Our data structure should look like this :
        
        ```java
        public class DataIndexedEnglishWordSet {
            private boolean[] present;
        
            public DataIndexedEnglishWordSet() {
                present = new boolean[2000000000];
            }
        
            public void add(String s) {
                present[englishToInt(s)] = true;
            }
        
            public boolean contains(int i) {
                resent present[englishToInt(s)];
            }
        		public static int letterNum(String s, int i) {
            /** Converts ith character of String to a letter number.
            * e.g. 'a' -> 1, 'b' -> 2, 'z' -> 26 */
            int ithChar = s.charAt(i)
            if ((ithChar < 'a') || (ithChar > 'z')) {
                throw new IllegalArgumentException();
            }
        
            return ithChar - 'a' + 1;
        		}
        
        		public static int englishToInt(String s) {
            int intRep = 0;
            for (int i = 0l i < s.length(); i += 1) {
                intRep = intRep * 26;
                intRep += letterNum(s, i);
            }
        
            return intRep;
        		}
        }
        ```
        
    - Recall, we started with wanting to :
        
        (a) Be better than $\Theta(\log N)$ We've now done this for integers and for single English words.
        
        (b) Allow for non-comparable items. We haven't touched this yet, although we are getting there. So far, we've only learnt how to add integers and English words, both of which *are* comparable, **but**, have we ever **used** the fact that they are comparable? I.e., have we ever tried to compare them (like we did in BSTs)? No. So we're getting there, but we haven't actually inserted anything non-comparable yet.
        
        (c) We have data structures that insert integers and English words. Let's make a quick visit to inserting arbitrary `String` objects, with spaces and all that. And maybe even insert other languages and emojis!
        
        (d) Further recall that our approach is still very wasteful of memory. We haven't solved that issue yet!
        
- Implementing `**DataIndexedStringSet`** with ASCII code.
    - Unfortunately, the previous approach wasn’t enough to store all possible English letters or symbols.
    - Therefore, we’re going to use ASCII representation. In case you don’t know what ASCII is, it’s a standard that maps each character to an integer value. They’re 128 possible character. Characters from 33 - 126 are “printable” characters so they don’t include white space or new line characters.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%202.png)
        
    - We can use the previous approach with 126 as the base. However, as you expected you’d get a gigantic values representing each word.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%203.png)
        
    - We can implement `asciiToInt` function to convert a string to its equivalent integer value.
        
        ```java
        public static int asciiToInt(String s) {
            int intRep = 0;
            for (int i = 0; i < s.length(); i += 1) {           
                intRep = intRep * 126;
                intRep = intRep + s.charAt(i);
            }
            return intRep;
        }
        ```
        
    - What if we want to support, for example, Chinese ? We’d have a larger table than ASCII called Unicode.
    - The largest possible representation is 40959, so we need to use that as the base. Here's an example:
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%204.png)
        
    - So... to store a 3 character Chinese word, we need an array of size larger than **39 trillion** (with a T)!. This is getting out of hand... so let's explore what we can do.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%205.png)
        
    - Things are going really messy, so we need to tackle this issue.
- Integer Overflow and Hash Codes.
    - Integer Overflow.
        - Well, the previous approach isn’t practical at all. Recall that we need 39 trillion to represent a handful amount of Chinese characters.
        - The largest possible integer in Java is 2,147,483,647. If you go beyond that limit, you overflow and you’d start back over at the smallest integer with is -2,147,483,647.
            
            ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%206.png)
            
        - Overflow results in collisions, one entry may equal to another one just as a coincidence of two overflow values are equal. This lead to unexpected behaviors of our data structure.
            
            ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%207.png)
            
    - Hash Code.
        - In computer science, taking an object and converting it into some integer is called "computing the **hash code** of the object". For instance, the hashcode of "melt banana" is 839099497.
        - We looked at how to compute this hashcode for Strings. For other Objects, there are one of two things we do:
            - Every Object in Java has a default `.hashcode()` method, which we can use. Java computes this by figuring out where the `Object` sits in memory (every section of the memory in your computer has an address!), and uses that memory's address to do something similar to what we did with `String`s. This methods gives a *unique* hashcode for every single Java object.
            - Sometimes, we write our own `hashcode` method. For example, given a `Dog`, we may use a combination of its `name`, `age` and `breed` to generate a `hashcode`.
        - Hash codes have three necessary properties, which means a hash code must have these properties in order to be **valid**:
            1. It must be an Integer
            2. If we run `.hashCode()` on an object twice, it should return the **same** number
            3. Two objects that are considered `.equal()` must have the same hash code.
        - Not all hash codes are created equal, however. If you want your hash code to be considered a **good** hash code, it should:
            1. Distribute items evenly
                
                **Note that at this point, we know how to add arbitrary objects to our data structure, not only strings.**
                
        - So we need a hash function to generate a hash code (integer value). So what is a hash function ?
        - From [Wolfram Mathworld website](https://mathworld.wolfram.com/HashFunction.html), A hash function $H$ projects a value from a set with many (or even an infinite number of) members to a value from a set with a fixed number of (fewer) members.
        - It’s important to note that hash functions are irreversible which means that given a hash code, you can’t “unhash” it to get the original value.
        - So we want to map our objects (may be an infinite number of objects) to limited integer number which is 4,294,967,296 ( in case you don’t have negative values).
        - Since we have an infinite set of objects, which is larger than the biggest integer in Java, **a collision MUST happen and they’re inevitable**.
        - Why we’re certain about collisions occurrence? Well, [Pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle) proves that.
        - The P**igeonhole Principle** states that if *n* items are put into *m* containers, with *n*> m, then at least one container must contain more than one item
        - If you have more than 4,294,967,296 objects, at least two objects would have the same hash code. For example, we have more than 4 billion plants each with different attributes, however, two of them will share the same hash code.
        - We have two fundamental challenges :
            - How do we handle collisions ?
            - How do we compute a good hash code for arbitrary objects ?
- Handling Collisions in a Hash Table - Separate Chaining.
    - One way to handle collisions is to create a bucket of N items that cause the collision.
    - So let’s say both “moo” and “Nep” have same hash code value which is 718.
    - We should head to position 718 in the array and create a bucket that stores these strings.
    - That bucket might be :
        - LinkedList.
        - ArrayList.
        - ArraySet.
        - Or anything else you want to use.
    - This method is called separate chaining but how does it really work ?
    - Each bucket in our array is initially empty. When we add an item X at index H, there’re **two possibilities** :
        - If bucket H is empty, we create a new list containing X and store it at index H.
        - If bucket H is already a list, we just add X to this list in case **it’s not already present**.
    - Note that it’s called separate chain because each index store a chain of items that share the same hash code.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%208.png)
        
    - So we managed to resolve the collision. However, our runtime is reduced into $\Theta(Q)$ where Q is the length of the largest chain (list) in the array. So we go to the correct index (hash code), and start a linear search to find whether the item already exists or not.
    - Same runtime goes with insertion. Why ? Because we have to make sure that the item is not in the list at the first place, so we need to run a linear search for `contains` method.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%209.png)
        
    - Also be aware that we needed to have a billion bucket just to store a two items. What should we do if we only have 10 buckets to store our items ?
    - You can use the modulo operator. So $bucket=hashCode\space\%\space arraySize$. Where arraySize is 10.
    - So for 1598, we will have 1598 % 10 which is 8 so we need to go to the 8th index to store a list containing bee and dog (dog should be stored in bucket 8 = 3328 % 8).
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2010.png)
        
    - A magnificent drawback is that each list might be very long.
- Hash Table creation with separate chaining.
    - We’ve just created a hash table using separate chaining by doing the following :
        - Data is converted by a **hash function** into an integer representation called **hash code**.
        - The **hash code** is then **reduced** to a **valid index**, usually using the **modulus operator** as we previous saw. However, in Java we need to handle negative hash codes before producing a valid index.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2011.png)
        
- Hash Table Runtime.
    - So far so good ? Well, I have two news for you :
        - The good one is that the previous approach saves a lot of memory and we can handle any arbitrary data.
        - The bad news is that the worst case runtime is now $\Theta(Q)$ where Q is the length of the longest list.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2012.png)
        
    - Given the five buckets like above, try to come out with the order of growth of Q (length of a list) with respect to N (total number of items inserted into the hash table).
        - Solution
            - The answer is $\Theta(N)$.
            - In the best case, all items are evenly distributed, the length of the longest list will be N/5.
            - While in the worst case, the length of the longest list will be N.
            - So it’s linear in both cases.
    - We also try to increase the number of buckets so to improve the overall running time. We keep a ratio called “load factor” between the number of items and the number of buckets. Whenever that load factor is beyond our predefined factor, then double the number of buckets just as a normal array list.
    - Let’s set N/M (N is number of items, M number of buckets, and the ratio is the load factor) bigger than of equal 1.5. Whenever we exceed this value, we double M.
    - We started by adding five items and we’ve reached 1.25 so far.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2013.png)
        
    - Let’s add another item.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2014.png)
        
    - Let’s double M now and we’ll have 8 buckets. Be aware that we’ll rehash the items once again so try to figure out where should each item go.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2015.png)
        
        - Solution.
            
            ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2016.png)
            
    - If the number of buckets grows linearly like the number of items inserted $M=\Theta(N)$, then $O(N/M) = O(1)$.
    - Therefore, the worst case runtime for all operations is $\Theta(N/M) = \Theta(1)$ with, of course, using the doubling strategy.
    - We haven’t discuss operations that resize the table, have we ?
    - Resizing takes $\Theta(N)$ because we have to redistribute all the items on the newly resized array.
    - Most add operations will be $\Theta(1)$ except ones that trigger resizing will need $\Theta(N)$.
    - We can say that the average runtime for add operation is still $\Theta(1)$ as long as we resize by a multiplicative factor.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2017.png)
        
    - We’ve been saying “evenly distributed”, but who should ensure such property ? Apparently, **the hash function.**
    - But first, let’s visit how hash tables in Java work.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2018.png)
        
- Hash Tables in Java.
    - Hash tables are the most popular implementation for both sets and maps.
    - Some of the advantages of hash tables include :
        - Great performance in practice.
        - Don’t require items to be comparable unlike (TreeSet or TreeMap)
        - Implementations often relatively simple.
        - Python dictionaries are just hash tables in disguise.
    - Java uses hash tables in `java.util.HashMap` and `java.util.HashSet`
    - The main question is → How does a `HashMap` determines an object’s hash code ?
    - Unlike trees, we don’t need to implement any interface. Recall that all objects inherit properties from `Object` class, hence we need to implement `hashCode` method to compute an object’s hash code.
    - For example, `String` objects have internal implementation of `hashCode` method.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2019.png)
        
    - We notice a couple of things :
        - Each String might have different hash code, and collision will occur as we explained with the Pigeonhole principle.
        - We may get negative hash code.
    - Well, the normal modulus operator might result in negative index values which gives index errors.
    - You then have to use `Math.floorMod(hashCode, numberofBuckets)` which prevents negative indexes. It works in round-robin style. So if you have 4 buckets and you got a hash code of -1, this should look to the last bucket which is number 3.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2020.png)
        
    - To sum up, In Java we convert data using `hashCode` method into an integer representation called a hash code.
    - A hash code is then reduced to a valid index using `Math.floorMod()` method we discussed earlier.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2021.png)
        
    - Two important warnings to consider :
        1. Never store objects that can change in a `HashSet` or `HashMap`
            - For example, you might have a set of lists. So changing the content of any list would result in different hash code which may result in items getting lost.\
        2. Never override `equals` method without also overriding `hashCode` method.
            - This leads items getting lost and generally a weird behavior.
            - `HashMap` and `HashSet` use `equals` to determine if an item exists in a particular bucket.
- Good hashing functions.
    - Our goal is to create a hash function that evenly distribute items on buckets.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2022.png)
        
    - Writing a good hash code is tricky but there’re some rules of thumb to follow.
    - An example of Java 8’s String `hashCode` method :
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2023.png)
        
    - Why 31 is used as a base ? wouldn’t 126 be a better base that covers all ASCII characters?
    - This is true in case of ignoring the overflow, but we can’t ignore it unfortunately.
        
        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2024.png)
        
    - A typical hash code base should be a ********************small prime******************** number.
        - Why prime ? even numbers introduce the overflow issue which we’ve just seen with base 126.
        - Why small ? it has lower cost to compute.
    
    ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2025.png)
    
    ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2026.png)
    
- Extra → Open Addressing.
    - Open Addressing
        - In open addressing, we're resizing our array more often to maintain the constant behavior of HT.
        - So we try to maintain a load factor (number of elements divided by the size of the table)  less than a specified value or threshold value
            
            ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2027.png)
            
        
        - When we inset key/value pair, the key is hashed and we therefore obtain an original position for where this key/value pair belongs to.
        - But in some cases, we might have same position for two key/value pairs, all we have to do is offsetting the original position until we find a position that is unoccupied.
        - Types of probing :
            1. Linear.
            2. Quadratic.
            3. Double.
            4. Pseudo random number generator.
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2028.png)
                
        - Example
            
            ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2029.png)
            
            - So we basically trying to find a new index by offsetting the old index repeatedly using a probing function.
            - BUT ! we might run into a cycle because of most randomly selected probing sequences modulo N would produce cycle shorter than the table size.
            - This would cause infinite loop when all buckets of the cycle are occupied.
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2030.png)
                
        - Linear Probing
            - In linear probing, our probing function is obviously a linear function. But some linear function produces cycles less than N (N is the number of slots)
            - For example :
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2031.png)
                
            - So we have to pick a constant a to make sure we make a full cycle. To do so, we need to make sure that both a and N are relatively prime. Two numbers are said to be relatively prime when their GCD (Greatest common denominator) is equal to one.
                
                Which means $GCD(a,N) = 1$. Hence our probing function can generate a full cycle and therefore we can find an empty bucket to insert our key/value pair.
                
        - Quadratic Probing
            - So what is QP?
            
            ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2032.png)
            
            - We still unfortunately face the cycle chaos, as quadratic functions are unable to produce a cycle of order N.
            - Then how to pick a proper probing function ?
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2033.png)
                
            - We'll pick  $P = (x^2 + x) / 2$ as our probing function.
            - Example
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2034.png)
                
                So you'll keep inserting depending on the hash value you get per key, if it's already occupied then you increment **x** value by one and you try to find new hash value using the probing function. In case of exceeding the threshold value, you'd need to resize your table and reinserting the elements.
                
        - Double hashing
            - So what is double hashing ?
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2035.png)
                
                - So in this case, we're multiplying a constant by another hash function.
                - Notice that $H_2(k)$  must hash the same type of key as $H_1(k)$ .
                - Double hashing reduces to linear probing, but the constant is known in runtime as we evaluate $H_2(k)$ at runtime obviously.
                
            - Note that since our DH is reduced to linear probing, you'd problem face cycle chaos once again. We'll learn how to resolve this in the example below.
            - Example
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2036.png)
                
                So we'll trying our trick we've used with linear probing by keeping the table size and the generated constant relatively prime, i.e. $GCD(N,a) = 1$.
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2037.png)
                
            - How to construct $H_2(k)$??
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2038.png)
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2039.png)
                
        - Removal in open addressing
            - An example illustrates the problem in removing an item in HT.
                - Suppose we have an empty hash table and we're using a linear probing function $P(x) = x$
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2040.png)
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2041.png)
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2042.png)
                
                Now let's jump into removing. 
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2043.png)
                
                So basically we'll keep probing like we were doing in insertion. So we'll start with looking for $k_2$ 
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2044.png)
                
                And we'll keep probing till we find our intended key.
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2045.png)
                
                And we'd replace it with null, the issue would arise once we try to query a key.
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2046.png)
                
                Oops, we've jumped into a null position which concludes that k3 has no existence, but it's already there. So looks like that the naïve method doesn't work !
                
            - The solution.
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2047.png)
                
                So now we can keep searching (probing) even if we hit a tombstone, we just skip it till we find the key we want.
                
            - Questions on tombstone
                
                ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2048.png)
                
                - What is a tombstone ?
                    - A tombstone is just a key/value pair, in which the key is a unique value or pointer and the value of that pair is null (tombstone, null).
                - How do we get rid of tombstone for optimization?
                    - We have two options.
                    - The first is we count tombstone is a filled slots, so when we exceed the specified load factor, we increase the table size and hence we remove tombstone by not copying them to the new table.
                    - The second thing is when we search for key, and we hit set of tombstones, we store the first tombstone we hit temporarily until find our key, then we place the slot of that key as a tombstone and the key/value pairs goes to the first tombstone we hit.
                    - Why we do this? basically when we want to search for the same key once more, we'll just find it from the first try.
                    - Example
                        
                        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2049.png)
                        
                        ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2050.png)
                        
                        Don't replace your initial key/value pair with null when doing the second option, replace them with a tombstone.
                        
                        Replacing them with null may cause a premature termination while looking up for a value in your table.
                        
- Summary
    
    ![Untitled](Hashing%20c386e557da604eaaa8d651e0f37a9cab/Untitled%2051.png)