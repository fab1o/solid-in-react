# SOLID in React

## Liskov Substitution

Any higher-level component can be replaced with its base component.

### Examples

For illustrate this one, we have to use TypeScript. Consider a `FancyInput` that renders an `<input>`.

```tsx
type InputType = {
  type: string;
  className?: string;
  placeholder?: string;
  value?: string;
  autocomplete?: string;
};

export default function FancyInput(props: InputType): JSX.Element {
  const { className = '', ...rest } = props;

  return <input className={`class-input ${className} `} {...rest} />;
}
```

**Usage:**

```jsx
<FancyInput placeholder="placeholder" autocomplete="off" type="password" />
```

#### The problem:

- The property name is different than its base `<input>`, so we can't replace `<FancyInput>` for its base component.
- In this case, "auto complete" functionality doesn't work because there is a typo.

### After

```tsx
import { type InputHTMLAttributes } from 'react';

export default function FancyInput(props: InputHTMLAttributes): JSX.Element {
  const { className = '', ...rest } = props;

  return <input className={`class-input ${className}`} {...rest} />;
}
```

**Usage:**

```jsx
<FancyInput placeholder="placeholder" autoComplete="off" type="password" />
```

### Benefits

- Keep components robust and cohesive.

---

[prev](slide4.md) | [next](slide6.md)
