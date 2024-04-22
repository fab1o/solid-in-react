# SOLID in React

## Open-closed

Structuring our components in a way that allows them to be extended without changing them.

### Examples

`AuthHeader` is a shared component used on different pages. Depending on the page, `AuthHeader` should render a different UI.

```jsx
import { useRouter } from './hooks/useRouter';

export function AuthHeader() {
  const { pathname, isAuthorized } = useRouter();

  const isEvent = pathname.includes('/events/');
  const isEventEdit = pathname.includes('/events/edit');

  return (
    <header>
      <img src="/logo.svg" alt="Logo" />
      <div>
        {pathname === '/dashboard' && <a href="/events">All event</a>}
        {isEvent && <a href="/events/new">Create event</a>}
        {isEventEdit && isAuthorized && (
          <a href="/events/delete">Delete event</a>
        )}
        {pathname === '/' && <a href="/dashboard">Go to dashboard</a>}
      </div>
    </header>
  );
}
```

**Usage:**

```jsx
<AuthHeader />
```

#### The problem:

- We may have to change `AuthHeader` to accommodate for new pages that are introduced. As a result, regression tests may have to be performed.

### After

```jsx
export function AuthHeader(props) {
  const { children } = props;

  return (
    <header>
      <img src="/logo.svg" alt="Logo" />
      <div>{children}</div>
    </header>
  );
}
```

**Usage:**

```jsx
<AuthHeader>
  <a href="/events">All event</a>
</AuthHeader>
```

### Benefits

- Avoids regression tests, because new functionality can be introduced without changing the component as it is already used elsewhere.

---

[prev](slide3.md) | [next](slide5.md)
