# xp-skills

Agent skills for one loop: **grill an idea into a Story, implement it, commit**, all in a single context window. Inspired by Extreme Programming, built on [Matt Pocock's skills](https://github.com/mattpocock/skills).

## Why

Specs grow big and freeze premises that a faster loop would have caught. Here there is no spec: if a change doesn't fit in one grill and one implement, it is several Stories, and the grill cuts it down. The rest waits as one-line Cards.

## Install

```sh
npx skills add gbaims/xp-skills
```

## Use

- **`/xp`**: the whole loop. Grills the idea into a Story, implements it on your acceptance, commits, then tells you to `/clear` for the next one.
- **`/xp-grill`**: just the grill, when you want to shape a Story without building it yet.
- **`/xp-implement`**: just the build, from a Story accepted earlier in the same conversation.

## Files the skills keep in your repo

- **`GLOSSARY.md`** and **`docs/adr/`**: the domain language and hard-to-reverse decisions, sharpened during the grill.
- **`CARDS.md`**: one line per Story not yet grilled. A Card leaves the file in the commit that builds it.
- **Commit messages**: each Story, written out in full, lives in the body of the commit that built it.

See [GLOSSARY.md](GLOSSARY.md) for the terms and [CREDITS.md](CREDITS.md) for what came from where.
