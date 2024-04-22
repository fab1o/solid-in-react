# SOLID in React

## Dependency Inversion

High-level components should not depend on low-level concrete components, only on abstractions.

### Examples

```ts
export const cookieManager = {
  save(data) {
    document.cookie = data;
  },
  get() {
    return document.cookie;
  },
};
```

```ts
import { cookieManager } from './cookieManager';

export const profileManager = {
  saveUser(user) {
    // Business logic for saving a user
    cookieManager.save(user);
  },

  getUser() {
    // Business logic for getting a user
    return cookieManager.get();
  },
};
```

**Usage:**

```ts
profileManager.getUser();
```

#### The problem:

- `profileManager` is tighly coupled with `cookieManager` and it saves cookie in `document` thus it only works in the browser.

### After

We use dependency injection.

```ts
export interface CookieManager {
  save(data): void;
  get(): any;
}
```

```ts
import type { CookieManager } from './cookieManager';

export class ProfileManager {
  private cookieManager: CookieManager;

  constructor(cookieManager) {
    this.cookieManager = cookieManager;
  }

  public saveUser(user) {
    // Business logic for saving a user
    this.cookieManager.save(user);
  }

  public getUser() {
    // Business logic for getting a user
    return this.cookieManager.get();
  }
}
```

**Usage:**

```ts
const myCookieManager = new CookieManager();
const profileManager = new ProfileManager(myCookieManager);

profileManager.getUser();
```

### Benefits

- Again, better testing: mock implementation of a dependency instead of using a "production" implementation.
- This is key to testing in isolation.

But some words of caution....

---

[prev](lide6/slide6.md) | [next](slide8.md)
