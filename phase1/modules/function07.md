Chapter 7 — call(), apply(), bind() Deep Dive

1. Why call(), apply(), bind() Exist?
2. What is Explicit Binding?
3. call() Method
4. apply() Method
5. bind() Method (MOST IMPORTANT)
6. Function Borrowing
7. Partial Application
8. Internal JavaScript Thinking
9. Arrow Functions with bind/call/apply
10. Comparison Table



1. Why call(), apply(), bind() Exist?

    Problem:
        Sometimes JavaScript loses correct this.

    Example:
        const user = {
            name: "Vipin",
            greet() {
                console.log(this.name)
            }
        }
        const fn = user.greet
        fn()

    Output: undefined

    Because:
        this ≠ user anymore

    So JavaScript gives tools to manually control this.

    Those tools are:
        call()
        apply()
        bind()


2. What is Explicit Binding?
    Explicit binding means manually setting the value of this for a function.

    Explicit binding means:
        "You decide what this should be."

3. call() Method
    call() invokes a function immediately with a specified this value and arguments passed individually.

    Syntax
        functionName.call(thisValue, arg1, arg2)

    Example
        function greet() {
            console.log(this.name)
        }
        const user = {
            name: "Vipin"
        }
        greet.call(user)

    Output: Vipin

    
    Internal Thinking

    Normally:
        greet()

    means:
        this = global/undefined

    But:
        greet.call(user)

    means:
        this = user
    manually.

    Visual Representation
        call()
        ↓
        Force:
        this = user

    Passing Arguments
        function introduce(city) {
            console.log(this.name, city)
        }

        const user = {
            name: "Vipin"
        }

        introduce.call(user, "Bhopal")

    Output: Vipin Bhopal

    Mental Rule
    call() executes immediately


4. apply() Method

    Very similar to call().

    apply() invokes a function immediately with specified this value and arguments passed as array.

    apply() is same as call()
    BUT arguments come inside array.

    Syntax
        fn.apply(thisValue, [args])

    Example
        function introduce(city, country) {
            console.log(this.name, city, country)
        }

        const user = {
            name: "Vipin"
        }

    introduce.apply(user, ["Bhopal", "India"])

    Output: Vipin Bhopal India

    Difference:
        call vs apply
        
    call()  
    Arguments separate:
        fn.call(obj, a, b, c)
        
    apply()
    Arguments array:
        fn.apply(obj, [a, b, c])

    Interview Answer
        Difference between call and apply?
        Both explicitly bind this, but call() accepts arguments individually while apply() accepts arguments as array.


5. bind() Method (MOST IMPORTANT)

    This one confuses many developers.

    bind() returns a new function with permanently bound this.

    bind() means:
    "Create a new function where this is fixed forever."

    VERY IMPORTANT.

    Biggest Difference
        Method	Executes Immediately?
        call	✅ Yes
        apply	✅ Yes
        bind	❌ No

    Example
        function greet() {
            console.log(this.name)
        }
        const user = {
            name: "Vipin"
        }
        const boundFn = greet.bind(user)
        boundFn()

    Output: Vipin

    VERY IMPORTANT

    This:
        greet.bind(user)
    does NOT execute function.
    It RETURNS new function.

    Visual Representation
        bind()
        ↓
        returns NEW function
        with fixed this

    Mental Model
        Original Function
            ↓
        bind(user)
            ↓
        New Function
        (this permanently = user)

    Why bind() is Powerful?
    Because callbacks often lose this.

    Example:
        const user = {
            name: "Vipin",

            greet() {
                console.log(this.name)
            }
        }

        setTimeout(user.greet, 1000)

        Output: undefined
    
    WHY?
    Because:
        setTimeout calls function normally.

    So:
        this lost
        Solution Using bind()
        setTimeout(user.greet.bind(user), 1000)

    Now:
        this permanently = user

    Output: Vipin

    Real-World Importance
    bind() heavily used in:
        React class components
        event handlers
        async callbacks
        DOM listeners


6. Function Borrowing

    VERY IMPORTANT.

    Functions can be reused between objects.

    Example
        const user1 = {
            name: "Vipin"
        }

        const user2 = {
            name: "Rahul"
        }

        function greet() {
            console.log(this.name)
        }

        greet.call(user1)
        greet.call(user2)

    Output:
        Vipin
        Rahul


    Why Powerful?
        Same function reused for multiple objects.
        This is:
        Function Borrowing


7. Partial Application

    Advanced concept.

    Example
        function multiply(a, b) {
            return a * b
        }

        const double = multiply.bind(null, 2)

        console.log(double(5))

    Output: 10

    WHY?
    bind() preset:
        a = 2

    Remaining argument:
        b = 5

    This is:
        Partial Application

    Very important functional programming concept.

8. Internal JavaScript Thinking

    When JS sees:
        fn.call(obj)

    Engine conceptually thinks:
        Execute fn

    with:
        this = obj
        
    bind()
    Conceptually:
        Create wrapper function
        with permanently fixed this


9. Arrow Functions with bind/call/apply

    VERY IMPORTANT.

    Arrow functions IGNORE:
        call
        apply
        bind

    Example
        const test = () => {
            console.log(this)
        }
        test.call({name: "Vipin"})

    Arrow function still uses lexical this.

    Because arrow functions do NOT create own this.

    HUGE Interview Question

        Why don't bind/call/apply work on arrow functions?

        Arrow functions do not have their own this. They lexically inherit this from surrounding scope, so explicit binding methods cannot change it.


10. Comparison Table
    Feature	               call	  apply	  bind
    Executes immediately	✅	  ✅	    ❌
    Returns new function	❌	  ❌	    ✅
    Arguments	          separate	array separate/preset
    Changes this	         ✅	   ✅	✅      permanent

Common Beginner Mistakes
    Mistake 1:
        Thinking bind() executes immediately.
        WRONG.
        It returns new function.

    Mistake 2:
        Confusing call/apply argument style.

    Mistake 3:
        Using arrow functions with bind/call/apply.

    Mistake 4:
        Not understanding callback this loss.

Interview Questions
    Q1. Difference between:
        call
        apply
        bind
    Q2. Why does bind() return new function?

    Q3. What is function borrowing?

    Q4. Why does this get lost in callbacks?

    Q5. Why don't bind/call/apply work with arrow functions?

    Q6. What is partial application?

Mini Challenge

    Create reusable function:
    showDetails(city, country)

    Use:
        call()
        apply()
        bind()

    with multiple objects.