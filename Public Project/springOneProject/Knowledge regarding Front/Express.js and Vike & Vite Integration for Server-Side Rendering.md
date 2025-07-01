- [x] Express.js and Vike & Vite Integration for Server-Side Rendering ➕ 2025-07-01 ✅ 2025-07-01

### When Express.js handles a user's request, does it create a Vike process as middleware per request?

No, Express.js does not create a new Vike process for each user request. Vike runs as part of the same Node.js process as your Express server.

#### How Express.js and Vike Integration Works

When Vike is integrated with Express.js for server-side rendering, it operates within the same single Node.js process.

#### Development Environment

- Express.js utilizes Vite in middleware mode.
    
- Vite's development server is integrated as middleware using `app.use(vite.middlewares)`.
    
- Incoming requests are handled by Express, which then calls Vike's rendering functions within the same process.
    

#### Production Environment

- Express.js serves the pre-built application.
    
- Vike's server-side rendering is executed through function calls within the same Node.js process.
    
- No separate processes are created for individual requests.
    

#### Middleware Execution Model

- Express.js middleware functions, including Vike, are executed sequentially for each request within a single process.
    
- The middleware stack processes each request, with each function having access to the request (`req`), response (`res`), and `next` objects.
    

```js
// Example of how Vike integrates with Express
app.use('*', async (req, res, next) => {
  // Vike rendering happens here within the same process
  const { render } = await vite.ssrLoadModule('/src/entry-server.js');
  const appHtml = await render(url);
  // Send response back
});
```

#### Performance Implications

This single-process model is more efficient because it avoids the overhead of creating and destroying processes for each request. It allows for shared memory, enabling resources like compiled templates and modules to be cached and reused, leading to faster execution through direct function calls rather than inter-process communication.

### When it's stated that 'Express.js runs with Vite in middleware mode,' does that mean Vite's program code resides within the Express.js process?

Yes, that is correct. When Express.js runs with Vite in middleware mode, the Vite program code resides within the same process as the Express.js application.

#### How Vite Middleware Mode Works

You create a `ViteDevServer` instance that becomes a part of your Express.js application.

```js
const vite = await createViteServer({
  server: { middlewareMode: true },
  appType: 'custom'
});
```

This `vite` variable is an instance of `ViteDevServer` running inside your Express.js process. It's integrated using:

```js
app.use(vite.middlewares);
```

#### Single Process Architecture

This integration means Vite's functionality is embedded within the Express.js process. The `vite.middlewares` is an instance that gets added to Express's middleware stack, processing requests within the same Node.js process that runs the Express server.

#### Development vs. Production Behavior

- **During Development**: Vite's development server features (like HMR, module transformation) run within the Express process. No separate Vite process is spawned.
    
- **During Production**: The single-process model is maintained. Vite's build artifacts are served by Express, still without involving separate processes.
    

This architecture provides full control over the main server while decoupling Vite from the production environment, embedding its capabilities directly into the Express application.

---

### Summary

#### Express and Vite/Vike Integration

- **Single-Process Architecture**: When using Vite/Vike with Express.js for Server-Side Rendering (SSR), both run within the same single Node.js process. Express does not create a new process for each incoming request.
    

#### Middleware Mode

- **In-Process Execution**: In development, Vite operates in "middleware mode." A `ViteDevServer` instance is created and integrated directly into the Express application's middleware stack.
    
- **Code Residence**: Vite's program code and its functionalities reside and execute within the Express.js process.
    

#### Request Handling

- **Development**: Express handles incoming requests and passes them through its middleware stack, which includes Vite's development server for tasks like Hot Module Replacement (HMR) and module transformation.
    
- **Production**: Express serves the static build artifacts from Vite. SSR is performed by calling rendering functions within the same Express process.
    

#### Performance

- **Efficiency**: This single-process model is highly efficient as it avoids the performance overhead associated with creating and managing multiple processes.
    
- **Resource Sharing**: Memory and resources (e.g., cached modules) are shared within the single process, leading to faster execution via direct function calls.