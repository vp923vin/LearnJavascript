Chapter 3 — Execution Context Deep Dive

1. What is Execution Context?
2. Types of Execution Context
3. Two Phases of Execution Context
4. Memory Creation Phase
5. Why Variables Become undefined
6. Why Function Declarations Work Before Definition
7. Variable vs Function Hoisting
8. Function Execution Context
9. Call Stack
10. Stack Overflow
11. Internal JavaScript Engine Thinking
12. Important Realization




1. What is Execution Context?
    Execution Context is the environment in which JavaScript code is evaluated and executed.

    Think of execution context like:
        A temporary box/workspace
        where JavaScript executes code.

    Inside this box, JS stores:
        variables
        functions
        scope information
        this
        execution details

    VERY IMPORTANT UNDERSTANDING
        JavaScript does NOT run code line-by-line directly.

        Before execution:
            JS first prepares memory and environment.
            Then executes code.
            This preparation process is execution context creation.

2. Types of Execution Context
    There are mainly 2 types.

    - Global Execution Context (GEC)
        Created when JS file starts running.
        There is ONLY ONE global execution context.

    - Function Execution Context (FEC)
        Created every time a function is called.
        Every function call creates NEW execution context.

    Visual Representation
        Program Starts
            ↓
        Global Execution Context Created
            ↓
        Function Called
            ↓
        Function Execution Context Created
    
    Example:
        var a = 10
        function test() {
            var b = 20
        }
        test()
    
    Execution contexts:
    - Global Execution Context
    - Function Execution Context for test()


3. Two Phases of Execution Context

    This is EXTREMELY important.
    Every execution context has TWO phases:

    Phase 1 → Memory Creation Phase
        Also called:
            Creation Phase
            Hoisting Phase
        JS scans code and allocates memory.

    Phase 2 → Execution Phase
        JS executes code line by line.


4. Memory Creation Phase
    During this phase:

    JavaScript:
        allocates memory for variables
        stores function declarations
        sets initial values
        creates scope chain
        sets up this

    BUT:
        code does NOT execute yet


    var a = 10
    function greet() {
        console.log("Hello")
    }
    console.log(a)

    During Memory Phase
    Conceptually:
        Memory:
            a → undefined
            greet → full function object

    VERY IMPORTANT:
        variables get undefined
        function declarations store FULL function

        This is hoisting foundation.


    Then Execution Phase
    Now JS runs code:

    a = 10
    Then:
    console.log(a)

    Outputs: 10

5. Why Variables Become undefined

    BIG interview topic.

    Example:
    console.log(a)
    var a = 5
    Output: undefined

    Why?
    Because during memory phase:
    a → undefined

    Then execution phase later assigns:
    a = 5

6. Why Function Declarations Work Before Definition

    hello()

    function hello() {
        console.log("Hi")
    }

    Works because during memory phase:
    hello → complete function object
    already stored.



7. Variable vs Function Hoisting

    VERY IMPORTANT.

    Variable
    console.log(a)
    var a = 10


    Memory phase:
        a → undefined
        Function
        hello()

    function hello(){}

    Memory phase:

    hello → complete function
    BIG DIFFERENCE

    Variables: undefined
    Functions: full function object

8. Function Execution Context
    Whenever function runs:

    function add(a, b) {
        return a + b
    }
    add(2, 3)

    JS creates NEW execution context.

    Inside Function Context
        JS creates:
            local memory
            local variables
            parameter values
            own scope
            own execution environment

    Important Understanding
    Every function call gets:
        completely separate memory space

    Example:
        function test(x) {
            return x * 2
        }
        test(5)
        test(10)

    Each call creates different execution context.


9. Call Stack
    VERY IMPORTANT.
    JavaScript uses: Call Stack
    to manage execution contexts.

    "Call Stack is a stack data structure that keeps track of execution contexts."

    Stack Rule
        Last In → First Out (LIFO)
    
    Example
        function one() {
            two()
        }

        function two() {
            three()
        }

        function three() {
            console.log("Hello")
        }

        one()

    Stack Flow
        Global Context

        ↓ push one()

        ↓ push two()

        ↓ push three()

        ↓ execute

        ↓ pop three()

        ↓ pop two()

        ↓ pop one()


10. Stack Overflow
    Example:
        function test() {
            test()
        }

        test()

    Infinite recursion.
    Stack keeps growing.

    Eventually:
    Maximum call stack size exceeded

    Why?
    Because:
    new execution contexts keep stacking
    memory limit exceeded

11. Internal JavaScript Engine Thinking

    When JS sees:
        function greet(name) {
            return "Hello " + name
        }
        greet("Vipin")

    Engine internally does conceptually:

    Step 1: 
        Create Global Execution Context.

    Step 2:  Memory Phase
        greet → function object

    Step 3: Execution Phase
        Function call found:
        greet("Vipin")

    Step 4:
        Create Function Execution Context.

    Step 5: Local memory
        name → "Vipin"
        
    Step 6: 
        Execute code.

    Step 7: 
        Return value.

    Step 8:
        Destroy function context.

12. Important Realization

    Execution context is why:
        variables work
        scope works
        closures work
        functions isolate data
        recursion works
        call stack works

    This is JavaScript’s execution engine foundation.


    Common Beginner Mistakes
        Mistake 1:
            Thinking JS directly executes line by line.
            WRONG.
            JS first:
                creates memory
                sets up environment
                then executes

        Mistake 2:
            Confusing:
                memory phase
                execution phase

        Mistake 3: 
            Not understanding each function gets new context.

        Mistake 4: 
            Thinking variables and functions hoist same way.

        WRONG.

Interview Questions
    Q1. What is execution context?

    Q2. Difference between:
        Global Execution Context
        Function Execution Context

    Q3. What happens during memory creation phase?

    Q4. Why are variables undefined before assignment?

    Q5. Why can function declarations run before definition?

    Q6. What is call stack?

    Q7. Why does stack overflow happen?