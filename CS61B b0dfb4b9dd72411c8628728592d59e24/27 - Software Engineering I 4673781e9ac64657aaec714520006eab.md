# 27 - Software Engineering I

- Lecture is inspired by “A Philosophy of Software Design” by John Ousterhout.

### Complexity.

- The Power of Software and what makes it a different discipline that others.
    - Unlike other engineering disciplines, software is effectively unconstrained by the law of physics so **programming is an act of pure creativity**.
    - The limitation we may face is to understand what we’re building which means we’re luckily not concerned with any other limitation like other disciplines i.e. chemical engineering worrying about temperature or pressure for a chemical process.
- The Enemy is Complexity.
    - As mentioned earlier, biggest limitation is understanding the system we’re trying to build.
    - The more a program gets stuffed with features, the more complex it becomes.
    - Over time, it becomes a challenge for programmers to understand all the relevant pieces as they make future modifications.
    - Some tools like IDEs, Debuggers, Frameworks help to deal with complexity. However, our main goal is to keep our software **simple**.
- How to deal with complexity?
    - There’re two approaches:
        1. Making code simpler and more obvious 
        2. Encapsulation into modules
            - You can create or use modules by just knowing its API as if it’s a black box.
- What is complexity? What are the forms of complexity?
    - Quoting from Ousterhout: “Complexity is anything related to the structure of a software system that makes it hard to understand and modify the system.”
    - Forms of complexity:
        - Understanding how the code works.
        - The amount of time it takes to make small improvements.
        - Finding what needs to be modified to make an improvement.
        - Difficult to fix one bug without introducing another.
    - Note that complexity is not a synonymous with “large and sophisticated”. You may have short program that are quite complex.
    - In other words: “If a software system is hard to understand and modify, then it is complicated. If it is easy to understand and modify, then it is simple”.
- What is the difference between a complex system and a simple system?
    - In a complex system, takes a lot of effort to make small improvements.
    - In a simple system, bigger improvements require less effort.
- Impact of complexity (Fast Inverse Square Root example).
    - Complexity depends on how often a piece of a system is modified.
    - A system may have a few pieces that are highly complex but if nobody ever looks at that code, then it’s overall impact is minimal (only god knows how it works).
    - Some examples of that is writing a super efficient C code that works really good but literally no one can understand without having a background about what is had been written.
    - An example of that is [Fast inverse square root](https://en.wikipedia.org/wiki/Fast_inverse_square_root).
        
        ![Untitled](27%20-%20Software%20Engineering%20I%204673781e9ac64657aaec714520006eab/Untitled.png)
        
    - The entire Wikipedia article explains this little function over here ><.
    - Ousterhout’s book gives a crude mathematical formulation:
        - C = sum(c_p * t_p) for each part p.
            - c_p is the complexity of part p.
            - t_p is the time spent working on part p.

### Symptoms and Causes of Complexity.

- Symptoms of Complexity.
    - Ousterhout describes three symptoms of complexity:
        - **Change amplification**: A simple change requires modification in many places.
        - **Cognitive load**: How much you need to know in order to make a change.
            - Note: This is not the same as number of lines of code. Often MORE lines of code actually makes code simpler, because it is more narrative.
            - Incidentally, this is why I don’t like ++ or --!
        - **Unknown unknowns:** The worst type of complexity. This occurs when it’s not even clear what you need to know in order to make modifications!
            - Common in large code bases.
- How helpful an obvious system is?
    - In a good design, a system is ideally **obvious**.
    - In an obvious system, to make a change a developer can:
        - Quickly understand how existing code works.
        - Come up with a proposed change without doing too much thinking.
        - Have a high confidence that the change should actually work, despite not reading much code.
- How complexity grows?
    - Every software system starts out beautiful, pure, and clean.
    - As they are built upon, they slowly twist into uglier and uglier shapes. This is almost inevitable in real systems.
    - Each complexity introduced is no big deal, but: “Complexity comes about because hundreds or thousands of small dependences and obscurities build up over time.”
    - “Eventually, there are so many of these small issues that every possible change is affected by several of them.”
    - This incremental process is part of what makes controlling complexity so hard.
    - Ousterhout recommends a zero tolerance philosophy. This means that don’t write any code that anybody thinks is ugly.
    - This is the reason why code reviews exist.
- Causes of Complexity.
    - **Dependencies**: When a piece of code cannot be read, understood, and modified independently.
        - Example: Code that relies on super obscure libraries or the interaction of two super obscure libraries, then it can be complex.
    - **Obscurity**: When important information is not obvious.
        - Example: Naming our children `goodChild` and `badChild`make the code much easier to understand and reason about. Exploring Trees by using the distinction between a good child node vs a bad child node makes the code more obvious and less obscure.

### Strategic vs. Tactical Programming.

- What is Tactical programming?
    - Tactical Programming is about getting something to work, such as a new feature or a bug fix.
    - This may seem like a silly criticism. Clearly, working code is good.
- Problems of tactical programming?
    - You’re only focused on the correctness of the program and don’t spend enough time on the overall design.
    - As a result, you introduce tons of little complexities, e.g. making two copies of a method that do something similar.
    - Each individual complexity seems reasonable, but eventually you start to feel the weight.
        - Refactoring would fix the problem, but it would also take time, so you end up introducing even more complexity to deal with the original ones.
    - The end result is misery.
- What is strategic programming?
    - Quoting from Outerhout:  “The first step towards becoming a good software designer is to realize that **working code isn’t enough.**”
    - “The most important thing is the long term structure of the system.”
    - Adding complexities to achieve short term time gains is unacceptable.
    - Planning ahead may seem hard and mysterious for novice programmers, but it’s worth it for creating a long-living system
- Suggestions for Strategic Programming.
    - For each new class/task:
        - Rather than implementing the first idea, try coming up with (and possibly even partially implementing) a few different ideas.
        - When you feel like you have found something that feels clean, then fully implement that idea.
        - In real systems: Try to imagine how things might need to be changed in the future, and make sure your design can handle such changes.
- What do with mistakes made while applying Strategic programming?
    - No matter how careful you try to be, there will be mistakes in your design.
    - Avoid the temptation to patch around these mistakes. Instead, fix the design.
        - Example: Don’t add a bunch of special cases! Instead, make sure the system gracefully handles the cases you didn’t think about.
        - Specific example: Adding sentinel nodes to SLLists.
    - Indeed, it is impossible to design large software systems entirely in advance.
- ****Tactical vs. Strategic Programming Case Study (Ousterhout).****
    
    ![Untitled](27%20-%20Software%20Engineering%20I%204673781e9ac64657aaec714520006eab/Untitled%201.png)
    
    ![Untitled](27%20-%20Software%20Engineering%20I%204673781e9ac64657aaec714520006eab/Untitled%202.png)
    

### Exercises from CS61B Textbook.

- What are two ways to manage complexity?
    - **Make code simpler and more obvious**. Eliminate special cases and avoid repetition. Make code as general and parsimonious as possible.
    - **Encapsulate code into modules.** Every module should have a specific purpose. Programmers and users can subsequently use other modules in their design, without having to understand how these modules work.
- What is the difference between strategic and tactical programming? Which is better for managing complexity?
    - Tactical programming focuses on getting something to work, instead of focusing on overall design.
    - In contrast, strategic programming involves planning ahead and selecting from multiple possible implementations for the cleanest possible solution.
    - Strategic programming requires thinking about possible future changes and emphasizing code quality.
    - For managing complexity, strategic programming is more effective. Tactical programming introduces complexity with small fixes and patches over time, whereas strategic programming aims to eliminate these complexities by changing the underlying design.