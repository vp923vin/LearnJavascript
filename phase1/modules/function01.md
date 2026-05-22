Today you learned:

1. why functions exist
2. abstraction
3. reusable behavior
4. functions as objects
5. first-class citizens
6. memory references
7. declaration vs execution
8. function scope


1. Why Do Functions Exist?

    Imagine writing this:
        console.log(2 + 3)
        console.log(5 + 7)
        console.log(10 + 20)
  
    Problem:
        repetitive
        hard to maintain
        not reusable
    
    Now:
        function add(a, b) {
            return a + b
        }

        console.log(add(2, 3))
        console.log(add(5, 7))
        console.log(add(10, 20))

    Core Idea:
        A function allows us to:
            store logic once
            execute many times
            give different inputs
            produce outputs

    
2. Functions Help in Abstraction

    Abstraction means:
        Hide complexity and expose simple usage.

    Example: 
        function sendEmail() {
            // complex logic
        }

    When you call:
        sendEmail();

    You don't care:
        SMTP
        server
        authentication
        formatting

    All complexity is hidden.
    That is abstraction.


3. DRY Principle

    DRY means:
        Don't Repeat Yourself
    
    Without functions:
        let area1 = 10 * 20
        let area2 = 30 * 40
        let area3 = 50 * 60

    With function:
        function area(width, height) {
            return width * height
        }
    
    Cleaner.
    Reusable.
    Maintainable.

4. Functions are Behaviors
    This is VERY important.
    In many languages, functions are just callable blocks.

    In JavaScript:
        functions are BEHAVIOR OBJECTS.
    Example:
        function greet() {
            console.log("Hello")
        }
        This is not just code.

    JavaScript creates:
        - executable code
        - function object
        - hidden properties
        - memory references

    Functions are special objects.

5. Functions are First-Class Citizens

    This is one of JavaScript’s superpowers.
    You can:
        1. Store in variable:
            const sayHi = function() {
                console.log("Hi")
            }

        2. Pass into another function:
            function execute(fn) {
                fn()
            }
            execute(sayHi)

        3. Return from function: 
            function create() {
                return function() {
                    console.log("Inside")
                }
            }

        4. Store in object:
            const user = {
                greet: function() {
                    console.log("Hello")
                }
            }

        Why This Matters?
            This capability created:
                React patterns
                Express middleware
                callbacks
                promises
                async systems
                event systems
                functional programming
        Modern JavaScript exists because functions are first-class citizens.


6. JavaScript Functions are Objects
    
    function test() {}
    console.log(typeof test)

    output: function
    Looks special.

    But internally:
        test.name
        test.length

    Functions have properties.
    Because functions are actually objects.

    Functions Can Have Custom Properties
        function greet() {}
        greet.language = "English"
        console.log(greet.language)

        output: English
    Normal functions behaving like objects

    Internal JavaScript Reality:
    When JS sees:
        function greet() {}

    Engine creates:
        executable function object
        hidden internal properties
        memory reference
        prototype object

    Something conceptually like:
    {
        code: executable code,
        name: "greet",
        prototype: {},
        [[Environment]]: reference
    }

    This is why functions are extremely powerful.

7. How JavaScript Stores Functions in Memory

    VERY IMPORTANT FOUNDATION.
    When JS runs:

    function greet() {
        console.log("Hello")
    }
    
    JavaScript stores function in memory.

    Conceptually:
        Memory: 
            greet → function object reference

        The actual function lives somewhere in memory.
        Variable greet points to it.

        Visual Representation: 

            STACK MEMORY
                greet  --------->  HEAP MEMORY

                                    Function Object
                                    ----------------
                                    code
                                    name
                                    prototype
                                    hidden properties

    Why This Matters Later?
        Because:
            closures depend on memory references
            callbacks depend on references
            async depends on references
            this depends on invocation context

        Everything connects later.


8. Function Declaration vs Function Execution

    BIG beginner confusion.
    This:
        function greet() {
            console.log("Hello")
        }

    ONLY declares function.
    Nothing runs.

    Execution happens here:
        greet()

    "Store this behavior for future use."

9. Functions Create Scope

    Every function creates its own world.

    Example:

        function test() {
            let message = "Hello"
        }
        console.log(message);

        output: Error.

    Because:
        message exists only inside function scope
        Functions create isolated memory environments.

    This becomes critical in:
        closures
        execution context
        lexical scope

    
10. JavaScript Functions are Special Compared to Other Languages
    In many languages:
        functions are limited
        In JavaScript:
            functions behave like values
            functions can return functions
            functions can store state
            functions can create closures
            functions can dynamically change behavior

    This is why JavaScript is highly flexible.

    
Interview Questions
Q1. Why are JavaScript functions called first-class citizens?

Q2. Difference between:
    declaring function
    invoking function

Q3. Can functions have properties?

Q4. Why are functions powerful in JavaScript?


Small Exercises

Exercise 1
    Create:
        multiply(a, b)
    Return multiplication

    function multiply(a, b){
        return a * b;
    }
    multiply(3, 5)

Exercise 2
    Store function in variable and execute it.

    const subtrct = function (a, b){
        return a - b;
    }
    subtrct(13, 5);


Exercise 3
    Add custom property to function.
    function greeting(){}
    greeting.day = "Thursday";
    console.log(greeting.day)

Exercise 4
    Create function that returns another function.
    function greeting(){
        return function hello(){
            console.log("hello mr/miss, how are you ?");
        }
    }
    greeting()();


Mini Challenge

Build calculator functions:
add()
subtract()
multiply()
divide()

    function add(a, b){
        return a + b;
    }
    function subtract(a, b){
        return a - b;
    }
    function multiply(a, b){
        return a * b;
    }
    function divide(a, b){
        return a / b;
    }


    