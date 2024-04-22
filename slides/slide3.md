# SOLID in React

## Single Responsibility

A component, function or class should have only one reason to change.

### Examples

### Case 1: Internal - what it does inside

Consider a `TodosTable` component that fetches data and renders a `<table>`.

```jsx
import { useEffect, useState } from 'react';

export function TodosTable() {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    async function getTodos() {
      const url = 'https://jsonplaceholder.typicode.com/todos/';
      const response = await fetch(url);
      const todos = await response.json();

      setTodos(todos);
    }
    getTodos();
  }, []);

  return (
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Title</th>
        </tr>
      </thead>
      <tbody>
        {todos.map((todo) => (
          <tr key={todo.id}>
            <td>{todo.id}</td>
            <td>{todo.title}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

#### The problem:

- Harder to test: fetch must be mocked.

### After

Now `TodosTable` only renders.

```jsx
export function TodosTable(props) {
  const { todos } = props;

  return (
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Title</th>
        </tr>
      </thead>
      {todos.map((todo) => (
        <tr>
          <td>{todo.id}</td>
          <td>{todo.title}</td>
        </tr>
      ))}
    </table>
  );
}
```

### Case 2: External - how it is used by others

Consider a `FancyButton` component that renders a `<button>`.

```jsx
export function FancyButton(props) {
  const { text, type } = props;

  return (
    <button type={type} class="fancy">
      {text}
    </button>
  );
}
```

Now we want more from `FancyButton`:

```jsx
export function FancyButton(props) {
  const { href, imageSrc, text, type } = props;

  if (href) {
    return (
      <a href={href} class="fancy">
        {text}
      </a>
    );
  }

  if (imageSrc) {
    return (
      <div class="fancy-container">
        <img src={imageSrc} alt="button" />
        <button class="btn">{text}</button>
      </div>
    );
  }

  return (
    <button type={type} class="fancy">
      {text}
    </button>
  );
}
```

#### The problem:

- Increased number of tests we have to write to test the same functionality on different components that use `FancyButton`.

### After

Separate the different types of buttons.

```jsx
export function FancyButton(props) {
  const { text, type } = props;

  return (
    <button type={type} class="fancy">
      {text}
    </button>
  );
}
```

```jsx
export function LinkButton(props) {
  const { href, text } = props;

  return (
    <a href={href} class="fancy">
      {text}
    </a>
  );
}
```

```jsx
export function ImageButton(props) {
  const { imageSrc, text } = props;

  return (
    <div class="fancy-container">
      <img src={imageSrc} alt="button" />
      <button class="fancy">{text}</button>
    </div>
  );
}
```

### Benefits

- Components that are easier to test in isolation.
- Reduces the number of tests that we have to do and things that can break.

---

[prev](slide2.md) | [next](slide4.md)
