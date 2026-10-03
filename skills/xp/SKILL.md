---
name: xp
description: "Run one Story end to end: grill it, implement it on acceptance, commit."
disable-model-invocation: true
---

# XP

One Story, one context window: grill, implement, commit.

1. Call the Skill tool with `xp-grill`. This step is done when the user accepts the written Story.
2. Call the Skill tool with `xp-implement`. This step is done when the Story is committed.
3. Show the Cards left in `CARDS.md`, if any, and tell the user to `/clear` and run `/xp` for the next Story. The session ends here: the next Story starts in a fresh context.
