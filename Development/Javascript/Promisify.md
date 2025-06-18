
### Where it comes from

It's not a native feature of JavaScript itself, but rather a concept implemented in various JavaScript environments and libraries. 

Node provides `util.promisify()`


### callback-style function

A callback-style function is a function that takes another function (the callback) as one of its arguments. This callback function is then invoked at a later time, typically when an asynchronous operation completes or when a certain condition is met.

![[Screenshot 2024-10-31 at 6.10.54 PM.png|700]]

#### Key Characteristics

1. **Function as Argument**: The callback is passed as an argument to another function.
2. **Asynchronous Execution**: Often used for handling asynchronous operations.
3. **Inversion of Control**: The calling function determines when the callback is executed.
4. **Continuation-Passing Style**: The result is passed to the callback rather than returned directly.

#### Advantages

1. **Handling Asynchronous Operations**: Ideal for operations that don't return immediately.
2. **Flexibility**: Allows the caller to determine what happens after the operation completes.
3. **Separation of Concerns**: The function performing the operation doesn't need to know what happens next.

#### Challenges

1. **Callback Hell**: Nested callbacks can lead to complex, hard-to-read code.
2. **Error Handling**: Can be tricky, especially with deeply nested callbacks.
3. **Inversion of Control**: The calling code must trust the called function to execute the callback properly.


#### Modern Alternatives for callback-style function

While callback-style functions are still widely used, modern JavaScript often employs Promises or async/await for cleaner asynchronous code:


![[Screenshot 2024-10-31 at 6.13.48 PM.png|700]]


### What is Promisification ?

Promisify is a technique used to **transform** callback-style asynchronous functions into functions that return Promises. This transformation is often called "promisification."

