# 38 - Compression and Complexity.

## Introduction to Compression.

- You might’ve compressed a file using Winrar or CLI using `zip`
- This results in compressing the file into smaller version with `.zip` extension at the end.
    
    ```powershell
    $ zip mobydick.zip mobydick.txt 
    adding: mobydick.txt (deflated 59%)
    
    $ ls -l
    -rw-rw-r-- 1 jug jug 643207 Apr 24 10:55 mobydick.txt
    -rw-rw-r-- 1 jug jug 261375 Apr 24 10:55 mobydick.zip
    ```
    
- Note how the file size changes.
- In this lecture, we’re concerned with algorithms and data structured used in compression and how do they work which will lead to a closing discussion of complexity theory.
- We’ll consider a compression model to understand how does compression process works.
- In our first model of compression, we consider compression as applying a *compression algorithm* on a sequence of bits.
- To reverse the compression, we apply the inverse *decompression algorithm.*
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled.png)
    
- Say you had a text file called `example.txt`. If you were to zip that text file, you'd get `example.zip`, a zip file with a size much lesser than the original `example.txt` file. This is the main idea behind compression--a technique used to reduce file size.
- Then, if you were to unzip `example.zip` into a file called `unzippedexample.txt`, you would notice no difference between `example.txt` and `unzippedexample.txt.` This is an indicator of **lossless** compression, where no information is lost.

## Prefix-free Codes.

### Using Morse Code as Compression Method.

- In Java, each string is represented as an array of characters where each character is 8 bit long.
- For example, dog can be represented as:
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%201.png)
    
- One easy way to compress, then, is to simply use less than 8 bits per character. To do this, we have to decide which **codewords** (bit sequences) go with each **symbol** (character).
- Morse code can be used as a method to map alphanumeric symbols to codewords.\
- Looking at the alphabet below, can you guess what does the sequence – – • – – • represent?
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%202.png)
    
- Unfortunately, it’s ambiguous. The same sequence of symbols can represent either MEME, or GG, depending on what you choose – – • to represent.
- In real usage, operators must pause between codewords to indicate a break. The pause acts as an implicit third symbol, but we can't encode this real-time information into our code.
- **Alternate strategy: Avoid ambiguity by making code *prefix free*.**

### Prefix-free Codes

- An alternate strategy to avoid the need for real-time is to use **prefix-free codes**. In a prefix-free code, no codeword is a prefix of any other. In the Morse Code example, there would be no confusion whether the – – in the pattern – – • – – • is supposed to represent M, or the start of G. In other words, M is a prefix of G.
- Let's represent Morse code as a tree of codewords leading to symbols. As we can see from the tree, several symbols have representations that are prefixes of other symbols.
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%203.png)
    
- As an example of an (arbitrary) prefix-free code, consider the following encoding:
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%204.png)
    
- The following code is also prefix-free:
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%205.png)
    
- Note that some codes are more efficient for certain strings than others: in the first representation, `I ATE` uses less bits than the second code. However, this is highly dependent on what string we're trying to encode.
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%206.png)
    

## Shannon-Fano Codes.

- Shannon-Fano codes are an approach to create prefix-free codes based on a set of symbols/characters and their probabilities. The main idea is that we want shorter prefix-free codes for more popular characters, and longer codes for lesser used characters.
- The algorithm is:
    - Count relative frequencies of all characters in a text.
    - Split into ‘left’ and ‘right halves’ of roughly equal frequency.
        - Left half gets a leading zero. Right half gets a leading one.
        - Repeat.
- Check slide 14 [here](https://docs.google.com/presentation/d/17l72hesAQEv9vPsHakJRDXmP8T5cipOMh-_8V2M7tmY/edit#slide=id.g536bf0065_0150) to see how it works visually.
- At the end, you will get a tree as shown below, with shorter paths for characters with a higher frequency, and longer paths for characters with a lower frequency.
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%207.png)
    
- Note that Shannon-Fano coding is NOT optimal, so it is not used very often.

## Huffman Coding Conceptuals.

### Core Idea

- The story of how Huffman created his own prefix-free coding method is [here](http://www.huffmancoding.com/my-uncle/scientific-american).
- Huffman coding takes a bottom-up approach to prefix-free codes, as opposed to the top-down approach taken by Shannon-Fano codes. The algorithm is as follows:
    - Calculate relative frequencies.
    - Assign each symbol to a node with weight = relative frequency.
    - Take the two smallest nodes and merge them into a super node with weight equal to sum of weights.
    - Repeat until everything is part of a tree.
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%208.png)
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%209.png)
    
- After merging all nodes to get a tree structure, we’ll give the left node number 0 and right node number 1.

### Data Structures Used.

- Let's now think about the data structures we would use for the encoding and decoding processes of the Huffman Coding process. Recall that encoding will translate symbols to code words and decoding will do the opposite. An example is as follows:
    - Encoding translates `I ATE` into `0000011000100101.`
    - Decoding translates `0000011000100101` into `I ATE`.
- So for encoding, which data structure should we use?
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2010.png)
    
    - Answer
        - A `HashMap` or an array of `BitSequence` both are similar.
            
            ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2011.png)
            
- How about decoding?
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2012.png)
    
    - Answer.
        
        ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2013.png)
        
        ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2014.png)
        
        ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2015.png)
        
        ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2016.png)
        

### Huffman Coding Usage Philosophies.

- Now that we have talked about what Huffman Coding is, let's talk about some practical issues that arise when we want to use it. There are two main philosophies we can use.
- Corpus.
    - For each input type (English text, Chinese text, images), assemble huge numbers of sample inputs for each category. Use corpus to create a standard code for English, Chinese, etc.
    - A corpus is a collection of pieces of language to be used as a sample of the language. Below is an example where we specify that we want to use the `ENGLISH` corpus to compress `mobydick.txt`
        
        ```powershell
        $ java HuffmanEncodePh1 ENGLISH mobydick.txt
        ```
        
    - **Problem:** Suboptimal encoding, which means our corpus does not exactly match our input. What this means in the context of this example is that `mobydick.txt` might not match up well with the general frequencies of `ENGLISH` texts and instead might have some other quirks or specifications from the author.
- Unique Code.
    - For every possible input file, create a unique code just for that file. Then, when someone receives that file, they will know how to decode it using the code we send along the compressed file.
    - As seen in the example below, we do not specify a corpus and we send along another file to help the decoding process for that specific file.
        
        ```powershell
        $ java HuffmanEncodePh2 mobydick.txt
        ```
        
    - **Problem**: This approach requires us to use extra space for the codeword table in the compressed bitstream.
    - **However**, this generally works better than the corpus philosophy so is used commonly in the real world.
- Approach 2 → Unique Code is what people generally use.
- Live demo is of compression is [here](https://docs.google.com/presentation/d/1DWuSkE9MxQPUTjbSJCMe54rCim4eAwM4aFRvhqq5_Hs/edit#slide=id.g2159afc5e6_0_290) and decompression is [here](https://docs.google.com/presentation/d/1x7WXK5-X0bvxk6Q1IBuYXGibZzyRDgr8IIb30YiR4iU/edit#slide=id.g21592b0f8d_0_0).

### Summary

![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2017.png)

- See [Huffman.java](http://algs4.cs.princeton.edu/55compression/Huffman.java) for an example implementation on 8-bit symbols.

## Compression Theory.

### Compression Ratios.

- The goal of data compression is to reduce the size of a sequence of data while retaining as much information as possible. For example, the letter `e` appears more frequently in the English dictionary than `z`, so we would want to represent with smaller bits.
- **Compression ratio** is a measure of how much the size of the compressed data differs from the original data.
    - **Huffman Coding** is a compression technique that represents common symbols with smaller numbers of bits, resulting in a more efficient encoding.
    - **Run-length encoding** is another compression technique that replaces repeated characters with the character itself and the number of times it occurs. XXXXXXXXXYYYYXXXXX -> X10Y4X5
    - **LZW** is a compression technique that searches for common repeated patterns in the input and replaces them with a shorter code.
- The general idea behind most compression techniques is to **exploit any existing redundancy** or order within the sequence to reduce the size of the data.
- However, if a sequence has no existing redundancy or order, compression may not be possible.

### Self-Extracting Bits.

- Self-extracting bits is a compression technique that wraps the compressed bits and the decompression algorithm into a **single sequence of bits**.
- The goal is to simplify the compression and decompression process by combining the two steps into one. Self-extracting bits can be used to create executable files that can be run on any system with an interpreter (e.g. Java interpreter).
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2018.png)
    

### HugPlant Example

- To compress an image file like `hugplant.bmp`, we can break it into 8-bit chunks and Huffman encode each chunk.
    
    ![Untitled](38%20-%20Compression%20and%20Complexity%20fbbed43d7bd34121b6cfdc0d07d66bab/Untitled%2019.png)
    
- We package the compressed data plus decoder into a single self-extracting `.java` file, represented as a `byte[]` array.
- When the `byte[]` array is passed to an interpreter, the interpreter executes the Huffman decoding algorithm and produces the original `hugplant.bmp` image file.
- The size of the compressed bitstream and the Huffman decoding algorithm combined is **smaller** than the original image file, making it more efficient to store and transmit.
- However, the receiver of the compressed file must have access to the appropriate interpreter to decode and reconstruct the original image.

## LZW Compression.

### Key Idea

- The LWZ approach is based on the idea of exploiting redundancy and patterns in the input data to achieve compression. Basically, each codeword can represent multiple symbols.
- For example, imagine a sequence of symbols `ABCABCA`. In traditional compression, each symbol would be mapped to a fixed-length codeword, resulting in a compressed sequence like `010001001000100100`.
- With the LWZ approach, the codewords can be based on patterns in the input data. In this case, the algorithm might start with a codeword table. (A --> 0, B --> 1, C --> 2). This could result in a compressed sequence looking something like `01201201`. By allowing for codewords that can represent multiple symbols, the LWZ approach can achieve more efficient compression than traditional approaches.

### Algorithm

- The algorithm starts with a simple codeword table where each codeword corresponds to a single symbol.
- Whenever a codeword is used, a new codeword is created by concatenating the previous codeword with the next symbol.
- The algorithm does not specify what happens when the codeword table becomes full, but there are many variants of the algorithm that handle this differently.
- A neat fact about the LWZ approach is that it is possible to reconstruct the codeword table from the compressed bitstream alone, without needing to send the table along with the compressed data.
- LWZ decompression [demo](https://docs.google.com/presentation/d/1U8XO6CWfcU4QgrFOZmGjAgmaKxLc8HXk6qB1JQVlqrg/edit#slide=id.g53705ba95_0259).

### Fun Facts

- The algorithm is named after its inventors, Lempel, Ziv, and Welch.
- The LWZ algorithm is used as a component in many compression tools, including .gif files, .zip files, and more.
- The LWZ algorithm was once controversial due to attempts to enforce licensing fees, but the patent expired in 2003.

## Summary.

- Check [here](https://cs61b-2.gitbook.io/cs61b-textbook/38.-compression-and-complexity/38.7-summary).

## Exercises.

- Click [here](https://cs61b-2.gitbook.io/cs61b-textbook/38.-compression-and-complexity/38.8-exercises).