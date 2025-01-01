# Event Loop

- The event loop is a key concept in programming, especially in languages like JavaScript, Python (asyncio), and Node.js, that handle **asynchronous operations**. Here's a simple explanation:
- Micro-tasks **Promises and process.nextTick (Node.js specific).** always execute first after synchronous code.
- Macro-tasks **(like setTimeout, setInterval, and I/O callbacks)** come next.
- I/O Callbacks (like fs.readFile) are processed after other tasks if they are ready.
- `Order execution`
    - **Synchronous code** → **Micro-tasks** → **Macro-tasks**.-> **I/O Callbacks**
  
#### - example:
```js
console.log("Start");

setTimeout(() => {
  console.log("Macro-task: setTimeout");
}, 0); // Macro-task

Promise.resolve().then(() => {
  console.log("Micro-task: Promise");
}); // Micro-task

setImmediate(() => {
  console.log("Macro-task: setImmediate (Node.js specific)");
}); // Macro-task (Node.js)

console.log("End");
```
#### Output:
```js
Start
End
Micro-task: Promise
Macro-task: setTimeout
Macro-task: setImmediat
```
