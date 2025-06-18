
## What is Promise ?

An object that represents the **eventual completion or failure** of an asynchronous operation.

A Promise is a special JavaScript object that links "producing code" (code that takes time to execute) with "consuming code" (code that wants the result of the producing code). It serves as a **proxy** for a value not necessarily known when the promise is created.

## Key Characteristics of Promises

1. **States**: A Promise can be in one of three states:
    
    - Pending: Initial state, neither fulfilled nor rejected
    - Fulfilled: The operation completed successfully
    - Rejected: The operation failed
    
2. **Immutability**: Once a promise is fulfilled or rejected, its state cannot change.
3. **Asynchronous**: Promises are designed to handle asynchronous operations.
4. **Chainable**: Promises can be chained together for sequential asynchronous operations.


A Promise is an object that represents the **result of an asynchronous computation**. 
That result may or may not be ready yet, and the Promise API is intentionally vague about this: **there is no way to synchronously get the value of a Promise**; you can **only ask** the Promise to call a callback function when the value is ready.

While computation of asynchronous codes runs, that part returns Promise Object.

![[Screenshot 2024-10-31 at 7.17.50 PM.png]]

So execution of function passed to then() is asynchronous, even execution of whole code finished.
Different event queue.

Code Execution != Asynchronous Function Execution

We can't figure out the state of Promise while the code executes.

## Difference between resolve, reject and then(), catch()

Resolve, and reject is called inside of Promise.
When resolved is called or asynchronous codes inside of Promise invokes error,
status of Promise becomes "Rejected", so outside of Promise, function passed to catch() will be executed.
Otherwise - when resolve inside Promise is executed, then Status Promise turns into "Fulfilled", and outside of Promise - function sent to then() will be executed.

so, Resolve and Reject determines States of Promise.


## Customized Error Handling Based on Rejection Source

While the final .catch() in a Promise chain conveniently handles errors from any stage, sometimes you need more fine-grained control to respond differently based on **where the rejection originated**. Let's explore how to achieve this, drawing on the sources and our conversation:

**1. Strategic Placement of .catch() within the Chain**

As highlighted in source [1], you can insert .catch() blocks at specific points within your chain to handle errors from particular then callbacks or Promises. Here's a modified example from our previous discussion:

![[Screenshot 2024-10-31 at 7.44.58 PM.png|600]]

In this refined version, we have two .catch() blocks:

- The first .catch() specifically handles errors that occur during the fetching or parsing of the JSON response.
- The second .catch() handles errors that arise during the displayUserProfile operation.

**2. Error Type Discrimination within .catch()**

Within a .catch() block, you can examine the error object to determine its type and implement different handling logic. Source [2] provides a good example of this:


![[Screenshot 2024-10-31 at 7.45.19 PM.png|700]]

This code snippet checks if the error is an instance of NetworkError or TypeError and displays different messages accordingly. You could extend this pattern to handle other error types or even examine custom error properties to make more specific decisions.

**3. Custom Error Objects for Enhanced Information**

Source [3] suggests defining your own error subclasses with additional properties to carry more context about the error. You could create custom error classes for different stages in your Promise chain, allowing your .catch() blocks to identify the source of the error based on the error object's class.

**Example:**

![[Screenshot 2024-10-31 at 7.45.39 PM.png|600]]


**4. Limitations and Considerations**

- **Promise Resolution**: Keep in mind that once a Promise is resolved (fulfilled or rejected), its state is fixed. A .catch() block further down the chain will only execute if a preceding .catch() re-throws the error or returns a rejected Promise.
- **Complex Logic**: If your error handling logic becomes very intricate, it might be a sign that you need to refactor your Promise chain or consider alternative asynchronous patterns.

**In conclusion, by thoughtfully positioning .catch() blocks and using error type discrimination, you can implement tailored error-handling strategies based on the origin of rejection within your Promise chains.**



## Executing Code After Multiple Promises Resolve

You're describing a scenario where you have two Promises, each representing an asynchronous operation that will eventually produce a result. You want to execute a piece of code only after **both** Promise 1 and Promise 2 have successfully resolved and you have access to both result 1 and result 2. The sources provide a solution to this common asynchronous programming challenge:

**Using Promise.all() for Parallel Execution**

Source [1] introduces the Promise.all() function, which is perfectly suited for this situation. Promise.all() takes an array of Promise objects as its input and returns a new Promise. This returned Promise possesses the following behavior:

- **Fulfillment:** It fulfills only when **all** the input Promises have fulfilled. The fulfillment value is an array containing the fulfillment values of each input Promise, in the same order as they were provided.
- **Rejection:** If **any** of the input Promises is rejected, the Promise.all() Promise is immediately rejected. The rejection value is the reason for the first rejected Promise.

**Code Example**

![[Screenshot 2024-10-31 at 8.04.55 PM.png|700]]

**Explanation**

1. **promise1 and promise2:** These represent your asynchronous operations, each returning a Promise. In this example, promise1 fetches and parses JSON data, while promise2 is a placeholder for another asynchronous function.
2. **Promise.all([promise1, promise2]):** This line creates a new Promise that represents the completion of both promise1 and promise2. It initiates the execution of these Promises in parallel.
3. **.then(([result1, result2]) => { ... }):** This callback function is executed only when the Promise.all() Promise fulfills, meaning both promise1 and promise2 have successfully resolved. The results are conveniently provided in an array, allowing you to access them as result1 and result2.
4. **.catch(error => { ... }):** This block handles any errors that might occur during the execution of either promise1 or promise2.

**Key Points**

- **Parallel Execution:** Promise.all() runs the provided Promises concurrently, potentially improving performance.
- **Guaranteed Completion:** Your code inside the .then() block after Promise.all() will only execute when all the input Promises have resolved successfully.
- **Error Handling:** The .catch() block provides a centralized way to manage errors, ensuring robustness.

By using Promise.all(), you can effectively coordinate multiple asynchronous operations and execute specific logic only when all of them have successfully completed.