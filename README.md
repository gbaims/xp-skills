# xp-skills

Agent skills for one loop: **grill an idea into a Story, implement it, open its pull request**, all in a single context window. Inspired by Extreme Programming, built on [Matt Pocock's skills](https://github.com/mattpocock/skills).

## Why

Specs grow big and freeze premises that a faster loop would have caught. Here there is no spec: if a change doesn't fit in one grill and one implement, it is several Stories, and the grill cuts it down. The rest waits as one-line Cards.

## Install

```sh
npx skills add gbaims/xp-skills
```

## Use

- **`/xp`**: the whole loop. Grills the idea into a Story on a fresh branch, implements it on your acceptance, opens a pull request, then tells you to `/clear` for the next one. Merging is yours.
- **`/xp-grill`**: just the grill, when you want to shape a Story without building it yet.
- **`/xp-implement`**: just the build, from a Story accepted earlier in the same conversation.

## Requirements

A GitHub repo and an authenticated [`gh`](https://cli.github.com/) CLI. Set the repo to squash-merge with the pull request title and body as the commit message; `/xp-grill` warns when it isn't and shows the command.

## What the skills keep

- **`GLOSSARY.md`** and **`docs/adr/`**: the domain language and hard-to-reverse decisions, sharpened during the grill.
- **GitHub issues**: one Card per open issue without a linked pull request, a one-line note of a Story not yet grilled. The pull request that builds a Card closes its issue.
- **Pull requests**: each Story, written out in full, opens its pull request's body, followed by the review report. Squash-merged, it becomes the body of the Story's single commit on the default branch.

See [GLOSSARY.md](GLOSSARY.md) for the terms and [CREDITS.md](CREDITS.md) for what came from where.
