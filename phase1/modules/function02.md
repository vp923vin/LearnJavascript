chapter 2 functions introductions

1. Function Declaration
2. Function Expression
3. Anonymous Functions
4. Named Function Expressions
5. Arrow Function
6. Syntax Variations
7. Biggest Difference — this
8. Arrow Functions Do NOT Have arguments
9. Arrow Functions Cannot Be Constructors
10. Arrow Functions and Hoisting





1. Function Declaration
    A Function Declaration is a way of defining a named function using the function keyword

    function greet() {
        console.log("Hello")
    }

    This is called:
        Function Declaration

    Because:
        function is declared directly
        parser registers it during memory creation phase

    "Create this function in memory before execution starts."

    ""Function declaration defines a named function that is fully hoisted during the memory creation phase.""

    - Important Characteristic

        You can call it BEFORE definition.
        sayHi()

        function sayHi() {
            console.log("Hi")
        }

        output: Hi

        Why?
        Because of:   Hoisting

    - Internal Engine Visualization
        Before execution:
        Memory Phase:
        sayHi → function object

        Then execution starts.

2. Function Expression
    A Function Expression is a function that is created as part of an expression and assigned to a variable.

    const greet = function() {
        console.log("Hello")
    }

    This is NOT function declaration.  
    This is:
        Function Expression
    Because:
        function becomes a value
        assigned into variable

    "Create function object → assign into variable"

    
    VERY IMPORTANT DIFFERENCE
    greet()

    const greet = function() {
        console.log("Hello")
    }

    output: ReferenceError

    Why?

    Because:

        variable exists in Temporal Dead Zone
        function object not assigned yet

    A function expression is a function assigned to a variable, where the function is created during execution time rather than fully hoisted.



    | Feature                    | Function Declaration     | Function Expression           |
    | -------------------------- | ------------------------ | ----------------------------- |
    | Syntax                     | Direct function creation | Function assigned to variable |
    | Hoisting                   | Fully hoisted            | Variable hoisted only         |
    | Callable before definition | ✅ Yes                    | ❌ No                          |
    | Function creation time     | Memory phase             | Execution phase               |
    | Usually named?             | Yes                      | Often anonymous               |
    | Common usage               | Reusable logic           | Callbacks / dynamic behavior  |


    Function declaration is fully hoisted and can be called before definition, while function expression is assigned during execution and cannot be called before initialization.

3. Anonymous Functions

    const test = function() {
        console.log("Hello")
    }

    This function has NO name, called Anonymous Function,.

    Why Anonymous Functions Exist?  
    Mostly for:
        callbacks
        temporary behavior
        inline logic

    Example:
        setTimeout(function() {
            console.log("Done")
        }, 1000)

    Function used once only.

    Problem with Anonymous Functions
        - Harder debugging.
        - Error stacks become ugly.

    
4. Named Function Expressions
    A Named Function Expression is a function expression where the function itself has its own internal name.

    Easy definition: 
    A named function expression is:
        a function stored in variable
        but function also keeps its own private name
    
    const test = function greet() {
        console.log("Hello")
    }

    Interesting case.

    Here:
        Variable name:
            test

        Function internal name:
            greet

    Why Useful?

    Better:
        stack traces
        recursion
        debugging

    Important Behavior  
    This works:
        test()

    But this fails outside:
        greet()

    Because internal function name exists only inside function scope.

    Visual Representation
        Global Scope:

        test → function object

        Inside function only:
        greet → self reference


5. What is an Arrow Function?
    An Arrow Function is a shorter syntax for writing functions introduced in ES6, which does not have its own this, arguments, super, or prototype.

    Easy Definition
        Arrow function is a shorter modern function syntax that behaves differently from normal functions internally.

    Basic Syntax
    Normal function:
        function add(a, b) {
            return a + b
        }

    Arrow function:
        const add = (a, b) => {
            return a + b
        }   
    Short: 
    const add = (a, b) => a + b;

    NOte: 
        Arrow functions are lightweight functions
        that borrow `this` from surrounding scope.

    Why Arrow Functions Were Created?

        Old JavaScript had HUGE problem with this.

    Example:

        const user = {
            name: "Vipin",

            greet: function() {
                setTimeout(function() {
                    console.log(this.name)
                }, 1000)
            }
        }

        user.greet()

    Output: undefined

    Why?
    Because callback function got its own this.
    This caused massive confusion.
    Arrow functions solved this problem.


    Arrow Function Solution
        const user = {
            name: "Vipin",

            greet: function() {
                setTimeout(() => {
                    console.log(this.name)
                }, 1000)
            }
        }

        user.greet()

    Output: Vipin

    Because arrow functions do NOT create their own this.
    This is their biggest feature.


6. Syntax Variations

    Multiple Parameters:
    const add = (a, b) => {
        return a + b
    }

    Single Parameter
    Parentheses optional:
    const square = n => {
        return n * n
    }

    No Parameters
    const greet = () => {
        console.log("Hello")
    }

    One-Line Implicit Return
    const multiply = (a, b) => a * b
    Automatically returns value.

    Equivalent to:
    const multiply = (a, b) => {
        return a * b
    }


    Returning Object

    Common beginner trap.
    WRONG:
    const user = () => { name: "Vipin" }

    JS thinks {} is function body.

    Correct:
    const user = () => ({ name: "Vipin" })
    Parentheses force object return.



7. Biggest Difference — this
    THIS IS THE HEART OF ARROW FUNCTIONS.
    Normal Function

    Normal functions create their own this.

    Example:
        const user = {
            name: "Vipin",

            greet: function() {
                console.log(this.name)
            }
        }

        user.greet();
    
    this depends on HOW function is called.

    **Arrow Function:**
        - Arrow functions DO NOT create their own this.
        - They capture surrounding this.
        - Called: Lexical this

    Lexical this means arrow functions inherit this from their surrounding scope instead of creating their own.

    Example
        const user = {
            name: "Vipin",

            greet: function() {

                const inner = () => {
                    console.log(this.name)
                }

                inner()
            }
        }

        user.greet()

    Output: Vipin

    Arrow function borrowed:
    this = user object
    from surrounding greet() function.

8. Arrow Functions Do NOT Have arguments

    Normal function:
        function test() {
            console.log(arguments)
        }

        test(1, 2, 3)
    Works.

    Arrow function:

        const test = () => {
            console.log(arguments)
        }

    ❌ Error

    Because arrow functions do NOT create:
        own arguments
        own this
        own super

    Use Rest Parameters Instead
        const test = (...args) => {
            console.log(args)
        }

9. Arrow Functions Cannot Be Constructors
    Normal function:

    function User(name) {
        this.name = name
    }

    const u = new User("Vipin")
    Works.

    Arrow function:

    const User = (name) => {
        this.name = name
    }

    new User("Vipin")

    ❌ Error

    Why?

    Because arrow functions do NOT have:

        [[Construct]]

    internal method.

    And they do NOT have prototype.


    Check Prototype:
        Normal function:
            function test(){}
            console.log(test.prototype)

        Exists.

        Arrow function:
            const test = () => {}
            console.log(test.prototype)

        Output: undefined

    Arrow functions do not have their own this, prototype, or internal [[Construct]] method, so they cannot be called with new.

10. Arrow Functions and Hoisting
    Function Declaration
        hello()

        function hello(){}

    ✅ Works

    Arrow Function
        hello()
        const hello = () => {}

    ❌ Error

    Because:
        variable hoisting rules apply
        not function declaration rules



    Important Understanding
        Arrow functions are usually:
            Function Expressions
            NOT declarations.


11. Should Arrow Functions Replace Normal Functions?
    NO.

    Very common beginner mistake.

    Avoid Arrow Functions For
        ❌ object methods needing own this
        ❌ constructors
        ❌ prototype methods sometimes
        ❌ dynamic this

    BAD Example
        const user = {
            name: "Vipin",

            greet: () => {
                console.log(this.name)
            }
        }

        user.greet()

    Output: undefined

    Because arrow function captured outer/global this.

    NOT object this.

    Correct Version:
        const user = {
            name: "Vipin",

            greet() {
                console.log(this.name)
            }
        }

12. Internal Engine Difference

    Normal Function Internally
    Conceptually:
        Function Object:
        - code
        - prototype
        - this
        - arguments
        - constructor ability


    Arrow Function Internally
    Conceptually:
        Function Object:
        - code
        - lexical this reference
    Much lighter.

    Common Beginner Mistakes
        Mistake 1 Using arrow function as object method.
        Mistake 2 Forgetting implicit return rules.
        Mistake 3 Returning object incorrectly.
        Mistake 4 Thinking arrow functions are just shorter syntax.

    WRONG: They behave differently internally.


Interview Questions
    Q1 Difference between normal and arrow function?

    Q2 Why do arrow functions not have their own this?

    Q3 Can arrow functions be constructors?

    Q4 Why are arrow functions useful in callbacks?

    Q5 Difference between: function(){} and () => {} internally ?