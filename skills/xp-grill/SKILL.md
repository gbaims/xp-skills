---
name: xp-grill
description: Grill the user into one accepted Story, sharpening the domain glossary and ADRs along the way. Use only when the user or the xp skill explicitly asks for it.
---

# XP Grill

Interview the user relentlessly until you share one **Story**: a change small enough to go from idea to commit in this context window. The grill ends with the Story written out in full and accepted by the user. `xp-implement` builds from that text, and its reviewer sees nothing else, so the Story carries every decision.

## 1. Start from a Card

If `CARDS.md` exists at the repo root, show its Cards and ask whether this Story starts from one of them. A **Card** is a one-line note of a Story not yet grilled. Remember which Card was picked: `xp-implement` removes it at commit.

Read `GLOSSARY.md` (or `GLOSSARY-MAP.md`) and the ADRs in `docs/adr/` touching the area before the first round.

## 2. Grill in rounds

Map the idea as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body>

➡️ <your recommended answer>
```

Each round reshapes the tree: settled decisions push the frontier outward. Recompute it and ask the next round. A question whose answer depends on another question still open in this round belongs to a later round.

Finding _facts_ is your job, never the user's. When a question needs a fact from the environment, dispatch a sub-agent to find it, and ask the rest of the frontier while it runs. The _decisions_ are the user's: put each to them and wait.

### Cut to size

Every round, test the idea against the size of a Story:

- **Seams**: a Story is tested at one seam, two at most. More seams means more than one Story.
- **Converging frontier**: when, after about three rounds, each round opens more questions than it settles, premises are piling up. Cut.

To cut, propose the smallest first Story that delivers something observable, and grill only that. Everything cut becomes a Card: append one line per Card to `CARDS.md` (create it if missing). A Card is one line with no acceptance criteria and no decisions; detail is born only when a Card becomes a Story.

### Sharpen the language

The glossary and ADRs change only here, in the grill, as decisions crystallise.

- **Challenge against the glossary.** When the user uses a term that conflicts with `GLOSSARY.md`, call it out: "Your glossary defines 'cancellation' as X, but you seem to mean Y. Which is it?"
- **Sharpen fuzzy language.** When a term is vague or overloaded, propose a precise canonical one: "You're saying 'account': do you mean the Customer or the User?"
- **Probe with scenarios.** Invent concrete edge-case scenarios that force the user to be precise about the boundaries between concepts.
- **Cross-reference with code.** When the user states how something works, check whether the code agrees, and surface contradictions.
- **Write the glossary inline.** When a term is resolved, update `GLOSSARY.md` right then, in the format in [GLOSSARY-FORMAT.md](GLOSSARY-FORMAT.md). It holds domain terms only: no implementation details, no Story content.
- **Offer ADRs sparingly**, only when a decision meets all three criteria in [ADR-FORMAT.md](ADR-FORMAT.md).

### Agree the seams

Before the frontier can close, settle with the user which seams the Story is tested at and what each one observes. Prefer existing seams, and the highest one possible. When the shape of an interface or the placement of a seam is in question, read [DESIGN.md](DESIGN.md) for the vocabulary.

## 3. Write the Story

When the frontier is empty (every branch visited, nothing silently assumed, seams agreed), write the Story in this format:

```md
## Story: <short title>

**Intent**: why this exists, from the perspective of whoever uses it (2–4 lines).

**Behaviours**: a numbered list of observable behaviours. This is the reviewer's acceptance bar: each item must be checkable without reading this conversation.

**Seams**: where the tests live and what each seam observes.

**Decisions**: what was decided in the grill and why, including rejected alternatives and the premises the Story rests on ("assumes X already exists"). A reviewer must not "fix" a deliberate choice; an implementer must be able to tell when a premise has fallen.

**Out of scope**: what was cut, and the Cards created from it.
```

Use the glossary's terms. Leave out file paths and code snippets; they go stale.

The grill is done when the user accepts the Story text. If they change anything, rewrite it and ask again.

## Re-entry

When `xp-implement` reports a fallen premise, grill only the decisions that depend on it, then rewrite the Story and get it accepted again before implementation resumes.
