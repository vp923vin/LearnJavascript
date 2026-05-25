Chapter 6 — "this" Keyword Complete Mastery

1. What is "this"?







1. What is "this"?
    "this" is a special keyword that refers to the object that is currently executing the function.

    this means:
        "Who called this function?"
    VERY IMPORTANT.


    Biggest Rule of this
        this is NOT determined where function is written
        this is determined HOW function is called

    This is the MOST IMPORTANT RULE.

    Example
        const user = {
            name: "Vipin",

            greet() {
                console.log(this.name)
            }
        }

        user.greet()

    Output: Vipin

    WHY?
    Because:
        user.greet()

    means:
        user object called greet()

    So:
        this = user

    
    Mental Model
        LEFT side of dot decides this
        VERY IMPORTANT beginner rule


2. Global this

    Browser
        In browser global execution context:
        console.log(this)

        Output: window

    Because:
        global object in browser is:
        window

    Node.js

    In Node.js:
        global behavior differs.

    But browser interview questions usually assume:
    window


3. this Inside Normal Function
    VERY IMPORTANT.

    Example
        function test() {
            console.log(this)
        }

        test()
    Browser Non-Strict Mode

    Output: window

    WHY?
    Because:
        simple function call defaults to global object.

    Strict Mode
        "use strict"

        function test() {
            console.log(this)
        }

        test()
    
    
    Output: undefined

    WHY?
    Strict mode prevents automatic global binding.
    Safer behavior.

    Answer:
    Normal function this depends on invocation.

4. this Inside Object Method
    MOST COMMON USE.

    Example
        const user = {
            name: "Vipin",

            greet() {
                console.log(this.name)
            }
        }

        user.greet()

    Output: Vipin

    WHY?

    Because:
        user.greet()

    means:
        this = user

    Visual Representation
        user
        ↓
        greet()
        ↓
        this = user


        Important Rule
            Method call → object before dot becomes this
            Example
                const car = {
                    brand: "BMW",

                    show() {
                        console.log(this.brand)
                    }
                }

                car.show()

            Here:
                this = car



5. this Depends on Call-Site
    THIS IS THE CORE CONCEPT.

    Example
        function greet() {
            console.log(this)
        }

        const user = {
            greet
        }

        user.greet()

    Output: user object

    But:
        greet()

    Output:
        window (or undefined in strict mode)


    SAME FUNCTION
        Different invocation.
        Different this.
        VERY IMPORTANT.

    Mental Rule
        Function definition does NOT decide this
        Function call decides this

6. Losing this

    VERY IMPORTANT real-world problem.
    Example
        const user = {
            name: "Vipin",

            greet() {
                console.log(this.name)
            }
        }

        const fn = user.greet

        fn()

    Output: undefined

    WHY?

    Because now:
        fn()
        is simple function call.

    NOT:
        user.greet()

    So:
        this ≠ user anymore
    

    Visual

        Original:
            user.greet()
            this = user

        After extraction:
            fn()
            this = global/undefined


    This bug is EXTREMELY common in:
        React
        event handlers
        callbacks
        async code

7. Arrow Functions and this

    VERY IMPORTANT.

    Arrow functions DO NOT create their own this.

    Arrow functions lexically inherit this from surrounding scope.

    Example
        const user = {
            name: "Vipin",

            greet() {

                const inner = () => {
                    console.log(this.name)
                }

                inner()
            }
        }

        user.greet()

    Output: Vipin

    WHY?
    Inside greet(): 
        this = user

        Arrow function captures it.

    Visual
        greet()

        this = user
            ↓
        Arrow function captures it



    VERY IMPORTANT

    Arrow function ignores:
        who called me

    Instead checks:
        where was I created

    
8. Wrong Arrow Function Usage
    BAD
        const user = {
            name: "Vipin",

            greet: () => {
                console.log(this.name)
            }
        }

    Output: undefined

    WHY?
        Arrow function created in global scope.

    So:
        this = global this

    NOT: user


    HUGE RULE
    Never use arrow functions as object methods when you need object this


9. Constructor Function this
    Example
        function User(name) {
            this.name = name
        }

        const u1 = new User("Vipin")

    What Happens?

    new keyword:
        creates empty object
    sets:
        this = new object

    returns object
    Visual
        new User("Vipin")
        ↓
        this = {}

    Then:

        this.name = "Vipin"

    Result
        {
            name: "Vipin"
        }

10. Explicit Binding

    JavaScript allows manually controlling this.

    Using:

        call()
        apply()
        bind()

    Quick Example
        function greet() {
            console.log(this.name)
        }

        const user = {
            name: "Vipin"
        }

        greet.call(user)

    Output: Vipin

    Because:
        this = user manually


11. Internal JavaScript Thinking

    When JS sees:
        obj.say()

    Engine internally thinks:
        Call function with:
        this = obj

    When JS sees:
        say()

    Engine thinks:
        Normal function call

    So:
        this = global/undefined


12. Four Main this Rules
    Rule 1 — Global Binding
        console.log(this)

    Rule 2 — Implicit Binding
        obj.method()
        this = obj
    Rule 3 — Explicit Binding
        call()
        apply()
        bind()
    Manually set this.

    Rule 4 — new Binding
        new User()
        this = new object


    Arrow Function Special Rule
    Arrow functions:
        do not create their own this


Common Beginner Mistakes
    Mistake 1
        Thinking this refers to function itself.
        WRONG.

    Mistake 2
        Thinking this depends where function is written.
        WRONG.
        Depends how called.

    Mistake 3
        Using arrow functions as object methods.

    Mistake 4
        Losing this by storing method separately.

Interview Questions
    Q1. What determines this in JavaScript?

    Q2. Difference between normal and arrow function this?

    Q3. Why does method lose this?

    Q4. What does new do internally?

    Q5. Why should arrow functions not be used as object methods?

    Q6. What are the 4 binding rules of this?