Chapter 4 — Scope & Lexical Environment Deep Dive

1. What is Scope?
2. Types of Scope
3. Global Scope
4. Function Scope
5. Block Scope
6. Lexical Scope (MOST IMPORTANT)
7. Scope Chain
8. Shadowing
9. Lexical Environment (Advanced Foundation)
10. Internal JavaScript Variable Lookup



1. What is Scope?
    Scope determines where variables and functions are accessible in a program.

    Scope decides:
        "Where can I access this variable?"

    Example
        let name = "Vipin"

        function test() {
            console.log(name)
        }

        test()

    Output: Vipin
  
    Because:
    name exists in accessible scope.

    Another Example
        function test() {
            let age = 25
        }
        console.log(age)

    Output: ReferenceError

    Because:
        age exists only inside function scope.

    Mental Model
        Think of scope like rooms in a house.
            Global Room
                ↓
            Function Room
                ↓
            Inner Function Room
        
        Inner rooms can access outer rooms.
        Outer rooms cannot access inner rooms.

2. Types of Scope

    Mainly:
        Global Scope
        Function Scope
        Block Scope
        Lexical Scope

3. Global Scope
    Variables declared outside all functions and blocks belong to global scope.

    Example
        let city = "Bhopal"
        function showCity() {
            console.log(city)
        }
        showCity()

    Output: Bhopal

    Because:
        global variables are accessible inside functions.

    Important Note: 
        Too many global variables are BAD.

        Why?
        Because:
            memory pollution
            accidental overwrites
            debugging difficulty

        Avoid unnecessary globals.

4. Function Scope
    Variables declared inside a function are accessible only within that function.

    Example
        function test() {
            let score = 100
            console.log(score)
        }

        test()
    Works.

    But:
        console.log(score)

    ❌ Error

    Why?
    Because:
        score belongs only to:
        test() execution context

    Outside cannot access inside.

    Important Realization
    Every function creates:
        new execution context
        new local memory
        new scope


5. Block Scope
    Variables declared with let and const inside {} are block scoped.

    Example
        {
            let a = 10
            const b = 20
        }
        console.log(a)

    ❌ Error

    Why?
    Because:
        a exists only inside block.
    
    VERY IMPORTANT
        var ignores block scope.
        Example:
            {
                var x = 50
            }
            console.log(x)
        
        Output: 50

    Because:
        var is function-scoped, NOT block-scoped.

6. Lexical Scope (MOST IMPORTANT)
    This is where JavaScript becomes powerful.

    Lexical Scope means scope is determined by where functions are written in code.

    Lexical scope means:
        Functions remember where they were created.
    VERY IMPORTANT.

    Example:
        function outer() {
            let username = "Vipin"
            function inner() {
                console.log(username)
            }
            inner()
        }
        outer()

    Output: Vipin

    WHY?
    Because:
        inner() was CREATED inside outer().
        So inner function gets access to outer scope.

    VERY IMPORTANT RULE
    Inner scope can access outer scope
    BUT:
    Outer scope cannot access inner scope

    Example
        function outer() {
            function inner() {
                let secret = "Hidden"
            }
            console.log(secret)
        }
        outer()

    ❌ Error

    Because:
    outer cannot access inner scope.


    Mental Model:
        Think of scope lookup like climbing stairs upward.
            Inner Scope
                ↑
            Outer Scope
                ↑
            Global Scope
        JavaScript searches upward only.
        Never downward.

7. Scope Chain
    Scope Chain is the chain of accessible scopes used to resolve variable lookup.

    Example
        let country = "India"

        function outer() {

            let state = "MP"

            function inner() {

                let city = "Bhopal"

                console.log(city)
                console.log(state)
                console.log(country)
            }

            inner()
        }

        outer()

    Output:

    Bhopal
    MP
    India

    Variable Lookup Process
    When JS sees:
        console.log(state)
    Inside inner():

    JS searches:
        Step 1: 
            Inside inner scope.
            Not found.

        Step 2:
            Move to outer scope.
            Found:
            state = "MP"

        Stop searching.

    Important Rule
        JavaScript searches:
            inside → outside

        NOT:
            outside → inside


8. Shadowing
    VERY IMPORTANT interview topic.

    Shadowing occurs when inner variable uses same name as outer variable.

    Example
        let name = "Global"

        function test() {

            let name = "Local"

            console.log(name)
        }

        test()

    Output: Local
    WHY?
    Inner variable shadows outer variable.
    Nearest variable wins.

9. Lexical Environment (Advanced Foundation)
    Lexical Environment is an internal JavaScript structure that stores variable mappings and references to outer environments.

    Lexical environment is:
        Hidden memory system storing variables and outer references.

    Conceptually

    When function creates:
        function test() {
            let x = 10
        }
    JS internally creates something like:

    Lexical Environment:
    x → 10
    Outer Reference → Global Scope
       
    VERY IMPORTANT
        Every function gets:
            local variables
            reference to outer environment
    This is the FOUNDATION of closures. 

10. Internal JavaScript Variable Lookup
    When JS sees:
        console.log(username)
        It searches:
            Current Scope
                ↓
            Outer Scope
                ↓
            Global Scope
    This searching mechanism is scope chain.

    Common Beginner Mistakes
        Mistake 1:
            Thinking object creates scope.

            WRONG.
            Only:
                functions
                blocks
                modules
            create scope.

        Mistake 2:
            Confusing block scope and function scope.

        Mistake 3:
            Thinking variables search downward.

            WRONG.
            Lookup always goes upward.

        Mistake 4:
            Confusing lexical scope with execution order.

            Lexical scope depends on:
                where function is WRITTEN
            NOT:
                where function is CALLED

        VERY IMPORTANT.

Interview Questions
    Q1. What is scope?

    Q2. Difference between:
        global scope
        function scope
        block scope
    Q3. What is lexical scope?

    Q4. What is scope chain?

    Q5. What is shadowing?

    Q6. How does variable lookup work?

    Q7. Why can inner function access outer variables?