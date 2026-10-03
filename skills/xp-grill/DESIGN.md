# Design vocabulary

Design **deep modules**: a lot of behaviour behind a small interface, placed at a clean seam, testable through that interface. Use these terms exactly when agreeing a Story's seams; don't substitute "component," "service," "API," or "boundary."

## Terms

**Module**: anything with an interface and an implementation. Deliberately scale-agnostic: a function, class, package, or tier-spanning slice. _Avoid_: unit, component, service.

**Interface**: everything a caller must know to use the module correctly: the type signature, but also invariants, ordering constraints, error modes, required configuration, and performance characteristics. _Avoid_: API, signature.

**Implementation**: what's inside a module, its body of code.

**Depth**: leverage at the interface. The amount of behaviour a caller (or test) can exercise per unit of interface they have to learn. A module is **deep** when a large amount of behaviour sits behind a small interface, **shallow** when the interface is nearly as complex as the implementation.

**Seam** _(Michael Feathers)_: a place where you can alter behaviour without editing in that place; the location at which a module's interface lives. Where to put the seam is its own design decision, distinct from what goes behind it. _Avoid_: boundary.

**Adapter**: a concrete thing that satisfies an interface at a seam. Describes role (what slot it fills), not substance.

**Leverage**: what callers get from depth: more capability per unit of interface they learn.

**Locality**: what maintainers get from depth: change, bugs, knowledge, and verification concentrate in one place.

## Principles

- **The interface is the test surface.** Callers and tests cross the same seam. If you want to test past the interface, the module is probably the wrong shape.
- **Depth is a property of the interface, not the implementation.** A deep module can be internally composed of small parts; they just aren't part of the interface. Internal seams stay private to the module's own tests.
- **The deletion test.** Imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.
- **One adapter means a hypothetical seam. Two adapters means a real one.** Introduce a seam only when something actually varies across it (typically production + test).

When designing an interface, ask: can I reduce the number of methods? Simplify the parameters? Hide more complexity inside?

## Designing for testability

1. **Accept dependencies, don't create them.** `processOrder(order, paymentGateway)` is testable; a function that constructs its own `StripeGateway` is not.
2. **Return results, don't produce side effects.** `calculateDiscount(cart): Discount` beats `applyDiscount(cart): void`.
3. **Small surface area.** Fewer methods mean fewer tests; fewer params mean simpler setup.

## Dependency categories

Classify a module's dependencies; the category decides how it is tested across its seam.

1. **In-process**: pure computation, in-memory state, no I/O. Test through the interface directly; no adapter needed.
2. **Local-substitutable**: dependencies with local stand-ins (PGLite for Postgres, an in-memory filesystem). Test with the stand-in running in the suite; the seam stays internal.
3. **Remote but owned**: your own services across a network. Define a port at the seam; production uses an HTTP/gRPC/queue adapter, tests use an in-memory adapter.
4. **True external**: third-party services you don't control (Stripe, Twilio). Inject the dependency as a port; tests provide a mock adapter.

## Replace, don't layer

When tests land at a deeper interface, the old unit tests on the shallow modules beneath it become waste: delete them. Tests assert on observable outcomes through the interface and survive internal refactors.
