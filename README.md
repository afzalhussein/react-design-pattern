# react-design-pattern
A collection of examples 
Design patterns are reusable solutions to common problems in software design. In React, there are several design patterns that can help you create more maintainable and scalable applications. Here are some commonly used design patterns in React, along with examples:

### 1. **Container-Presenter Pattern**

This pattern separates the logic of your application from the UI rendering. The container component handles the logic (state, data fetching, etc.), and the presenter component is responsible for rendering the UI.

**Example:**

```javascript
// Container Component
import React, { useState, useEffect } from 'react';
import TodoList from './TodoList';

const TodoContainer = () => {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    // Fetch data from an API
    fetch('/api/todos')
      .then(response => response.json())
      .then(data => setTodos(data));
  }, []);

  return <TodoList todos={todos} />;
};

export default TodoContainer;

// Presenter Component
import React from 'react';

const TodoList = ({ todos }) => (
  <ul>
    {todos.map(todo => (
      <li key={todo.id}>{todo.text}</li>
    ))}
  </ul>
);

export default TodoList;
```

### 2. **Higher-Order Components (HOC)**

A Higher-Order Component is a function that takes a component and returns a new component with additional props or functionality. It is used for code reuse, logic, and bootstrap abstraction.

**Example:**

```javascript
import React, { Component } from 'react';

// Higher-Order Component
const withLoading = WrappedComponent => {
  return class extends Component {
    state = { loading: true };

    componentDidMount() {
      setTimeout(() => {
        this.setState({ loading: false });
      }, 2000); // Simulating a delay
    }

    render() {
      return this.state.loading ? <p>Loading...</p> : <WrappedComponent {...this.props} />;
    }
  };
};

// Wrapped Component
const MyComponent = () => <div>Content loaded!</div>;

// Using the HOC
const MyComponentWithLoading = withLoading(MyComponent);

export default MyComponentWithLoading;
```

### 3. **Render Props**

Render Props is a pattern for sharing code between components using a prop whose value is a function.

**Example:**

```javascript
import React from 'react';

class MouseTracker extends React.Component {
  state = { x: 0, y: 0 };

  handleMouseMove = event => {
    this.setState({
      x: event.clientX,
      y: event.clientY,
    });
  };

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>
        {this.props.render(this.state)}
      </div>
    );
  }
}

const App = () => (
  <MouseTracker
    render={({ x, y }) => (
      <h1>
        Mouse position: ({x}, {y})
      </h1>
    )}
  />
);

export default App;
```

### 4. **Compound Components**

Compound components allow multiple components to work together to manage state.

**Example:**

```javascript
import React, { useState } from 'react';

const Tabs = ({ children }) => {
  const [activeIndex, setActiveIndex] = useState(0);

  return React.Children.map(children, (child, index) =>
    React.cloneElement(child, {
      isActive: index === activeIndex,
      onClick: () => setActiveIndex(index),
    })
  );
};

const Tab = ({ isActive, onClick, children }) => (
  <div
    onClick={onClick}
    style={{
      cursor: 'pointer',
      fontWeight: isActive ? 'bold' : 'normal',
    }}
  >
    {children}
  </div>
);

const App = () => (
  <Tabs>
    <Tab>Tab 1</Tab>
    <Tab>Tab 2</Tab>
    <Tab>Tab 3</Tab>
  </Tabs>
);

export default App;
```

### 5. **Context API**

The Context API is used for global state management, avoiding prop drilling by passing data through the component tree without having to pass props down manually at every level.

**Example:**

```javascript
import React, { createContext, useContext, useState } from 'react';

// Create Context
const ThemeContext = createContext();

const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

const ThemedComponent = () => {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <div>
      <p>Current theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
};

const App = () => (
  <ThemeProvider>
    <ThemedComponent />
  </ThemeProvider>
);

export default App;
```

These design patterns help you to write more organized, maintainable, and scalable React applications. Each pattern has its own use case, and choosing the right one depends on the specific needs of your application.
