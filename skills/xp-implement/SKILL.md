---
name: xp-implement
description: Build an accepted Story test-first, review it, and open its pull request. Use only when the user or the xp skill explicitly asks for it.
---

# XP Implement

Build the **Story** accepted in this conversation, from the first red test to the open pull request. The Story is the whole brief: its Behaviours are the acceptance bar, its Seams are where tests live, its Decisions are settled.

## 1. Gate

Find the Story the user accepted at the end of `xp-grill` in this conversation. If there is none, stop and tell the user to run `/xp` or `/xp-grill` first.

Resolve the default branch (`gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`). If `git branch --show-current` is the default branch, stop: the Story belongs on the branch `xp-grill` cuts. Record the branch base with `git merge-base HEAD origin/<default>`; the review diffs against it.

## 2. Build

Drive each Behaviour test-first at the Story's seams, one vertical slice at a time, following [TDD.md](TDD.md). Run typechecking and single test files regularly, and the full test suite once at the end. Commit each green slice with a one-line message. The step is done when every Behaviour has a passing test and the full suite is green.

### When a premise falls

When the code contradicts a Decision or premise in the Story (a seam that doesn't exist, an interface that can't take the shape agreed, a Behaviour that conflicts with existing code), stop building. Tell the user which Decision fell and what you found, then call the Skill tool with `xp-grill` to re-grill the dependent decisions and rewrite the Story. Resume from the rewritten Story once the user accepts it.

## 3. Review

Run the two-axis review in [REVIEW.md](REVIEW.md), passing the Story verbatim to the Spec axis. Then act on every finding:

- **Spec gap** (a Behaviour missing, partial, or wrong): fix it test-first.
- **Scope creep** (behaviour the Story didn't ask for): remove it.
- **Documented-standard violation**: fix it.
- **Smell in code the diff added**: refactor it away with the suite green. It is this Story's mess.
- **Smell in code that predates the diff**: hold it for triage.
- **Finding that contradicts a Decision**: leave the code as is and record which Decision it conflicts with. The Story outranks the reviewer.

Commit the fixes, if any. The step is done when every finding is fixed, removed, held, or recorded, and the full suite is green.

## 4. Triage

Put every held smell to the user as one round in the grill's format: a numbered ❓ per smell naming it and quoting the code, and a ➡️ recommending **fix** or **ignore** with a one-line reason. Wait for the answers, refactor the ones marked fix with the suite green, and commit. Skip this step when nothing was held.

## 5. Open the pull request

Push the branch (`git push -u origin HEAD`) and open a ready pull request with `gh pr create`:

- **Title**: the Story title.
- **Body**: the accepted Story verbatim, then `Closes #N` when it started from a Card, then a `## Report` listing the fixes made in review, the refactors done outside the Story in triage, the smells ignored and why, and the findings set aside because of a Decision.

The repo squash-merges with this title and body, so the body becomes the Story's commit message on the default branch.

Finish with the pull request's URL. Commits made later in the session go to the same branch; push them to update the pull request.
