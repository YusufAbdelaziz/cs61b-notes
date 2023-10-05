# 31 - Software Engineering II

### Complexity II.

- What is complexity?
    - Complexity is anything related to the structure of a software system that makes it hard to understand or modify the system.
- Complexity of a code snippet.
    - Could you tell why this snippet of code is complex?
    - This code is for a world in which player moves around obstacles (walls) to reach a goal.
        
        ![Untitled](31%20-%20Software%20Engineering%20II%206027ee1b0ead45ec8d62a733b5d04cc5/Untitled.png)
        
        - Answer
            - Naming → What does xxcenter refers to? xPosition could be a better naming.
            - Repeated code → There’s repeated calculation of what it means to go UP, lEFT, DOWN, etc.
            - Hard coded features:
                - Only up/down/left/right are possible.
                - When you leave, there’s only floor. The only obstacle is a WALL.
            - Possible changes →
                - You may have a method that given an obstacle type (Wall, Lava, etc), it tells you whether it’s possible to proceed or not.
                - You can also add separate methods for moving directions (left, right, up, down).
                - You can use a `switch` statement rather than if statements to make things look nicer.
                - Simplify the code that calculates the up, left, right, or down position.

### Sources of Complexity.

- What were the two sources of complexity according to Ousterhout?
    - **Dependencies**: When a piece of code cannot be read, understood, and modified independently.
    - **Obscurity**: When important information is not obvious.

### Modular Design.

- What does Modular Design really mean? What is our goal when we utilize modular design?
    - Modular design is to design a system by breaking it down into modules, where every module is totally independent.
    - Our goal is to minimize dependencies between modules.
- What does a module mean?
    - A module may informally refer to a class, a package, or other unit of code.
- Why it is not possible for a module to be entirely independent?
    - Because code from each module has to call other modules (i.e. need to know signature of methods to call them).
- What is the difference between an interface and an implementation?
    - As we discussed throughout the course, an interface defines what an ADT should do (operations). An implementation defines how these operations should be implemented to achieve the intended functionality.
    - A `Map` is an interface where as `HashMap`, `TreeMap`, etc. are all implementations.
- According to Ousterhout: “The best modules are those whose interfaces are much simpler than their implementation.” Why?
    - A simple interface **minimizes** the complexity the module can cause elsewhere. If you only have a `getNext()` method, that’s all someone can do.
    - If a module’s interface is simple, we can change an implementation of that module without affecting the interface.
        - Silly example: If List had an `arraySize` method, this would mean you’d be stuck only being able to build array based lists.
- In Java, an interface has both formal and informal parts. Discuss.
    - **Formal**: The list of method signatures. You’d get a compiler error in case you give a method more or less arguments than required.
    - **Informal**: Rules for using the interface that are not enforced by the compiler.
        - Example: If your iterator requires `hasNext` to be called before `next` in order to work properly, that is an informal part of the interface.
        - Example: If your add method throws an exception on null inputs, that is an informal part of the interface.
        - Example: Runtime for a specific method, e.g. add in `ArrayList`.
        - Can only be specified in comments.
- According to Ousterhout: “The best modules are those that provide powerful functionality yet have simple interfaces. I use the term *deep* to describe such modules. The most important way to make your modules deep is to practice “information hiding”. What does that mean?
    - A deep module is a module that has a simple interface and powerful functionality.
    - To keep a module deep, you should embed knowledge and design decision in the module itself, without exposing them to the outside world.
    - Keeping sensitive information and helper methods `private` will extensively help in information hiding and abstracting the complexity of a module.
- Information Leakage.
    - It’s the opposite of information hiding.
    - Occurs when a design decision is reflected in multiple modules. This means that any change to one requires a change to all.
    - Ousterhout:
        - “Information leakage is one of the most important red flags in software design.”
        - “One of the best skills you can learn as a software designer is a high level of sensitivity to information leakage.”
- Causes of Information Leakage: Temporal Decomposition.
    - In temporal decomposition, the structure of your system reflects the order in which events occur.
    - After thinking for a feature (let’s call it feature A) and then you start implementing it, you then do the same for another feature (let’s call it feature B).
    - You may find some commonalities between feature A and feature B that can be extracted to avoid information leakage. Without extracting, you may need to make multiple changes for common code.
        
        ![Untitled](31%20-%20Software%20Engineering%20II%206027ee1b0ead45ec8d62a733b5d04cc5/Untitled%201.png)
        

### Teamwork.

- What is reflexivity?
    - Quoting from [teamingxdesign](http://www.teamingxdesign.com) website “A group’s ability to collectively reflect upon team objectives, strategies, and processes, and to adapt to them accordingly.”
    - Recommended that you “cultivate a collaborative environment in which giving and receiving feedback on an ongoing basis is seen as a mechanism for reflection and learning.”
        - It’s OK and even expected for you and your partner to be a bit unevenly matched in terms of programming ability.
- Feedback.
    - Sometimes we receive feedback that makes our day and cheers us up. Sometimes we receive criticism which feel judgmental or in bad faith. Thus we personally sometimes fear giving feedback in fear that our feedback is misconstrued as an attack or taken offensively.
    - Please do not take negative feedback to heart.
    - Please always remember feedback is about listening actively and being patient and then responding by improving what can be fixed.

### Exercise

- [Here](https://cs61b-2.gitbook.io/cs61b-textbook/31.-software-engineering-ii/31.5-exerises).