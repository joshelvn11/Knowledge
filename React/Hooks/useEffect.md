# Comprehensive Guide to useEffect in React

## 1. INTRODUCTION

### What is useEffect and why it's important

The `useEffect` hook is one of React's most powerful and frequently used hooks. It provides a way to perform side effects in functional components. Side effects include data fetching, subscriptions, manual DOM manipulations, logging, and other operations that affect things outside the React component.

Before hooks were introduced in React 16.8, side effects were only possible in class components using lifecycle methods like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`. useEffect combines the functionality of these lifecycle methods into a single API, making it easier to organize and reason about side effects in your components.

### When to use useEffect and when not to use it

**Use useEffect when:**

- Fetching data from an API
- Setting up subscriptions or event listeners
- Manipulating the DOM directly
- Logging changes to state or props
- Synchronizing with external systems
- Executing code that shouldn't block the UI rendering

**Don't use useEffect when:**

- The code should run during rendering
- The code doesn't involve side effects
- You need to compute values based on state or props (use useMemo instead)
- You need to cache functions (use useCallback instead)
- You're handling events directly (use event handlers instead)

### Prerequisites

To get the most out of this tutorial, you should have:

- Basic knowledge of JavaScript (ES6+)
- Understanding of React fundamentals, including components and props
- Familiarity with React's functional components
- Basic understanding of React state (useState hook)

## 2. CORE CONCEPTS

### Fundamental principles of useEffect

1. **Effects run after render**: Unlike lifecycle methods in class components, effects scheduled with useEffect run after the render is committed to the screen, making your app feel more responsive.

2. **Declarative approach**: Instead of thinking about when effects run, you declare what they depend on. React will handle the timing of execution based on those dependencies.

3. **Cleanup functions**: Effects can return a cleanup function that runs before the component is removed from the UI and before re-running the effect due to a re-render.

4. **Multiple effects**: Components can use multiple useEffect calls to separate concerns, making your code more modular and focused.

### Key terminology and definitions

- **Side effect**: Any operation that affects something outside the scope of the current function being executed.
- **Dependency array**: An array passed as the second argument to useEffect that specifies which values from the component the effect depends on.
- **Cleanup function**: A function returned by an effect that cleans up resources or cancels subscriptions to prevent memory leaks.
- **Effect synchronization**: The process of keeping your effects in sync with the component's props and state.

### Mental model for understanding how useEffect works

Think of useEffect as a way to synchronize your React component with an external system. When certain values in your component change (those in the dependency array), the effect re-synchronizes with that external system.

The lifecycle of an effect can be broken down into these phases:

1. **Initial render**: React renders your component for the first time.
2. **After render**: React runs your effect.
3. **Cleanup before re-render**: If your effect returned a cleanup function and the component is about to re-render because dependencies changed, React runs the cleanup function.
4. **After re-render**: React runs your effect again with fresh values.
5. **Final cleanup**: When the component unmounts, React runs the cleanup function one last time.

This synchronization model helps explain why effects run when they do and provides a consistent way to think about effects regardless of their complexity.

## 3. BASIC USAGE

### Syntax and basic implementation

The basic syntax of `useEffect` is:

```jsx
useEffect(effectFunction, dependencyArray);
```

- `effectFunction`: A function containing the code to execute
- `dependencyArray`: Optional array controlling when the effect runs

There are three common patterns for using the dependency array:

1. **No dependency array**: The effect runs after every render.

   ```jsx
   useEffect(() => {
     console.log("This runs after every render");
   });
   ```

2. **Empty dependency array**: The effect runs only after the initial render.

   ```jsx
   useEffect(() => {
     console.log("This runs only once after the initial render");
   }, []);
   ```

3. **Array with dependencies**: The effect runs after the initial render and whenever any dependency changes.
   ```jsx
   useEffect(() => {
     console.log(`This runs when count (${count}) changes`);
   }, [count]);
   ```

### Step-by-step walkthrough of a simple example

Let's build a simple component that uses `useEffect` to update the document title whenever a counter changes:

```jsx
import React, { useState, useEffect } from "react";

function TitleUpdater() {
  const [count, setCount] = useState(0);

  // Effect to update the document title
  useEffect(() => {
    // This code runs after every render where count has changed
    document.title = `You clicked ${count} times`;

    // Optional cleanup function
    return () => {
      console.log("Cleaning up before next effect run or unmount");
      // In a real app, we might reset the title or clean up other resources
    };
  }, [count]); // Only re-run the effect if count changes

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

**Execution sequence:**

1. The component renders with `count` set to 0
2. React commits the changes to the DOM
3. React runs the effect, setting the document title to "You clicked 0 times"
4. When the button is clicked, `count` updates to 1
5. The component re-renders with the new count
6. React commits the changes to the DOM
7. React runs the cleanup function from the previous effect run
8. React runs the effect again, updating the title to "You clicked 1 times"

### Common patterns and best practices

#### 1. Data fetching

```jsx
import React, { useState, useEffect } from "react";

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Reset states when userId changes
    setLoading(true);
    setError(null);

    const fetchUser = async () => {
      try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        if (!response.ok) throw new Error("Failed to fetch user data");
        const data = await response.json();
        setUser(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUser();

    // Cleanup function to handle component unmount or userId change
    return () => {
      // Cancel any pending state updates
      // In a real app, you might need to cancel the fetch request if possible
    };
  }, [userId]); // Re-run effect when userId changes

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return null;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>Email: {user.email}</p>
    </div>
  );
}
```

#### 2. Subscription to external events

```jsx
import React, { useState, useEffect } from "react";

function WindowSizeTracker() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    // Event handler function
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    };

    // Add event listener
    window.addEventListener("resize", handleResize);

    // Cleanup function to remove event listener
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []); // Empty dependency array means this runs once on mount

  return (
    <div>
      <p>Window width: {windowSize.width}px</p>
      <p>Window height: {windowSize.height}px</p>
    </div>
  );
}
```

#### 3. Synchronizing with props or state

```jsx
import React, { useState, useEffect } from "react";

function AutoSaveForm({ initialText, onSave }) {
  const [text, setText] = useState(initialText);
  const [isSaving, setIsSaving] = useState(false);

  // Effect to handle auto-saving
  useEffect(() => {
    if (text === initialText) return; // Don't save if text hasn't changed

    const timeoutId = setTimeout(() => {
      setIsSaving(true);
      onSave(text)
        .then(() => setIsSaving(false))
        .catch(() => setIsSaving(false));
    }, 1000); // Debounce the save operation

    // Clean up the timeout if text changes again before timeout completes
    return () => clearTimeout(timeoutId);
  }, [text, initialText, onSave]);

  return (
    <div>
      <textarea
        value={text}
        onChange={(e) => setText(e.target.value)}
        style={{ width: "100%", height: "200px" }}
      />
      <div>{isSaving ? "Saving..." : "Changes saved"}</div>
    </div>
  );
}
```

#### Best Practices:

1. **Keep effects focused**: Each effect should do one thing well. Use multiple effects for multiple concerns.

2. **Include all dependencies**: Always include all values from the component scope that the effect uses.

3. **Avoid infinite loops**: Ensure state updates inside effects have appropriate dependency arrays.

4. **Clean up resources**: Always return a cleanup function if your effect creates resources that need to be closed.

5. **Use functional updates**: When updating state based on previous state inside an effect, use the functional form of the state updater.

6. **Stabilize functions with useCallback**: If you need to include functions in your dependency array, wrap them in useCallback to prevent unnecessary effect reruns.

## 4. ADVANCED TECHNIQUES

### Complex use cases

#### Handling dependent effects

Sometimes one effect depends on the result of another. Instead of nesting effects, use state as an intermediary:

```jsx
function UserData({ userId }) {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);

  // First effect fetches user data
  useEffect(() => {
    if (!userId) return;

    async function fetchUser() {
      const response = await fetch(`/api/users/${userId}`);
      const userData = await response.json();
      setUser(userData);
    }

    fetchUser();
  }, [userId]);

  // Second effect depends on the result of the first
  useEffect(() => {
    if (!user) return;

    async function fetchPosts() {
      const response = await fetch(`/api/users/${user.id}/posts`);
      const postsData = await response.json();
      setPosts(postsData);
    }

    fetchPosts();
  }, [user]);

  // ...render component
}
```

#### Implementing a polling mechanism

```jsx
function LiveDataFeed() {
  const [data, setData] = useState(null);
  const [polling, setPolling] = useState(true);
  const [pollingInterval, setPollingInterval] = useState(3000);

  useEffect(() => {
    if (!polling) return;

    const fetchData = async () => {
      try {
        const response = await fetch("/api/live-data");
        const newData = await response.json();
        setData(newData);
      } catch (error) {
        console.error("Polling error:", error);
      }
    };

    // Fetch immediately on mount
    fetchData();

    // Then set up the interval
    const intervalId = setInterval(fetchData, pollingInterval);

    // Clean up the interval on unmount or when polling parameters change
    return () => clearInterval(intervalId);
  }, [polling, pollingInterval]);

  return (
    <div>
      <div>Current data: {JSON.stringify(data)}</div>
      <button onClick={() => setPolling(!polling)}>
        {polling ? "Stop" : "Start"} polling
      </button>
      <select
        value={pollingInterval}
        onChange={(e) => setPollingInterval(Number(e.target.value))}
      >
        <option value={1000}>1 second</option>
        <option value={3000}>3 seconds</option>
        <option value={5000}>5 seconds</option>
      </select>
    </div>
  );
}
```

#### Managing multiple related states with useEffect

```jsx
function FilterableList({ items }) {
  const [filter, setFilter] = useState("");
  const [sortBy, setSortBy] = useState("name");
  const [filteredAndSortedItems, setFilteredAndSortedItems] = useState(items);

  // Handle filtering and sorting in a single effect
  useEffect(() => {
    let result = [...items];

    // Apply filter
    if (filter) {
      result = result.filter((item) =>
        item.name.toLowerCase().includes(filter.toLowerCase())
      );
    }

    // Apply sorting
    result.sort((a, b) => {
      if (a[sortBy] < b[sortBy]) return -1;
      if (a[sortBy] > b[sortBy]) return 1;
      return 0;
    });

    setFilteredAndSortedItems(result);
  }, [items, filter, sortBy]);

  return (
    <div>
      <input
        type="text"
        placeholder="Filter items..."
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
      />
      <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
        <option value="name">Name</option>
        <option value="date">Date</option>
        <option value="price">Price</option>
      </select>
      <ul>
        {filteredAndSortedItems.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### Performance considerations

#### Skipping effects with conditional logic

```jsx
useEffect(() => {
  // Exit early if we don't need to fetch
  if (!shouldFetch) return;

  // Rest of the effect code
  fetchData();
}, [shouldFetch, otherDependencies]);
```

#### Using refs to persist values without triggering effects

```jsx
function SearchComponent() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const lastQuery = useRef("");

  // This effect will run when query changes, but can
  // avoid unnecessary API calls by checking the ref
  useEffect(() => {
    if (query === lastQuery.current) return;

    const fetchResults = async () => {
      const response = await fetch(`/api/search?q=${query}`);
      const data = await response.json();
      setResults(data);
      lastQuery.current = query;
    };

    if (query.length >= 3) {
      fetchResults();
    }
  }, [query]);

  // Component rendering
}
```

#### Debouncing and throttling inside effects

```jsx
import React, { useState, useEffect, useRef } from "react";

function DebouncedSearch() {
  const [searchTerm, setSearchTerm] = useState("");
  const [results, setResults] = useState([]);
  const [isSearching, setIsSearching] = useState(false);

  // Use useEffect for debouncing the search
  useEffect(() => {
    if (!searchTerm) {
      setResults([]);
      return;
    }

    const debounceTimeout = setTimeout(() => {
      setIsSearching(true);

      fetch(`/api/search?q=${searchTerm}`)
        .then((res) => res.json())
        .then((data) => {
          setResults(data);
          setIsSearching(false);
        })
        .catch((err) => {
          console.error(err);
          setIsSearching(false);
        });
    }, 500); // 500ms debounce time

    // Clean up the timeout on dependency change or unmount
    return () => clearTimeout(debounceTimeout);
  }, [searchTerm]);

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      {isSearching ? (
        <p>Searching...</p>
      ) : (
        <ul>
          {results.map((item) => (
            <li key={item.id}>{item.title}</li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

#### Memoizing expensive calculations with useMemo instead of useEffect

```jsx
// Avoid this pattern
const [expensiveResult, setExpensiveResult] = useState(null);

useEffect(() => {
  // This runs after render and causes an extra re-render
  setExpensiveResult(computeExpensiveValue(a, b));
}, [a, b]);

// Use this pattern instead
const expensiveResult = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);
```

### Integration with other parts of React

#### Combining useEffect with Context

```jsx
import React, { createContext, useContext, useState, useEffect } from "react";

// Create theme context
const ThemeContext = createContext();

// Theme provider component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState(() => {
    // Initialize from localStorage if available
    return localStorage.getItem("theme") || "light";
  });

  // Persist theme changes to localStorage
  useEffect(() => {
    localStorage.setItem("theme", theme);
    // Update document body class for global styling
    document.body.className = `theme-${theme}`;
  }, [theme]);

  // Value object with theme state and setter
  const value = {
    theme,
    setTheme,
    toggleTheme: () =>
      setTheme((prevTheme) => (prevTheme === "light" ? "dark" : "light")),
  };

  return (
    <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
  );
}

// Custom hook to use the theme context
function useTheme() {
  const context = useContext(ThemeContext);
  if (context === undefined) {
    throw new Error("useTheme must be used within a ThemeProvider");
  }
  return context;
}

// Example component that uses the theme
function ThemedButton() {
  const { theme, toggleTheme } = useTheme();

  return (
    <button onClick={toggleTheme} className={`button-${theme}`}>
      Switch to {theme === "light" ? "dark" : "light"} mode
    </button>
  );
}
```

#### Creating custom hooks with useEffect

```jsx
import { useState, useEffect } from "react";

// Custom hook for window size
function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return windowSize;
}

// Custom hook for online status
function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    const handleOnline = () => setIsOnline(true);
    const handleOffline = () => setIsOnline(false);

    window.addEventListener("online", handleOnline);
    window.addEventListener("offline", handleOffline);

    return () => {
      window.removeEventListener("online", handleOnline);
      window.removeEventListener("offline", handleOffline);
    };
  }, []);

  return isOnline;
}

// Component using multiple custom hooks
function ResponsiveComponent() {
  const windowSize = useWindowSize();
  const isOnline = useOnlineStatus();

  return (
    <div>
      <p>
        Window size: {windowSize.width} x {windowSize.height}
      </p>
      <p>You are currently {isOnline ? "online" : "offline"}</p>
    </div>
  );
}
```

#### Using useEffect with useReducer for complex state logic

```jsx
import React, { useReducer, useEffect } from "react";

// Reducer function
function todosReducer(state, action) {
  switch (action.type) {
    case "FETCH_INIT":
      return { ...state, loading: true, error: null };
    case "FETCH_SUCCESS":
      return { ...state, loading: false, error: null, data: action.payload };
    case "FETCH_FAILURE":
      return { ...state, loading: false, error: action.payload };
    case "ADD_TODO":
      return { ...state, data: [...state.data, action.payload] };
    case "TOGGLE_TODO":
      return {
        ...state,
        data: state.data.map((todo) =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        ),
      };
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}

function TodoList() {
  const [state, dispatch] = useReducer(todosReducer, {
    data: [],
    loading: false,
    error: null,
  });

  // Effect for data fetching
  useEffect(() => {
    const fetchTodos = async () => {
      dispatch({ type: "FETCH_INIT" });

      try {
        const response = await fetch("/api/todos");
        const data = await response.json();
        dispatch({ type: "FETCH_SUCCESS", payload: data });
      } catch (error) {
        dispatch({ type: "FETCH_FAILURE", payload: error.message });
      }
    };

    fetchTodos();
  }, []);

  // Effect for saving to localStorage whenever todos change
  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(state.data));
  }, [state.data]);

  // Component rendering and event handlers
  // ...
}
```

## 5. COMMON PITFALLS

### Mistakes beginners often make

#### 1. Missing dependencies

One of the most common mistakes is not including all the values from the component scope that the effect function uses:

```jsx
// ❌ Bad: Missing dependency
function Example({ id }) {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData(id).then((result) => setData(result));
  }, []); // id is missing from dependency array

  // ...
}

// ✅ Good: All dependencies included
function Example({ id }) {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData(id).then((result) => setData(result));
  }, [id]); // id is properly included

  // ...
}
```

#### 2. Infinite loops

An infinite re-render loop can happen when you update state inside an effect without proper dependencies:

```jsx
// ❌ Bad: Creates an infinite loop
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // This causes a re-render, which triggers the effect again
    setCount(count + 1);
  }); // No dependency array, runs after every render

  return <div>{count}</div>;
}

// ✅ Good: Runs only once or when dependencies change
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // Only runs once after initial render
    setCount((prev) => prev + 1);
  }, []); // Empty dependency array

  return <div>{count}</div>;
}
```

#### 3. Object and array dependencies

React compares dependencies using `Object.is` (similar to `===`), so objects and arrays created during render are always considered new:

```jsx
// ❌ Bad: New object on every render causing effect to re-run
function SearchResults() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);

  // This config object is recreated on every render
  const options = {
    limit: 10,
    sort: "desc",
  };

  useEffect(() => {
    fetchResults(query, options).then((data) => setResults(data));
  }, [query, options]); // options is always new, effect always runs

  // ...
}

// ✅ Good: Move object inside effect or use primitive values
function SearchResults() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [limit] = useState(10);
  const [sort] = useState("desc");

  useEffect(() => {
    const options = { limit, sort };
    fetchResults(query, options).then((data) => setResults(data));
  }, [query, limit, sort]); // Stable primitive dependencies

  // ...
}
```

#### 4. Forgetting cleanup functions

Not cleaning up subscriptions or timers can lead to memory leaks or unexpected behavior:

```jsx
// ❌ Bad: No cleanup for event listener
function WindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);

    window.addEventListener("resize", handleResize);
    // Missing cleanup!
  }, []);

  return <div>Window width: {width}px</div>;
}

// ✅ Good: Proper cleanup of event listener
function WindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);

    window.addEventListener("resize", handleResize);

    // Return cleanup function
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return <div>Window width: {width}px</div>;
}
```

#### 5. Function dependencies

Functions created during render will be different on each render, which can cause unnecessary effect runs:

```jsx
// ❌ Bad: Function recreated on every render
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  // This function is recreated on every render
  const fetchUser = async () => {
    const response = await fetch(`/api/users/${userId}`);
    const data = await response.json();
    return data;
  };

  useEffect(() => {
    fetchUser().then((data) => setUser(data));
  }, [fetchUser]); // This causes infinite renders

  // ...
}

// ✅ Good: Use useCallback to stabilize the function
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  const fetchUser = useCallback(async () => {
    const response = await fetch(`/api/users/${userId}`);
    const data = await response.json();
    return data;
  }, [userId]); // Only changes when userId changes

  useEffect(() => {
    fetchUser().then((data) => setUser(data));
  }, [fetchUser]);

  // ...
}
```

### Debugging strategies

#### 1. Use the React DevTools

The React DevTools extension allows you to:

- See when components re-render
- Inspect component props and state
- Track hooks and their dependencies

#### 2. Add console.logs to track effect execution

```jsx
useEffect(() => {
  console.log("Effect running with values:", { count, name });

  // Effect code

  return () => {
    console.log("Cleanup running with values:", { count, name });
  };
}, [count, name]);
```

#### 3. Temporarily remove dependencies to isolate issues

```jsx
// Original effect
useEffect(() => {
  // Effect code
}, [a, b, c, d, e]);

// Debugging: Test with fewer dependencies
useEffect(() => {
  // Effect code
  // eslint-disable-next-line react-hooks/exhaustive-deps
}, [a, b]); // Temporarily removing c, d, and e to see if they cause the issue
```

#### 4. Split complex effects into smaller ones

```jsx
// ❌ Bad: One complex effect doing too many things
useEffect(() => {
  // Fetch user data
  fetch(`/api/users/${userId}`)
    .then((res) => res.json())
    .then(setUser);

  // Fetch posts data
  fetch(`/api/posts?userId=${userId}`)
    .then((res) => res.json())
    .then(setPosts);

  // Set up event listeners
  window.addEventListener("resize", handleResize);

  return () => {
    window.removeEventListener("resize", handleResize);
  };
}, [userId]);

// ✅ Good: Split into focused effects
// Effect for user data
useEffect(() => {
  fetch(`/api/users/${userId}`)
    .then((res) => res.json())
    .then(setUser);
}, [userId]);

// Effect for posts data
useEffect(() => {
  fetch(`/api/posts?userId=${userId}`)
    .then((res) => res.json())
    .then(setPosts);
}, [userId]);

// Effect for event listener
useEffect(() => {
  window.addEventListener("resize", handleResize);
  return () => {
    window.removeEventListener("resize", handleResize);
  };
}, []);
```

#### 5. Use ESLint with react-hooks/exhaustive-deps rule

The `react-hooks/exhaustive-deps` ESLint rule will warn you about missing dependencies in your useEffect calls. Make sure it's enabled in your ESLint configuration.

### How to recognize and solve typical issues

#### Issue 1: Stale closures

Stale closures happen when your effect captures outdated values from previous renders:

```jsx
// ❌ Problem: Stale closure
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      console.log(`Current count: ${count}`);
      setCount(count + 1); // Always uses the initial count value
    }, 1000);

    return () => clearInterval(interval);
  }, []); // Empty dependency array

  return <div>{count}</div>;
}

// ✅ Solution: Use functional updates or include dependencies
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount((prevCount) => prevCount + 1); // Uses the latest count
    }, 1000);

    return () => clearInterval(interval);
  }, []); // Empty dependency array is fine with functional updates

  return <div>{count}</div>;
}
```

#### Issue 2: Race conditions in data fetching

When fetching data, a slow request followed by a fast one can cause outdated data to overwrite newer data:

```jsx
// ❌ Problem: Possible race condition
function SearchResults({ query }) {
  const [results, setResults] = useState([]);

  useEffect(() => {
    fetch(`/api/search?q=${query}`)
      .then((res) => res.json())
      .then((data) => {
        // This might be from an outdated request
        setResults(data);
      });
  }, [query]);

  // ...
}

// ✅ Solution: Track current request or use AbortController
function SearchResults({ query }) {
  const [results, setResults] = useState([]);

  useEffect(() => {
    let isCurrent = true;

    fetch(`/api/search?q=${query}`)
      .then((res) => res.json())
      .then((data) => {
        if (isCurrent) {
          setResults(data);
        }
      });

    // Clean up function sets flag to false
    return () => {
      isCurrent = false;
    };
  }, [query]);

  // ...
}
```

#### Issue 3: Effect triggering too frequently

When effects run too often, it can cause performance issues or unexpected behavior:

```jsx
// ❌ Problem: Effect runs on every render
function DataFetcher() {
  const [data, setData] = useState(null);

  // This runs after every render
  useEffect(() => {
    fetchData().then(setData);
  });

  // ...
}

// ✅ Solution: Add appropriate dependencies
function DataFetcher() {
  const [data, setData] = useState(null);

  // This runs only once after mount
  useEffect(() => {
    fetchData().then(setData);
  }, []);

  // ...
}
```

#### Issue 4: Complex dependency arrays

When your effect has many dependencies, it might indicate a need to refactor:

```jsx
// ❌ Problem: Too many dependencies
function UserDashboard({ user, settings, theme, notifications, permissions }) {
  useEffect(() => {
    // Complex effect with many dependencies
    // ...
  }, [
    user,
    settings.display,
    settings.privacy,
    theme.color,
    notifications.length,
    permissions.canEdit,
    permissions.canDelete,
  ]);

  // ...
}

// ✅ Solution: Split into multiple focused effects
function UserDashboard({ user, settings, theme, notifications, permissions }) {
  // Effect for user-related logic
  useEffect(() => {
    // ...
  }, [user]);

  // Effect for settings-related logic
  useEffect(() => {
    // ...
  }, [settings.display, settings.privacy]);

  // Effect for theme-related logic
  useEffect(() => {
    // ...
  }, [theme.color]);

  // ...and so on
}
```

#### Issue 5: Repeated API calls

Making the same API call multiple times unnecessarily:

```jsx
// ❌ Problem: API call on every render
function ProfilePage({ userId }) {
  const [user, setUser] = useState(null);

  // This runs after every render
  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then((res) => res.json())
      .then(setUser);
  });

  // ...
}

// ✅ Solution: Add dependency array and cache
function ProfilePage({ userId }) {
  const [user, setUser] = useState(null);
  const [fetchedIds, setFetchedIds] = useState(new Set());

  useEffect(() => {
    if (fetchedIds.has(userId)) return;

    fetch(`/api/users/${userId}`)
      .then((res) => res.json())
      .then((userData) => {
        setUser(userData);
        setFetchedIds((prev) => new Set(prev).add(userId));
      });
  }, [userId, fetchedIds]);

  // ...
}
```

## 6. PRACTICAL EXAMPLES

### Example 1: Data Fetching with Loading States

This example shows how to implement a complete data fetching pattern with loading states, error handling, and cleanup:

```jsx
import React, { useState, useEffect } from "react";

function ProductDetails({ productId }) {
  const [product, setProduct] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Reset states when product ID changes
    setLoading(true);
    setError(null);
    setProduct(null);

    // Create an AbortController for cleanup
    const controller = new AbortController();
    const signal = controller.signal;

    const fetchProduct = async () => {
      try {
        const response = await fetch(
          `https://api.example.com/products/${productId}`,
          { signal } // Connect the abort signal to the fetch request
        );

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();
        setProduct(data);
        setLoading(false);
      } catch (err) {
        // Only set error if not aborted
        if (err.name !== "AbortError") {
          setError(err.message);
          setLoading(false);
        }
      }
    };

    fetchProduct();

    // Clean up function to abort fetch when unmounting or productId changes
    return () => {
      controller.abort();
    };
  }, [productId]); // Re-run when productId changes

  if (loading) {
    return <div className="loading-spinner">Loading...</div>;
  }

  if (error) {
    return <div className="error-message">Error: {error}</div>;
  }

  if (!product) {
    return null;
  }

  return (
    <div className="product-card">
      <h2>{product.name}</h2>
      <p className="price">${product.price.toFixed(2)}</p>
      <img src={product.imageUrl} alt={product.name} />
      <p className="description">{product.description}</p>
      <button className="add-to-cart">Add to Cart</button>
    </div>
  );
}
```

This example demonstrates:

- Resetting states when dependencies change
- Using AbortController to cancel in-flight requests
- Proper loading and error states
- Clean rendering logic based on state

### Example 2: Form with Auto-Save Functionality

This example shows how to create a form with auto-save functionality using debounced effects:

```jsx
import React, { useState, useEffect, useCallback } from "react";

function EditableDocument({ documentId, initialContent }) {
  const [content, setContent] = useState(initialContent);
  const [saving, setSaving] = useState(false);
  const [lastSaved, setLastSaved] = useState(null);
  const [saveError, setSaveError] = useState(null);

  // Create a stable save function with useCallback
  const saveDocument = useCallback(
    async (text) => {
      try {
        setSaving(true);

        // Simulate API call to save document
        const response = await fetch(`/api/documents/${documentId}`, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({ content: text }),
        });

        if (!response.ok) {
          throw new Error("Failed to save");
        }

        // Update last saved timestamp
        setLastSaved(new Date());
        setSaveError(null);
      } catch (err) {
        setSaveError(err.message);
      } finally {
        setSaving(false);
      }
    },
    [documentId]
  );

  // Auto-save effect with debounce
  useEffect(() => {
    if (content === initialContent) return; // Don't save if unchanged

    const timeoutId = setTimeout(() => {
      saveDocument(content);
    }, 1000); // 1 second debounce

    // Clean up the timeout if content changes again
    return () => clearTimeout(timeoutId);
  }, [content, initialContent, saveDocument]);

  return (
    <div className="document-editor">
      <div className="editor-header">
        <span className="status">
          {saving
            ? "Saving..."
            : saveError
            ? `Error: ${saveError}`
            : lastSaved
            ? `Last saved: ${lastSaved.toLocaleTimeString()}`
            : "Not saved yet"}
        </span>
      </div>

      <textarea
        className="editor-textarea"
        value={content}
        onChange={(e) => setContent(e.target.value)}
        placeholder="Start typing..."
        rows={10}
      />

      <div className="editor-footer">
        <button
          className="save-button"
          onClick={() => saveDocument(content)}
          disabled={saving || content === initialContent}
        >
          Save Now
        </button>
      </div>
    </div>
  );
}
```

This example demonstrates:

- Debounced auto-saving with useEffect
- Manual save option
- Status indicators for saving state
- Optimizing with useCallback
- Skipping unnecessary saves

### Example 3: Real-time Data Synchronization with WebSockets

This example shows how to use useEffect to manage a WebSocket connection for real-time updates:

```jsx
import React, { useState, useEffect, useRef } from "react";

function ChatRoom({ roomId, username }) {
  const [messages, setMessages] = useState([]);
  const [connectionStatus, setConnectionStatus] = useState("disconnected");
  const [newMessage, setNewMessage] = useState("");
  const socketRef = useRef(null);

  // Effect for WebSocket connection
  useEffect(() => {
    // Connection setup function
    const setupConnection = () => {
      setConnectionStatus("connecting");

      // Create WebSocket connection
      const socket = new WebSocket(`wss://chat.example.com/rooms/${roomId}`);
      socketRef.current = socket;

      // Connection opened
      socket.addEventListener("open", () => {
        setConnectionStatus("connected");

        // Send join message to server
        socket.send(
          JSON.stringify({
            type: "join",
            username,
            roomId,
          })
        );
      });

      // Listen for messages
      socket.addEventListener("message", (event) => {
        const data = JSON.parse(event.data);

        if (data.type === "message") {
          setMessages((prev) => [...prev, data]);
        } else if (data.type === "history") {
          setMessages(data.messages);
        }
      });

      // Connection closed or error
      socket.addEventListener("close", () => {
        setConnectionStatus("disconnected");

        // Try to reconnect after a delay if the component is still mounted
        setTimeout(() => {
          if (socketRef.current === socket) {
            setupConnection();
          }
        }, 3000);
      });

      socket.addEventListener("error", () => {
        setConnectionStatus("error");
      });
    };

    // Initialize connection
    setupConnection();

    // Cleanup function to close WebSocket when unmounting or roomId changes
    return () => {
      const socket = socketRef.current;
      if (socket) {
        // Send leave message
        if (socket.readyState === WebSocket.OPEN) {
          socket.send(
            JSON.stringify({
              type: "leave",
              username,
              roomId,
            })
          );
        }

        // Close connection
        socket.close();
        socketRef.current = null;
      }
    };
  }, [roomId, username]);

  // Function to send a new message
  const sendMessage = (e) => {
    e.preventDefault();

    if (!newMessage.trim() || connectionStatus !== "connected") {
      return;
    }

    const socket = socketRef.current;
    if (socket && socket.readyState === WebSocket.OPEN) {
      socket.send(
        JSON.stringify({
          type: "message",
          content: newMessage,
          username,
          roomId,
          timestamp: new Date().toISOString(),
        })
      );

      setNewMessage("");
    }
  };

  return (
    <div className="chat-room">
      <div className="chat-header">
        <h2>Room: {roomId}</h2>
        <div className={`connection-status status-${connectionStatus}`}>
          {connectionStatus === "connected"
            ? "Connected"
            : connectionStatus === "connecting"
            ? "Connecting..."
            : connectionStatus === "error"
            ? "Connection Error"
            : "Disconnected"}
        </div>
      </div>

      <div className="message-list">
        {messages.length === 0 ? (
          <div className="no-messages">No messages yet</div>
        ) : (
          messages.map((msg, index) => (
            <div
              key={index}
              className={`message ${
                msg.username === username ? "own-message" : ""
              }`}
            >
              <div className="message-header">
                <span className="username">{msg.username}</span>
                <span className="timestamp">
                  {new Date(msg.timestamp).toLocaleTimeString()}
                </span>
              </div>
              <div className="message-content">{msg.content}</div>
            </div>
          ))
        )}
      </div>

      <form className="message-form" onSubmit={sendMessage}>
        <input
          type="text"
          placeholder="Type a message..."
          value={newMessage}
          onChange={(e) => setNewMessage(e.target.value)}
          disabled={connectionStatus !== "connected"}
        />
        <button
          type="submit"
          disabled={!newMessage.trim() || connectionStatus !== "connected"}
        >
          Send
        </button>
      </form>
    </div>
  );
}
```

This example demonstrates:

- Managing WebSocket connections in useEffect
- Cleaning up connections when unmounting
- Using refs to track the current WebSocket instance
- Connection state management
- Automatic reconnection logic
- Real-time data handling

### Example 4: Infinite Scroll with Intersection Observer

This example shows how to implement infinite scrolling using the Intersection Observer API with useEffect:

```jsx
import React, { useState, useEffect, useRef, useCallback } from "react";

function InfiniteScroll() {
  const [posts, setPosts] = useState([]);
  const [page, setPage] = useState(1);
  const [loading, setLoading] = useState(false);
  const [hasMore, setHasMore] = useState(true);
  const [error, setError] = useState(null);

  // Reference for the observer
  const observer = useRef();

  // Last element ref callback function
  const lastPostElementRef = useCallback(
    (node) => {
      if (loading) return;

      if (observer.current) observer.current.disconnect();

      observer.current = new IntersectionObserver((entries) => {
        if (entries[0].isIntersecting && hasMore) {
          setPage((prevPage) => prevPage + 1);
        }
      });

      if (node) observer.current.observe(node);
    },
    [loading, hasMore]
  );

  // Effect for fetching posts
  useEffect(() => {
    setLoading(true);
    setError(null);

    const fetchPosts = async () => {
      try {
        const response = await fetch(
          `https://api.example.com/posts?page=${page}&limit=10`
        );

        if (!response.ok) {
          throw new Error("Failed to fetch posts");
        }

        const data = await response.json();

        setPosts((prevPosts) => {
          // Merge with previous posts, filtering duplicates by id
          const newPosts = [...prevPosts];
          const existingIds = new Set(prevPosts.map((post) => post.id));

          data.posts.forEach((post) => {
            if (!existingIds.has(post.id)) {
              newPosts.push(post);
            }
          });

          return newPosts;
        });

        setHasMore(data.posts.length > 0);
        setLoading(false);
      } catch (err) {
        setError(err.message);
        setLoading(false);
      }
    };

    fetchPosts();
  }, [page]);

  return (
    <div className="infinite-scroll-container">
      <h1>Infinite Scroll Posts</h1>

      <div className="posts-list">
        {posts.map((post, index) => {
          const isLastElement = index === posts.length - 1;

          return (
            <div
              key={post.id}
              ref={isLastElement ? lastPostElementRef : null}
              className="post-card"
            >
              <h2>{post.title}</h2>
              <p className="post-excerpt">{post.excerpt}</p>
              <div className="post-footer">
                <span className="post-author">By {post.author}</span>
                <span className="post-date">
                  {new Date(post.date).toLocaleDateString()}
                </span>
              </div>
            </div>
          );
        })}
      </div>

      {loading && (
        <div className="loading-indicator">Loading more posts...</div>
      )}

      {error && <div className="error-message">Error: {error}</div>}

      {!hasMore && !loading && posts.length > 0 && (
        <div className="end-message">No more posts to load</div>
      )}
    </div>
  );
}
```

This example demonstrates:

- Using Intersection Observer API with useEffect
- Implementing infinite scrolling
- Managing pagination state
- Handling loading states
- Deduplicating data
- Using refs and callback refs together
- Cleanup of observers

## 7. REAL-WORLD APPLICATIONS

### How useEffect is used in production environments

#### Authentication and session management

In many production applications, useEffect is used to manage user authentication state:

```jsx
function App() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  // Effect to check auth status on app load
  useEffect(() => {
    const checkAuthStatus = async () => {
      try {
        // Check for an existing auth token
        const token = localStorage.getItem("auth_token");

        if (!token) {
          setLoading(false);
          return;
        }

        // Validate token with the server
        const response = await fetch("/api/auth/verify", {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        });

        if (response.ok) {
          const userData = await response.json();
          setUser(userData);
        } else {
          // Token invalid, remove it
          localStorage.removeItem("auth_token");
        }
      } catch (error) {
        console.error("Auth verification failed:", error);
      } finally {
        setLoading(false);
      }
    };

    checkAuthStatus();
  }, []);

  // Auth context provider
  return (
    <AuthContext.Provider value={{ user, setUser, loading }}>
      {loading ? <LoadingScreen /> : <MainRouter />}
    </AuthContext.Provider>
  );
}
```

#### Analytics and monitoring

useEffect is commonly used to initialize and manage analytics:

```jsx
function AnalyticsProvider({ children }) {
  // Effect to initialize analytics
  useEffect(() => {
    // Initialize analytics service
    analytics.init({
      appId: process.env.REACT_APP_ANALYTICS_ID,
      version: APP_VERSION,
      environment: process.env.NODE_ENV,
    });

    // Track app loaded event
    analytics.trackEvent("app_loaded");

    // Clean up on unmount
    return () => {
      analytics.flush(); // Send any pending events
    };
  }, []);

  return (
    <AnalyticsContext.Provider value={analytics}>
      {children}
    </AnalyticsContext.Provider>
  );
}

// Component for page view tracking
function PageViewTracker({ path, title }) {
  useEffect(() => {
    // Track page view
    analytics.trackPageView({
      path,
      title,
      referrer: document.referrer,
    });
  }, [path, title]);

  return null; // Renders nothing
}
```

#### Feature flags and A/B testing

useEffect is used to load and manage feature flags or A/B test variations:

```jsx
function FeatureFlagProvider({ children }) {
  const [flags, setFlags] = useState({});
  const [loading, setLoading] = useState(true);

  // Effect to fetch feature flags
  useEffect(() => {
    const fetchFlags = async () => {
      try {
        const userId = getUserId(); // Get current user ID

        const response = await fetch(
          `https://flags.example.com/api/flags?userId=${userId}`
        );

        const data = await response.json();
        setFlags(data.flags);
      } catch (error) {
        console.error("Failed to fetch feature flags:", error);
        // Fallback to default flags
        setFlags(DEFAULT_FLAGS);
      } finally {
        setLoading(false);
      }
    };

    fetchFlags();
  }, []);

  // Check if a feature is enabled
  const isFeatureEnabled = useCallback(
    (featureKey) => {
      return flags[featureKey] === true;
    },
    [flags]
  );

  return (
    <FeatureFlagContext.Provider value={{ isFeatureEnabled, loading }}>
      {children}
    </FeatureFlagContext.Provider>
  );
}
```

### Case studies or examples from popular projects

#### Next.js Data Fetching

Next.js uses useEffect for client-side data fetching when SSR isn't needed:

```jsx
// pages/dashboard.js in a Next.js app
import { useState, useEffect } from "react";
import DashboardLayout from "../components/layouts/DashboardLayout";
import UsageMetrics from "../components/dashboard/UsageMetrics";
import RecentActivity from "../components/dashboard/RecentActivity";

export default function Dashboard() {
  const [dashboardData, setDashboardData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Only run on client-side
    const fetchDashboardData = async () => {
      try {
        // Fetch data from API
        const response = await fetch("/api/dashboard", {
          credentials: "include", // Include cookies for auth
        });

        if (!response.ok) {
          throw new Error("Failed to fetch dashboard data");
        }

        const data = await response.json();
        setDashboardData(data);
      } catch (error) {
        console.error("Dashboard fetch error:", error);
      } finally {
        setLoading(false);
      }
    };

    fetchDashboardData();
  }, []);

  return (
    <DashboardLayout>
      {loading ? (
        <LoadingSpinner />
      ) : (
        <>
          <UsageMetrics data={dashboardData?.metrics} />
          <RecentActivity activities={dashboardData?.recentActivities} />
        </>
      )}
    </DashboardLayout>
  );
}
```

#### React Router's useEffect Pattern

React Router uses useEffect to handle navigation events and history changes:

```jsx
import { useEffect } from "react";
import { useHistory, useLocation } from "react-router-dom";

// Example custom hook inspired by React Router
function useScrollToTop() {
  const { pathname } = useLocation();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);
}

// Analytics integration with React Router
function RouterAnalytics() {
  const location = useLocation();

  useEffect(() => {
    // Track page view on route change
    analytics.trackPageView({
      path: location.pathname,
      search: location.search,
    });
  }, [location]);

  return null; // This component doesn't render anything
}
```

#### React Query's useQuery Implementation

React Query, a popular data-fetching library, uses useEffect internally in its useQuery hook:

```jsx
// Simplified version of how React Query uses useEffect internally
function useCustomQuery(queryKey, queryFn, options = {}) {
  const [state, setState] = useState({
    data: null,
    error: null,
    status: "idle",
  });

  // Generate a stable cache key from the query key
  const cacheKey = JSON.stringify(queryKey);

  useEffect(() => {
    // Set to loading state
    setState((prev) => ({ ...prev, status: "loading" }));

    // Check cache first
    const cachedData = queryCache.get(cacheKey);
    if (cachedData && !options.skipCache) {
      setState({
        data: cachedData,
        error: null,
        status: "success",
      });

      // Still fetch in background if stale
      if (isCacheStale(cacheKey)) {
        fetchData();
      }
      return;
    }

    // Fetch data if no cache
    let isCancelled = false;

    async function fetchData() {
      try {
        const data = await queryFn();

        if (!isCancelled) {
          // Update state and cache
          setState({
            data,
            error: null,
            status: "success",
          });

          queryCache.set(cacheKey, data, options.cacheTime);
        }
      } catch (error) {
        if (!isCancelled) {
          setState({
            data: null,
            error,
            status: "error",
          });
        }
      }
    }

    fetchData();

    // Set up refetch interval if specified
    let intervalId;
    if (options.refetchInterval) {
      intervalId = setInterval(fetchData, options.refetchInterval);
    }

    // Cleanup function
    return () => {
      isCancelled = true;
      if (intervalId) clearInterval(intervalId);
    };
  }, [cacheKey, queryFn, options.skipCache, options.refetchInterval]);

  return state;
}
```

### Integration with common libraries/frameworks

#### Using useEffect with Redux

```jsx
import { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { fetchUserData, selectUserData, selectUserStatus } from "./userSlice";

function UserProfile({ userId }) {
  const dispatch = useDispatch();
  const userData = useSelector(selectUserData);
  const status = useSelector(selectUserStatus);

  // Fetch user data when component mounts or userId changes
  useEffect(() => {
    if (status !== "loading") {
      dispatch(fetchUserData(userId));
    }
  }, [userId, status, dispatch]);

  // Render component based on data and status
  // ...
}
```

#### Using useEffect with i18n (Internationalization)

```jsx
import { useEffect } from "react";
import { useTranslation } from "react-i18next";

function LanguageHandler({ language }) {
  const { i18n } = useTranslation();

  // Change language when prop changes
  useEffect(() => {
    if (i18n.language !== language) {
      i18n.changeLanguage(language);
    }
  }, [language, i18n]);

  return null;
}

// Document title translation
function PageTitle({ titleKey, values }) {
  const { t } = useTranslation();

  useEffect(() => {
    // Translate and set the document title
    document.title = t(titleKey, values);
  }, [t, titleKey, values]);

  return null;
}
```

#### Using useEffect with GraphQL (Apollo Client)

```jsx
import { useEffect } from "react";
import { useLazyQuery } from "@apollo/client";
import { GET_USER } from "./queries";

function UserProfile({ userId }) {
  const [getUserData, { loading, data, error }] = useLazyQuery(GET_USER);

  // Fetch user data when component mounts or userId changes
  useEffect(() => {
    if (userId) {
      getUserData({ variables: { id: userId } });
    }
  }, [userId, getUserData]);

  // Handle loading, error, and rendering...
}
```

## 8. ADDITIONAL RESOURCES

### Official documentation

- [React Docs: Using the Effect Hook](https://react.dev/reference/react/useEffect) - Complete reference for useEffect
- [React Docs: Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects) - In-depth guide on effects
- [React Docs: You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect) - When to avoid using effects

### Related topics to explore next

- **useState and State Management**: Understanding how to manage component state
- **useReducer**: For complex state logic that involves multiple sub-values
- **useCallback**: To optimize performance by preventing unnecessary re-renders
- **useMemo**: For memoizing expensive calculations
- **Custom Hooks**: Creating reusable hooks by combining React's built-in hooks
- **useRef**: For persistent values that don't cause re-renders
- **Context API**: For global state management across components
- **React Query/SWR**: Data fetching libraries that build on React's hooks

### Community resources

#### Articles and Tutorials

- [A Complete Guide to useEffect](https://overreacted.io/a-complete-guide-to-useeffect/) by Dan Abramov
- [How to Fetch Data with React Hooks](https://www.robinwieruch.de/react-hooks-fetch-data/) by Robin Wieruch
- [React Hooks: Compound Components](https://kentcdodds.com/blog/compound-components-with-react-hooks) by Kent C. Dodds

#### Libraries and Utilities

- [React Query](https://react-query.tanstack.com/) - Data fetching and caching library
- [SWR](https://swr.vercel.app/) - React Hooks library for data fetching
- [use-http](https://use-http.com/) - React hook for making HTTP requests
- [react-use](https://github.com/streamich/react-use) - Collection of essential React Hooks
- [ahooks](https://ahooks.js.org/) - A high-quality Hooks library

#### Courses and Videos

- [Epic React](https://epicreact.dev/) by Kent C. Dodds - Comprehensive React training
- [React Hooks Explained](https://www.youtube.com/watch?v=TNhaISOUy6Q) by Fireship
- [React Hooks Course](https://ui.dev/react-hooks/) by Tyler McGinnis

#### Tools

- [eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks) - ESLint plugin to enforce React Hooks rules
- [React DevTools](https://react.dev/learn/react-developer-tools) - Browser extension for debugging React applications
- [why-did-you-render](https://github.com/welldone-software/why-did-you-render) - Notifies you about potentially avoidable re-renders

#### Community Forums

- [Reactiflux Discord](https://www.reactiflux.com/) - Chat community of React developers
- [r/reactjs](https://www.reddit.com/r/reactjs/) - React subreddit
- [Stack Overflow React Questions](https://stackoverflow.com/questions/tagged/reactjs) - Q&A for React development
