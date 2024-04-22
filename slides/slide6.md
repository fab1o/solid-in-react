# SOLID in React

## Interface Segregation

Implementation details should not matter to high-level components.

### Examples

`PlanOption` component is tightly coupled with the `plan` object.

```jsx
export function PlanOption(props) {
  const { plan } = props;

  const { primaryText } = plan.display;

  return (
    <div>
      <h1>{primaryText}</h1>
    </div>
  );
}
```

**Usage:**

```jsx
<PlanOption plan={plan} />
```

#### The problem:

- Harder to test: A whole `plan` object must be mocked when we shoud not need to.
- If `plan` object changes, we may have to change the component.

### After

`PlanOption` is loosely coupled with `plan`. We give components only what they need.

```jsx
export function PlanOption(props) {
  const { primaryText } = props;

  return (
    <div>
      <h1>{primaryText}</h1>
    </div>
  );
}
```

**Usage:**

```jsx
<PlanOption primaryText={plan.primaryText} />
```

### Benefits

- Easier to test: easier to mock.
- Less things that could break.

---

[prev](slide5.md) | [next](slide7.md)
