# Test-driven development

TDD is the red → green loop. This reference makes the loop produce tests worth keeping. Every section applies on every cycle.

Name tests and interfaces in the glossary's vocabulary, and respect the ADRs in the area you're touching.

## Seams

Tests live at the seams the Story names, never against internals. The seams were agreed in the grill; a test that needs a seam the Story doesn't name is a fallen premise, not a judgement call.

## What a good test is

Tests verify behaviour through public interfaces, not implementation details. Code can change entirely; tests shouldn't. A good test reads like a specification: "user can checkout with valid cart" tells you exactly what capability exists, and survives refactors because it doesn't care about internal structure.

```typescript
// GOOD: tests observable behaviour
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});

// GOOD: verifies through the interface
test("createUser makes user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```

A good test uses the public interface only, describes WHAT rather than HOW, and makes one logical assertion.

## Anti-patterns

- **Implementation-coupled**: mocks internal collaborators, tests private methods, asserts on call counts, or verifies through a side channel (querying the database instead of using the interface). The tell: the test breaks on a refactor that didn't change behaviour.
- **Tautological**: the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`), so it passes by construction. Expected values come from an independent source of truth: a known-good literal, a worked example, the Story.
- **Horizontal slicing**: writing all tests first, then all implementation. Bulk tests verify imagined behaviour. Work in **vertical slices**: one test → one implementation → repeat, each test a **tracer bullet** that responds to what the last cycle taught you.

## Mocking

Mock at **system boundaries** only: external APIs, time, randomness, and sometimes databases or the filesystem (prefer a test database or in-memory stand-in). Your own modules and internal collaborators run for real.

At a boundary, design for mockability:

- **Inject the dependency**: `processPayment(order, paymentClient)` rather than constructing the client inside.
- **Prefer SDK-style interfaces**: one function per external operation (`getUser`, `createOrder`) rather than one generic `fetch(endpoint, options)`, so each mock returns one shape and needs no conditional logic.

## Rules of the loop

- **Red before green.** Write the failing test first, then only enough code to pass it. Add no speculative features.
- **One slice at a time.** One seam, one test, one minimal implementation per cycle.
- **Refactoring belongs to review**, not to the red → green cycle.
