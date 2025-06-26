- [ ] TypeScript Configuration for React Projects ➕ 2025-06-26 


### Show me an example file of a TypeScript setting file and example usage of TypeScript codes in React. And explain its contents for understanding.

A TypeScript project is configured using a `tsconfig.json` file, which dictates the compiler options and the files to be included in the compilation process.

#### `tsconfig.json` Example

```json
{
  "compilerOptions": {
    "target": "es2017",
    "module": "commonjs",
    "lib": ["dom", "es2017"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strictNullChecks": true
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "exclude": [
    "node_modules",
    "**/*.spec.ts",
    "**/*.test.ts"
  ]
}
```

#### Key `tsconfig.json` Sections Explained

- **compilerOptions**: An object containing rules for the TypeScript compiler.
    - `target`: Specifies the JavaScript version for the output code (e.g., `"es2017"`).
    - `module`: Defines the module system for imports/exports (e.g., `"commonjs"`).
    - `jsx`: Configures JSX compilation, with `"react-jsx"` being the modern standard for React.
    - `strict`: Enables all strict type-checking options.
    - `strictNullChecks`: Prevents `null` and `undefined` from being assigned to variables unless explicitly declared.
- **include**: An array of glob patterns specifying the files the compiler should process.
- **exclude**: An array of glob patterns specifying files to be ignored by the compiler.

You can generate a `tsconfig.json` file with default options by running `tsc --init`.

#### TypeScript with React Examples

##### Basic Functional Component with Props

Props can be typed using an interface, ensuring type safety.

```Typescript
import React from 'react';

// Define prop types using an interface
interface MyButtonProps {
  /** The text to display inside the button */
  title: string;
  /** Whether the button can be interacted with */
  disabled?: boolean;
}

// Functional component with typed props
const MyButton: React.FC<MyButtonProps> = ({ title, disabled = false }) => {
  return (
    <button disabled={disabled}>
      {title}
    </button>
  );
};

// Usage in parent component
const App: React.FC = () => {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="I'm a button" />
      <MyButton title="I'm disabled" disabled={true} />
    </div>
  );
};

export default App;
```

##### Using React Hooks with TypeScript

TypeScript can infer types for `useState` or allow explicit typing for more complex state.

```TypeScript
import React, { useState, useEffect, useRef } from 'react';

// useState
const Counter: React.FC = () => {
  // TypeScript infers the type as 'number'
  const [count, setCount] = useState(0);

  // For complex types, you can be explicit
  const [user, setUser] = useState<{ name: string; age: number } | null>(null);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      {user && <p>User: {user.name}, Age: {user.age}</p>}
    </div>
  );
};

// useRef
const FormComponent: React.FC = () => {
  const nameRef = useRef<HTMLInputElement | null>(null);

  const saveName = () => {
    if (nameRef.current) {
      console.log("Name: ", nameRef.current.value);
    }
  };

  return (
    <>
      <input
         ref={nameRef}
         type="text"
         name="name"
         placeholder="Enter your name"
      />
      <button onClick={saveName}>Save</button>
    </>
  );
};
```

##### Event Handling with TypeScript

Typing event handlers ensures that you are accessing valid properties on the event object.

```TypeScript
import React, { useState } from 'react';

const FormExample: React.FC = () => {
  const [value, setValue] = useState('');

  // Properly typed event handler
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value);
  };

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log('Submitted value:', value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
         value={value}
         onChange={handleChange}
         placeholder="Type something..."
      />
      <button type="submit">Submit</button>
    </form>
  );
};
```

### So the TypeScript setting file is about how to compile a tsx file. Am I right? And what are `module`, `lib`, and `target`? What are `es2017` or `commonjs`?

Yes, the `tsconfig.json` file provides instructions on how to compile `.tsx` and `.ts` files into JavaScript.

- **target**: This specifies the version of JavaScript the output code should conform to. For example, `es2017` means the compiled code will use JavaScript features from the 2017 standard.
- **module**: This defines the module system for handling `import` and `export` statements. `commonjs` is the system used by Node.js (`require`/`module.exports`), while `es6`/`esnext` uses the modern `import`/`export` syntax.
- **lib**: This tells TypeScript which built-in JavaScript APIs and features are available in the runtime environment. `dom` includes browser-specific APIs like `document` and `window`, while `es2017` includes features from that JavaScript version, such as `async/await`.

### Then does it mean there are different types of JavaScript and several ways to use JavaScript features?

Yes, that's correct. There are different versions of the JavaScript language specification (ECMAScript) and multiple systems for organizing code into modules.

#### JavaScript Versions (ECMAScript Standards)

JavaScript is standardized through ECMAScript (ES). New versions are released annually with new features.

- **ES5 (2009)**: The foundational version supported by nearly all browsers.
- **ES6/ES2015**: A major update that introduced classes, arrow functions, `let`, and `const`.
- **ES2017, ES2018, etc.**: Subsequent annual releases adding features like `async/await`.

The `target` option in `tsconfig.json` lets you choose which version of JavaScript your code will be compiled to.

#### Different Module Systems

- **CommonJS** (`require`/`module.exports`): The standard for Node.js.
- **ESM (ES6 Modules)** (`import`/`export`): The modern standard supported by browsers and modern bundlers.

### So, `target` means which version `tsc` is going to compile to, and `module` means which way JavaScript will import libraries, as it's dependent on the platform. Am I right?

Yes, that is a perfect summary.

- **Target**: Determines the JavaScript version of the output code. For example, `es5` for older browser compatibility or `es2017` for more modern features.
- **Module**: Dictates the import/export syntax in the compiled code, such as `commonjs` for Node.js or `es6` for modern browsers.

### Is there any relation between the target version and the module version? Also, I wonder if a module for Node.js is selected, can the compiled JavaScript file only be used on the Node.js platform and not in a client's browser?

Yes, there is a relationship between `target` and `module`, and your choice of module system can restrict where the code can run.

#### Target and Module Compatibility

Certain combinations are more effective than others. For example, compiling to an old target like `ES5` while using modern `ES6` modules can be inefficient. A bundler like Webpack or Vite is often used to resolve these incompatibilities for browser environments.

Better combinations include:

- `"target": "ES2017"` with `"module": "ES6"` for modern browsers.
- `"target": "ES2022"` with `"module": "Node16"` for modern Node.js applications.

#### Platform Restrictions with Module Choice

- **CommonJS** (`require`/`module.exports`): This format works natively in Node.js. Browsers do not understand `require`, so a bundler is necessary to convert the code to run in a browser.
- **ESM (ES6 Modules)** (`import`/`export`): This is supported by modern browsers (using `<script type="module">`) and modern versions of Node.js. It is more universal.

If you compile with `module: "CommonJS"`, the output JavaScript is intended for a Node.js environment or requires a build step with a bundler to be used in a browser.

### I'd like to use a modern setting for my React project. Which combination would you suggest?

For a modern React project, the following `tsconfig.json` configuration is recommended. It is suitable for use with modern bundlers like Vite or recent versions of Webpack.

```JSON
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["DOM", "DOM.Iterable", "ES2020"],
    "jsx": "react-jsx",
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "strict": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

#### Why This Combination is Recommended

- **`target: "ES2020"`**: Allows the use of modern JavaScript features like optional chaining (`?.`) and nullish coalescing (`??`).
- **`module: "ESNext"`** and **`moduleResolution: "bundler"`**: Utilizes the latest module syntax and delegates module resolution to your modern bundler (Vite, Parcel, Webpack 5+), which is the most efficient approach.
- **`jsx: "react-jsx"`**: Enables the new JSX transform, which means you no longer need to import React into every component file.
- **`strict: true`**: Enforces strict type-checking for higher code quality and fewer runtime errors.
- **`noEmit: true`**: Prevents TypeScript from generating output files, as this job is handled by the bundler.

---

### Summary

#### `tsconfig.json` Core Concepts

- **Purpose**: A configuration file that provides instructions to the TypeScript compiler.
- **`compilerOptions`**: The set of rules for compilation.
    - `target`: The output JavaScript version (e.g., `ES2020`).
    - `module`: The module system for imports/exports (e.g., `ESNext`, `CommonJS`).
    - `lib`: The available built-in APIs for the runtime environment (e.g., `DOM`).
    - `jsx`: How to handle JSX, with `react-jsx` being the modern standard.
    - `strict`: Enables all strict type-checking rules.
- **`include` / `exclude`**: Patterns to specify which files to compile or ignore.

#### JavaScript Versions & Module Systems

- **Versions (ECMAScript)**: JavaScript evolves with annual updates (ES5, ES6/ES2015, ES2020, etc.). `target` determines which version your code is compiled to.
- **Module Systems**:
    - **CommonJS**: For Node.js (`require`/`module.exports`). Requires a bundler for browser use.
    - **ESM (ES Modules)**: The modern standard (`import`/`export`) for browsers and newer Node.js versions.

#### Relationship and Compatibility

- `target` and `module` settings are related. Modern targets pair best with modern module systems.
- The choice of `module` can restrict the code's runtime environment (Node.js vs. Browser).

#### Modern React `tsconfig.json` Recommendation

- **`target`**: `ES2020`
- **`module`**: `ESNext`
- **`moduleResolution`**: `bundler`
- **`jsx`**: `react-jsx`
- **`strict`**: `true`
- **`noEmit`**: `true` (let the bundler handle output)