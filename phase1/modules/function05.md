Chapter 5 — Hoisting Deep Dive
1. What is Hoisting?
2. Hoisting with var





1. What is Hoisting?
    Hoisting is JavaScript's default behavior of allocating memory for variables and functions during the memory creation phase before code execution.


    Before running code:
        JavaScript first stores variables and functions in memory.
        This behavior is called: Hoisting
    Mental Model
        Phase 1:
        Prepare memory

        Phase 2:
        Execute code

    Hoisting happens during:
        Memory Creation Phase

    MOST IMPORTANT UNDERSTANDING
        JavaScript does NOT do:
            Move code upward

        Instead JS does:
            Allocate memory before execution

        VERY IMPORTANT.

2. Hoisting with var
    Example
        console.log(a)
        var a = 10
        Output: undefined

    WHY?
    During memory phase:
        JS creates:
            a → undefined

    Then execution phase starts.

    Execution Timeline
        Memory Phase
            a → undefined
        Execution Phase
            console.log(a)

        prints: undefined

    because assignment not happened yet.

    IMPORTANT
        ONLY declaration is hoisted:
        var a

        NOT assignment:
        = 10  

    Mental Rewrite

        Conceptually JS behaves like:
            var a
            console.log(a)
            a = 10

        NOT exact internal implementation,
        but useful mental model.      

3. Function Hoisting
    Functions hoist differently.

    Example
        hello()

        function hello() {
        console.log("Hi")
        }

    Output: Hi

    WHY?

    During memory phase:

    hello → complete function object

    NOT:
    undefined

    Visual Memory
        Memory:
        hello → function() {
            console.log("Hi")
        }

    So function already exists before execution starts.

    BIG DIFFERENCE
        | Type                 | Memory Phase  |
        | -------------------- | ------------- |
        | `var`                | `undefined`   |
        | function declaration | full function |

4. Function Expression Hoisting

    Now:
        hello()
        var hello = function() {
            console.log("Hi")
        }

    Output:
    TypeError: hello is not a function

    WHY?

    During memory phase:

        hello → undefined

    Because:
        hello is variable.

    NOT function declaration.


    Execution Flow
        Memory Phase
            hello → undefined
        Execution Phase
            hello()

        means:
            undefined()

        ❌ Error

    Later assignment happens:
    hello = function(){}


    VERY IMPORTANT DIFFERENCE
    Function Declaration
        function hello(){}

    Memory:
        full function object


    Function Expression
        var hello = function(){}

    Memory:
        undefined

    Interview Question
        Why does function declaration work before definition but function expression does not?

        Answer:
            Function declarations are fully hoisted with their function object, while function expressions behave like variables and only their declaration is hoisted.

5. let and const Hoisting

    This is where MANY developers become confused.

    Example
        console.log(a)
        let a = 10

        Output:  ReferenceError

    Many beginners think:
        let is not hoisted

    WRONG.
        let and const ARE hoisted.

    But differently.

    VERY IMPORTANT

    let and const are:
        hoisted but uninitialized

    Memory Phase

        Conceptually:
            a → uninitialized

        NOT:
            undefined

    This special period is called:
    Temporal Dead Zone (TDZ)

6. Temporal Dead Zone (TDZ)
    Temporal Dead Zone is the time between entering scope and variable initialization where let and const variables cannot be accessed.


    TDZ means:
        Variable exists in memory
        BUT cannot be used yet.

    Example
        console.log(a)
        let a = 5

        Memory Phase
            a → uninitialized
        Execution Phase
            console.log(a)

    JS says:
        "You cannot access before initialization."
        ReferenceError.

    Then:
        a = 5

    TDZ ends.

    Visual Timeline
        Scope Starts
            ↓
        TDZ Starts
            ↓
        let a
            ↓
        Initialization
            ↓
        TDZ Ends

7. Why TDZ Exists?

    VERY IMPORTANT.

    TDZ was introduced to:
        prevent bugs
        force cleaner code
        avoid accidental usage before initialization
    
    Example Problem with var
        if(true) {
            var x = 10
        }
        console.log(x)

        Output: 10

    Unexpected behavior.

    let fixes this:
        if(true) {
            let x = 10
        }
        console.log(x)

    ❌ Error

    Safer behavior.

8. const Hoisting

    Same as let.

    Example:
        console.log(a)
        const a = 10
        ❌ ReferenceError

    Because:
        hoisted
        but inside TDZ

    Important Difference
        const MUST initialize immediately.

    WRONG:
        const a

    ❌ Error

    Why?

    Because const means:
        constant binding


9. Hoisting Comparison Table
    | Type                 | Hoisted?            | Initial Value           | Before Initialization  |
    | -------------------- | ------------------- | ----------------------- | -----------------------|
    | var                  | ✅ Yes               | undefined               | accessible            |
    | let                  | ✅ Yes               | uninitialized           | TDZ error             |
    | const                | ✅ Yes               | uninitialized           | TDZ error             |
    | function declaration | ✅ Yes               | full function           | callable              |
    | function expression  | depends on variable | undefined/uninitialized | error                   |

10. Internal Engine Thinking

    Example:
        var a = 10
        function hello() {}
        let b = 20

    Memory Phase

        Conceptually:

        a → undefined

        hello → full function object

        b → uninitialized

    Execution Phase
        a = 10
        b = 20


11. Common Beginner Mistakes

    Mistake 1:
        Thinking hoisting means:
        moving code upward
        WRONG.

    Mistake 2:
        Thinking let and const are not hoisted.
        
        WRONG.

        They ARE hoisted.
        But inside TDZ.

    Mistake 3:
        Confusing:
            undefined

        with:
            ReferenceError

        var
            undefined

        let/const
            ReferenceError

        BIG difference.

    Mistake 4:
        Thinking function expressions behave like function declarations.
        WRONG.

    Avoid var

    Because:

        function scope confusion
        hoisting bugs
        unexpected behavior

        Modern JS rarely uses var.


Interview Questions
    Q1. What is hoisting?

    Q2. Difference between hoisting of:
        var
        let
        const

    Q3. What is TDZ?

    Q4. Why does var return undefined but let throws error?

    Q5. Why do function declarations work before definition?

    Q6. Difference between:
        function declaration hoisting
        function expression hoisting


