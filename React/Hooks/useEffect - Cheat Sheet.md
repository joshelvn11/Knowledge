# useEffect React Hook Cheat Sheet

## 1. CORE SYNTAX

### Basic Syntax Patterns

```jsx
// Basic syntax
useEffect(
  () => {
    // Effect code runs after render

    // Optional cleanup function
    return () => {
      // Cleanup code runs before unmount or before next effect
    };
  },
  [
    /* dependency array */
  ]
);
```

### Dependency Array Patterns

```jsx
// Runs after every render
useEffect(() => {
  console.log("Rendered");
});

// Runs only once after initial render (componentDidMount)
useEffect(() => {
  console.log("Component mounted");
}, []);

// Runs when dependencies change (componentDidUpdate)
useEffect(() => {
  console.log(`Count changed to: ${count}`);
}, [count]);

// Cleanup before unmount (componentWillUnmount)
useEffect(() => {
  return () => {
    console.log("Component will unmount");
  };
}, []);
```

### Required Imports

```jsx
import React, { useEffect } from "react";
```

### Most Common Operations

- **Data fetching**
- **Subscriptions to external sources**
- **DOM manipulations**
- **Side effects after state/prop changes**
- **Cleanup of resources (event listeners, timers)**

## 2. KEY CONCEPTS

### Essential Terminology

| Term                 | Definition                                                                                   |
| -------------------- | -------------------------------------------------------------------------------------------- |
| **Side effect**      | Any operation that affects something outside the scope of the current function               |
| **Cleanup function** | Function returned by the effect that runs before unmount or before the next effect execution |
| **Dependency array** | List of values that the effect depends on; when they change, the effect reruns               |
| **Stale closure**    | When an effect captures outdated values from previous renders                                |
| **Race condition**   | When a slow asynchronous operation completes after a faster one that started later           |

### Core Principles

- **Effects run after render** is committed to the screen
- **Dependencies control when effects run**, not whether they run
- **Every render has its own effects** with its own props and state values
- **Cleanup functions prevent memory leaks** and stale state issues
- **Effects should be focused on a single concern** for better maintainability

### Mental Model

```
Component Lifecycle with useEffect:
┌─────────────────┐
│  Initial Render  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Effect Runs    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐    ┌─────────────────┐
│  State Changes   │◄───┤   User Action   │
└────────┬────────┘    └─────────────────┘
         │
         ▼
┌─────────────────┐
│    Re-render     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Cleanup Previous │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Effect Runs    │
└────────┬────────┘
         │
         ▼
     (repeat)
```

## 3. COMMON PATTERNS

### Data Fetching

```jsx
// Basic data fetching
useEffect(() => {
  let isMounted = true;

  const fetchData = async () => {
    try {
      const response = await fetch(`https://api.example.com/data/${id}`);
      const data = await response.json();

      if (isMounted) {
        setData(data);
        setLoading(false);
      }
    } catch (error) {
      if (isMounted) {
        setError(error);
        setLoading(false);
      }
    }
  };

  fetchData();

  return () => {
    isMounted = false; // Prevent setting state on unmounted component
  };
}, [id]);
```

### Event Listener Management

```jsx
// Adding and removing event listeners
useEffect(() => {
  const handleResize = () => {
    setWindowWidth(window.innerWidth);
  };

  window.addEventListener("resize", handleResize);

  return () => {
    window.removeEventListener("resize", handleResize);
  };
}, []);
```

### Timer Management

```jsx
// Setting up and clearing intervals
useEffect(() => {
  const intervalId = setInterval(() => {
    setCount((c) => c + 1);
  }, 1000);

  return () => {
    clearInterval(intervalId);
  };
}, []);
```

### Debouncing User Input

```jsx
// Debouncing search input
useEffect(() => {
  if (!searchTerm) return;

  const timeoutId = setTimeout(() => {
    fetchSearchResults(searchTerm);
  }, 500);

  return () => {
    clearTimeout(timeoutId);
  };
}, [searchTerm, fetchSearchResults]);
```

### Synchronizing with localStorage

```jsx
// Loading from localStorage on mount
useEffect(() => {
  const savedValue = localStorage.getItem("key");
  if (savedValue) {
    setValue(JSON.parse(savedValue));
  }
}, []);

// Saving to localStorage when value changes
useEffect(() => {
  localStorage.setItem("key", JSON.stringify(value));
}, [value]);
```

## 4. QUICK REFERENCE

### useEffect Parameters

| Parameter        | Type     | Required | Description                          |
| ---------------- | -------- | -------- | ------------------------------------ |
| Effect function  | Function | Yes      | Function containing side effect code |
| Dependency array | Array    | No       | Values that the effect depends on    |

### Effect Function Behavior

| Dependency Array     | Behavior                                                             |
| -------------------- | -------------------------------------------------------------------- |
| Not provided         | Effect runs after every render                                       |
| Empty array `[]`     | Effect runs only once after initial render                           |
| With values `[a, b]` | Effect runs after initial render and when any value in array changes |

### Common useEffect Mistakes

| Mistake                   | Solution                                                        |
| ------------------------- | --------------------------------------------------------------- |
| Missing dependencies      | Use the exhaustive-deps ESLint rule                             |
| Infinite loops            | Check dependency array and avoid updating state unconditionally |
| Object/array dependencies | Use primitive values or useMemo                                 |
| Missing cleanup           | Return cleanup function for subscriptions and timers            |

## 5. CODE SAMPLES

### 1. Document Title Update

```jsx
// Update document title when count changes
function DocumentTitleExample({ title }) {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // Update the document title using the browser API
    document.title = `${title}: ${count} clicks`;
  }, [count, title]); // Re-run when count or title changes

  return (
    <button onClick={() => setCount(count + 1)}>Clicked {count} times</button>
  );
}
```

### 2. Data Fetching with Async/Await

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Reset states when userId changes
    setLoading(true);
    setError(null);

    // Define async function inside effect
    async function fetchUser() {
      try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        if (!response.ok) throw new Error("Failed to fetch");
        const data = await response.json();
        setUser(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }

    fetchUser(); // Call the async function
  }, [userId]); // Re-run when userId changes

  // Render based on loading/error/data state
}
```

### 3. Form with Auto-Save

```jsx
function AutoSaveForm({ initialText, onSave }) {
  const [text, setText] = useState(initialText);
  const [saving, setSaving] = useState(false);

  // Auto-save effect
  useEffect(() => {
    if (text === initialText) return; // Don't save if unchanged

    // Debounce save operation
    const timeoutId = setTimeout(() => {
      setSaving(true);
      onSave(text)
        .then(() => setSaving(false))
        .catch(() => setSaving(false));
    }, 1000);

    // Clean up timeout if text changes again before save
    return () => clearTimeout(timeoutId);
  }, [text, initialText, onSave]);

  return (
    <div>
      <textarea value={text} onChange={(e) => setText(e.target.value)} />
      <div>{saving ? "Saving..." : "Changes saved"}</div>
    </div>
  );
}
```

### 4. Custom Hook for Window Size

```jsx
// Custom hook using useEffect
function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    // Handler to call on window resize
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    }

    // Add event listener
    window.addEventListener("resize", handleResize);

    // Call handler right away to update initial size
    handleResize();

    // Remove event listener on cleanup
    return () => window.removeEventListener("resize", handleResize);
  }, []); // Empty array ensures effect runs only on mount and unmount

  return windowSize;
}

// Usage
function WindowSizeDisplay() {
  const size = useWindowSize();
  return (
    <p>
      Window size: {size.width} x {size.height}
    </p>
  );
}
```

### 5. Polling Data with Intervals

```jsx
function LiveDataFeed() {
  const [data, setData] = useState(null);
  const [polling, setPolling] = useState(true);

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
    const intervalId = setInterval(fetchData, 5000);

    // Clear interval on unmount or when polling changes
    return () => clearInterval(intervalId);
  }, [polling]);

  return (
    <div>
      <div>Current data: {JSON.stringify(data)}</div>
      <button onClick={() => setPolling(!polling)}>
        {polling ? "Stop" : "Start"} polling
      </button>
    </div>
  );
}
```

## 6. TROUBLESHOOTING

### Common Errors and Solutions

| Error                                | Solution                                                                  |
| ------------------------------------ | ------------------------------------------------------------------------- |
| **Infinite re-rendering**            | Check for state updates in an effect without proper dependencies          |
| **Stale state or props in effect**   | Ensure all values from component scope are in the dependency array        |
| **Effect runs twice in development** | Normal with React.StrictMode, don't rely on effects running exactly once  |
| **Multiple effects interfering**     | Split complex effects into smaller, focused ones with proper dependencies |
| **Can't update state after unmount** | Use cleanup function to track component mounted state                     |

### Debugging Tips

- Use `console.log` at the start and end of effects to track execution
- Add logs to cleanup functions to verify they're running
- Temporarily remove dependencies to isolate which one is causing issues
- Use React DevTools to track component renders and state changes
- Use ESLint with `react-hooks/exhaustive-deps` rule to catch missing dependencies

### Performance Optimization

- **Avoid expensive operations** in effects that run frequently
- **Use debouncing/throttling** for effects triggered by rapid changes (scroll, input)
- **Memoize** complex objects or arrays used in dependency arrays with `useMemo`
- **Stabilize functions** used in dependency arrays with `useCallback`
- **Split state** updates to avoid unnecessary effect triggers

```jsx
// Before optimization
function SearchResults({ query }) {
  const [results, setResults] = useState([]);

  // This recreates the options object on every render
  const options = { limit: 10, sort: "desc" };

  // Will run on every render because options is always new
  useEffect(() => {
    fetchResults(query, options).then(setResults);
  }, [query, options]);
}

// After optimization
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  const [limit] = useState(10);
  const [sort] = useState("desc");

  // Memoized options object
  const options = useMemo(
    () => ({
      limit,
      sort,
    }),
    [limit, sort]
  );

  // Will only run when query or memoized options change
  useEffect(() => {
    fetchResults(query, options).then(setResults);
  }, [query, options]);
}
```

## 7. COMPATIBILITY NOTES

### React Version Compatibility

| React Version | Notes                                            |
| ------------- | ------------------------------------------------ |
| < 16.8        | Hooks not available, must use class components   |
| >= 16.8       | Full support for useEffect                       |
| >= 18.0       | Concurrent features may affect timing of effects |

### Environment Considerations

- **Server-side rendering (SSR)**: Effects only run on the client, not during server rendering
- **React Native**: Works the same as in web React
- **Testing**: May need to mock timers or wait for effects with `act()` in test utilities

### Common Integration Points

- **React Router**: Use effects to react to route changes
- **Redux**: Use effects to dispatch async actions based on state/prop changes
- **Context API**: Effects can consume context values or update context through provided methods
- **Animation libraries**: Coordinate animations with component lifecycle through effects

### Related Hooks

| Hook                | Relationship to useEffect                                            |
| ------------------- | -------------------------------------------------------------------- |
| **useState**        | Provides state that effects can read and update                      |
| **useRef**          | Store values that persist between renders without triggering effects |
| **useContext**      | Access context values in effects                                     |
| **useReducer**      | Complex state management that effects can dispatch actions to        |
| **useCallback**     | Stabilize functions used in effect dependency arrays                 |
| **useMemo**         | Stabilize objects and arrays used in effect dependency arrays        |
| **useLayoutEffect** | Similar to useEffect but fires synchronously after DOM mutations     |

## Official Documentation

- [React Docs: Using the Effect Hook](https://react.dev/reference/react/useEffect)
- [React Docs: Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects)
- [React Docs: You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)
- [React Docs: Lifecycle of Reactive Effects](https://react.dev/learn/lifecycle-of-reactive-effects)
