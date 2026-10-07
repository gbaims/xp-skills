# Review

A two-axis review of the Story's diff:

- **Standards**: does the code conform to this repo's documented coding standards?
- **Spec**: does the code faithfully implement the Story?

Both axes run as **parallel sub-agents** so they don't pollute each other's context. Neither saw the grill, so each gets everything it needs in its prompt.

## 1. Capture the diff

Stage everything (`git add -A`) and capture the diff against the branch base: `git diff --cached <base>`. Confirm it is non-empty before spawning anything.

## 2. Find the standards sources

Anything in the repo that documents how code should be written, such as `CODING_STANDARDS.md` or `CONTRIBUTING.md`.

On top of those, the Standards axis always carries the **smell baseline** below, a fixed set of Fowler code smells (_Refactoring_, ch.3). Two rules bind it:

- **The repo overrides.** A documented repo standard always wins; where it endorses something the baseline would flag, suppress the smell.
- **Always a judgement call.** Each smell is a labelled heuristic ("possible Feature Envy"), never a hard violation. Skip anything tooling already enforces.

Each smell reads _what it is_ → _how to fix_:

- **Mysterious Name**: a function, variable, or type whose name doesn't reveal what it does or holds. → rename it; if no honest name comes, the design's murky.
- **Duplicated Code**: the same logic shape appears in more than one hunk or file. → extract the shared shape, call it from both.
- **Feature Envy**: a method that reaches into another object's data more than its own. → move the method onto the data it envies.
- **Data Clumps**: the same few fields or params keep travelling together. → bundle them into one type, pass that.
- **Primitive Obsession**: a primitive or string standing in for a domain concept. → give the concept its own small type.
- **Repeated Switches**: the same `switch`/`if`-cascade on the same type recurs. → replace with polymorphism, or one map both sites share.
- **Shotgun Surgery**: one logical change forces scattered edits across many files. → gather what changes together into one module.
- **Divergent Change**: one module is edited for several unrelated reasons. → split so each module changes for one reason.
- **Speculative Generality**: abstraction, parameters, or hooks the Story doesn't need. → delete it; inline back until a real need shows.
- **Message Chains**: long `a.b().c().d()` navigation the caller shouldn't depend on. → hide the walk behind one method on the first object.
- **Middle Man**: a class or function that mostly delegates onward. → cut it, call the real target direct.
- **Refused Bequest**: a subclass that ignores or overrides most of what it inherits. → drop the inheritance, use composition.

## 3. Spawn both sub-agents in parallel

**Standards sub-agent prompt**:

- The diff command.
- The standards-source files from step 2, plus the smell baseline pasted in full.
- The brief: "Report, per file/hunk where relevant, (a) every place the diff violates a documented standard: cite the standard (file + rule); and (b) any baseline smell you spot: name it and quote the hunk. Documented-standard breaches can be hard violations; baseline smells are always judgement calls; a documented repo standard overrides the baseline. Skip anything tooling enforces. Under 400 words."

**Spec sub-agent prompt**:

- The diff command.
- The accepted Story, pasted verbatim.
- The brief: "Report: (a) Behaviours the Story asked for that are missing or partial; (b) behaviour in the diff the Story didn't ask for (scope creep); (c) Behaviours that look implemented but wrong. Quote the Story line for each finding. The Story's Decisions are deliberate: flag a conflict with one, never recommend overriding it. Under 400 words."

## 4. Aggregate

Keep the two reports under `## Standards` and `## Spec`, unmerged and unranked across axes: a change can pass one and fail the other, and reporting them separately stops one axis from masking the other.
