
# How `Pick` and `Omit` Utility Types Keep TypeScript Code DRY

In large TypeScript projects, developers often create multiple interfaces with very similar properties. Writing these interfaces repeatedly leads to duplicated code, increased maintenance effort, and a higher chance of inconsistencies.

TypeScript solves this problem using utility types like `Pick` and `Omit`.

These utility types help developers create specialized versions of existing interfaces while keeping code **DRY (Don’t Repeat Yourself).**

In this blog, we will learn:

- What `Pick` is
- What `Omit` is
- How they reduce duplication
- Real-world use cases
- Why they improve maintainability

---

# Understanding the DRY Principle

The DRY principle means:

> “Avoid repeating the same code multiple times.”

Repeated code becomes difficult to maintain because every change must be updated in multiple places.

TypeScript utility types help solve this by reusing existing interfaces instead of rewriting them.

---

# What Is `Pick`?

`Pick` creates a new type by selecting specific properties from another type or interface.

## Syntax

```ts
Pick<Type, Keys>
```

- `Type` → Existing interface/type
- `Keys` → Properties to include

---

# Example of `Pick`

## Master Interface

```ts
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}
```

Suppose we only want public user information.

```ts
type PublicUser = Pick<User, "id" | "name" | "email">;
```

## Result

```ts
{
    id: number;
    name: string;
    email: string;
}
```

Instead of rewriting another interface manually, we reuse the original one.

---

# Why `Pick` Is Useful

## 1. Reduces Code Duplication

Without `Pick`:

```ts
interface PublicUser {
    id: number;
    name: string;
    email: string;
}
```

This duplicates fields already defined in `User`.

---

## 2. Easier Maintenance

Suppose the `User` interface changes.

```ts
interface User {
    id: number;
    name: string;
    email: string;
    age: number;
}
```

The derived types automatically stay updated without rewriting code.

---

# What Is `Omit`?

`Omit` creates a new type by removing specific properties from an existing interface.

## Syntax

```ts
Omit<Type, Keys>
```

- `Type` → Existing interface/type
- `Keys` → Properties to remove

---

# Example of `Omit`

## Master Interface

```ts
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}
```

Suppose we want everything except the password.

```ts
type SafeUser = Omit<User, "password">;
```

## Result

```ts
{
    id: number;
    name: string;
    email: string;
}
```

This is extremely useful for secure API responses.

---

# Real-World Use Cases

# 1. API Responses

Sensitive data should not be exposed publicly.

```ts
type UserResponse = Omit<User, "password">;
```

---

# 2. Login Forms

Only required fields should be included.

```ts
type LoginForm = Pick<User, "email" | "password">;
```

---

# 3. Admin Dashboards

Different views may require different data slices.

```ts
type AdminView = Pick<User, "id" | "name" | "email">;
```

---

# 4. Update Requests

Sometimes IDs should not be editable.

```ts
type UpdateUser = Omit<User, "id">;
```

---

# Combining `Pick` and `Omit`

These utility types can also work together.

## Example

```ts
type EditableUser = Omit<
    Pick<User, "id" | "name" | "email">,
    "id"
>;
```

This creates a type containing only editable fields.

---

# Benefits of Using `Pick` and `Omit`

## Cleaner Code

Less repeated interface creation.

---

## Better Consistency

All derived types stay connected to the original interface.

---

## Easier Refactoring

Changes in the master interface automatically affect derived types.

---

## Improved Scalability

Large applications become easier to manage.

---

# `Pick` vs `Omit`

| Utility Type | Purpose |
|---|---|
| `Pick` | Select specific properties |
| `Omit` | Remove specific properties |

---

# Best Practices

## Use a Master Interface

Create one main interface and generate smaller slices from it.

Example:

```ts
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}
```

Then derive specialized types as needed.

---

## Avoid Duplicate Interfaces

Bad:

```ts
interface LoginUser {
    email: string;
    password: string;
}
```

Better:

```ts
type LoginUser = Pick<User, "email" | "password">;
```

---

# Conclusion

`Pick` and `Omit` are powerful TypeScript utility types that help developers write cleaner, reusable, and maintainable code.

They reduce duplication by allowing developers to create specialized “slices” of existing interfaces instead of rewriting them manually.

