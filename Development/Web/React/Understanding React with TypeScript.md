- [x] Understanding React with TypeScript ➕ 2025-06-26 ✅ 2025-07-01

### How can I use TypeScript in React? Provide some examples.

Combining React with TypeScript enhances development by adding static type checking, which helps catch errors early, improves autocompletion, and makes code more self-documenting.

To start a new project, use: `npx create-react-app my-app --template typescript`

Remember that all files containing JSX must use the `.tsx` file extension.

#### Basic Functional Component with Props

You define the shape of a component's props using an `interface`. The `?` makes a prop optional. `React.FC` (Functional Component) is a generic type that uses your props interface.

```tsx
import React from 'react';

// Define the interface for component props
interface GreetingProps {
  name: string;
  age?: number; // Optional prop
}

const Greeting: React.FC<GreetingProps> = ({ name, age }) => {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      {age && <p>You are {age} years old.</p>}
    </div>
  );
};

export default Greeting;
```

#### Component with Event Handlers

For event handlers like `onClick`, you can type the event object itself. `React.MouseEvent<HTMLButtonElement>` specifies that the event comes from a click on a button element.

```tsx
import React from 'react';

interface ButtonProps {
  label: string;
  onClick: () => void;
  disabled?: boolean;
}

const Button: React.FC<ButtonProps> = ({ label, onClick, disabled = false }) => {
  const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    event.preventDefault();
    onClick();
  };
  
  return (
    <button onClick={handleClick} disabled={disabled}>
      {label}
    </button>
  );
};

export default Button;
```

#### State Management with the `useState` Hook

TypeScript can often infer the type of state from the initial value (e.g., `useState(0)` is inferred as a `number`). For more complex types like objects or arrays, you should provide an explicit type.

```tsx
import React, { useState } from 'react';

const Counter: React.FC = () => {
  // TypeScript infers 'number' type
  const [count, setCount] = useState(0);

  // Explicit typing for an object that can be null
  const [user, setUser] = useState<{ name: string; email: string } | null>(null);

  // Explicit typing for an array of strings
  const [items, setItems] = useState<string[]>([]);

  return (
    // ... JSX to display and update state
  );
};
```

#### State Management with the `useReducer` Hook

For more complex state logic, `useReducer` is useful. You should define types for the `state` object and all possible `actions` that can be dispatched.

```tsx
import React, { useReducer } from 'react';

// Define the state interface
interface State {
  count: number;
  loading: boolean;
}

// Define action types using a union
type CounterAction =
  | { type: 'INCREMENT' }
  | { type: 'DECREMENT' }
  | { type: 'SET_LOADING'; payload: boolean };

// The reducer function is strongly typed
const stateReducer = (state: State, action: CounterAction): State => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    // ... other cases
    default:
      return state;
  }
};

const CounterWithReducer: React.FC = () => {
  const initialState: State = { count: 0, loading: false };
  const [state, dispatch] = useReducer(stateReducer, initialState);

  return (
    // ... JSX to display state and dispatch actions
  );
};
```

#### Data Fetching Example

When fetching data, it's crucial to type the expected API response. This example types a `Todo` object and a `TodoList` (an array of `Todo` objects) and manages loading and error states.

```tsx
import React, { useState, useEffect } from 'react';

// Define interfaces for the data structure
interface Todo {
  id: number;
  title: string;
  completed: boolean;
}
type TodoList = Todo[];

const TodoListComponent: React.FC = () => {
  const [todos, setTodos] = useState<TodoList>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=5');
        const data: TodoList = await response.json();
        setTodos(data);
      } catch (err: any) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    fetchTodos();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    // ... JSX to render the list of todos
  );
};
```

### Are state, component variables, and event handlers the core of React, and do event handlers reside within components via hooks?

Yes, your understanding is correct. State is the "memory" of a component, and event handlers are the primary way to trigger changes to that state. In modern React, hooks are the mechanism that allows functional components to manage state and other React features.

#### State: The Component's Memory

State is data that persists between renders. When state is updated using its setter function (e.g., `setCount`), React re-renders the component to reflect the change. This is the core reactive principle of React.

#### Event Handlers in Functional Components

Event handlers are typically defined as regular functions inside your components. They have access to the component's state and props.

```tsx
const MyComponent = () => {
  const [count, setCount] = useState(0);

  // This is a simple event handler function
  const handleClick = () => {
    setCount(count + 1);
  };

  return <button onClick={handleClick}>Count: {count}</button>;
};
```

You only need to wrap an event handler in a hook like `useCallback` for performance optimizations, specifically to prevent it from being recreated on every render if it's passed down to a memoized child component.

### What is the difference between a regular JavaScript function and a React hook?

The defining characteristic of a React hook is that it **calls other React hooks** (like `useState`, `useEffect`, etc.). If a function does not call a hook, it is just a regular JavaScript function.

#### Key Differences

|Feature|Regular Function|React Hook|
|---|---|---|
|**Purpose**|General-purpose logic, calculations, data transformation.|To "hook into" React features like state, context, and lifecycle.|
|**Rules**|Can be called anywhere.|Must be called at the top level of a React component or another custom hook. Cannot be in loops or conditions.|
|**State**|Is stateless.|Can be stateful (using `useState`).|
|**Lifecycle**|Has no concept of the component lifecycle.|Can interact with the component lifecycle (using `useEffect`).|

#### When to Use Each

- **Use a Regular Function** for utility logic that doesn't need to interact with React's state or lifecycle (e.g., formatting a date, validating an email).
    
- **Use a Custom Hook** when you want to reuse **stateful logic** across multiple components (e.g., managing a form's state, fetching data, subscribing to an event).
    

### Can I think of React hooks as more complex and stricter functions with wide-ranging effects?

Yes, that is an excellent way to conceptualize them.

- **More Complex:** Hooks are not simple input/output functions. They interact with React's internal engine to manage state that persists across renders, trigger component updates, and handle side effects.
    
- **Affect Many Things:** A single hook call, like `useState`, can influence component rendering, memory management, and overall application behavior. `useEffect` can interact with external systems like APIs or the DOM.
    
- **More Strict:** Hooks have strict rules about where they can be called. This is because React relies on the **consistent order** of hook calls between renders to correctly associate state with its corresponding hook. Breaking these rules will lead to bugs and crashes.
    

### Can a hook in one component affect other components?

Yes, absolutely. This is fundamental to building a React application. Hooks enable components to communicate and share state through several patterns:

1. **Parent-to-Child (Props):** A parent component can use a hook (like `useState`) and pass the state value and its setter function down to child components as props.
    
2. **Context API (`useContext`):** This is the primary way to share state across the component tree without "prop drilling". A provider component manages the state with hooks, and any descendant component can consume and modify that state using the `useContext` hook.
    
3. **Custom Hooks + Context:** Combining a custom hook with the Context API is a powerful pattern for creating reusable, shared global state (e.g., `useAuth`, `useTheme`).
    
4. **External State Management:** Hooks can be used with libraries like Redux or Zustand. In this case, hooks act as the bridge between your components and a global state store that can be accessed by any component.
    

### Summary

#### Core Architecture Features

- Component-based Architecture
    
- Virtual DOM
    
- JSX (JavaScript XML)
    
- One-way Data Binding
    

#### Component Features

- Components
    
- Props
    
- State
    
- Fragments
    
- Lifecycle Methods
    
- Purity
    

#### Modern React Features

- Hooks (`useState`, `useEffect`, `useContext`, etc.)
    
- Context API
    
- Strict Mode
    

#### Key Concepts & Patterns

- Event Handling
    
- Conditional Rendering
    
- State Management (Local and Global)
    
- Server-side Rendering (SSR)
    
- API Integration
    
- Code Splitting & Lazy Loading
    
- Performance Optimization (Memoization)