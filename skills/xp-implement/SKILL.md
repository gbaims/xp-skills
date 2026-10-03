---
name: xp-implement
description: Build an accepted Story test-first, review it, and commit it. Use only when the user or the xp skill explicitly asks for it.
---

# XP Implement

Build the **Story** accepted in this conversation, from the first red test to the commit. The Story is the whole brief: its Behaviours are the acceptance bar, its Seams are where tests live, its Decisions are settled.

## 1. Gate

Find the Story the user accepted at the end of `xp-grill` in this conversation. If there is none, stop and tell the user to run `/xp` or `/xp-grill` first.

Record the start commit with `git rev-parse HEAD`; the review diffs against it.

## 2. Build

Drive each Behaviour test-first at the Story's seams, one vertical slice at a time, following [TDD.md](TDD.md). Run typechecking and single test files regularly, and the full test suite once at the end. The step is done when every Behaviour has a passing test and the full suite is green.

### When a premise falls

When the code contradicts a Decision or premise in the Story (a seam that doesn't exist, an interface that can't take the shape agreed, a Behaviour that conflicts with existing code), stop building. Tell the user which Decision fell and what you found, then call the Skill tool with `xp-grill` to re-grill the dependent decisions and rewrite the Story. Resume from the rewritten Story once the user accepts it.

## 3. Review

Run the two-axis review in [REVIEW.md](REVIEW.md), passing the Story verbatim to the Spec axis. Then act on every finding:

- **Spec gap** (a Behaviour missing, partial, or wrong): fix it test-first.
- **Scope creep** (behaviour the Story didn't ask for): remove it.
- **Documented-standard violation**: fix it.
- **Smell**: leave the code as is and list it in the final report for the user to decide.
- **Finding that contradicts a Decision**: leave the code as is and explain in the final report which Decision it conflicts with. The Story outranks the reviewer.

The step is done when every finding is fixed, removed, or listed, and the full suite is green.

## 4. Commit

Commit to the current branch:

- **Subject**: the Story title.
- **Body**: the accepted Story, verbatim.

If the Story started from a Card, remove that line from `CARDS.md` in the same commit (delete the file if it ends up empty).

Finish with a short report: the commit, the smells left for the user, and any findings set aside because of a Decision.
