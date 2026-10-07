---
name: xp
description: "Run one Story end to end: grill it, implement it on acceptance, open its pull request."
disable-model-invocation: true
---

# XP

One Story, one context window: grill, implement, pull request.

1. Call the Skill tool with `xp-grill`. This step is done when the user accepts the written Story.
2. Call the Skill tool with `xp-implement`. This step is done when the Story's pull request is open.
3. Show the pull request's URL and the open Cards (the same query `xp-grill` offers them from), and tell the user to `/clear` and run `/xp` for the next Story. The session ends here: the next Story starts in a fresh context. Merging the pull request is the user's.
