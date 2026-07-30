# recipes

A recipe book kept in git. The recipes are written by an AI and tested by a human, and every correction is kept — including the failures.

This repo is rendered read-only at [recipes.andythomas.me](https://recipes.andythomas.me). The site rebuilds on every push; nothing is authored there.

## How the book works

- **main is the book.** `recipes/<slug>/recipe.md` on main is the current version of a recipe.
- **A branch is a cooking session.** `new/<slug>` births a recipe; `attempt/<slug>/<descriptor>` is a repeat attempt at one (e.g. `attempt/short-ribs/crispier-skin`). An open branch means dinner is in progress.
- **Every branch merges.** Attempts land on main via PR with a true merge commit — even failed ones. A failed attempt merges its journal and verdict without touching the recipe. Squash and rebase merges are disabled; the offshoot shape is the record.
- **Every change states its reason,** commit-message style: "Cut sugar to 1 tbsp — sauce was cloying".
- Branches outside `new/` and `attempt/` are ignored by the site.

## Layout

```
recipes/<slug>/recipe.md                       the current version
recipes/<slug>/attempts/<NN>-<descriptor>.md   one journal per attempt
recipes/<slug>/photos/                         process photos, referenced from the markdown
```

## recipe.md format

```markdown
---
title: Gochujang short ribs
lede: One sentence, at most 40 words.
eyebrow: SUNDAY / BRAISE
tags: [sunday, braise]
serves: "4"
total_time: "4h 20m"
hero: photos/hero.jpg
satiety: 4
serving: "1 plate (about 14 oz)"
macros: { calories: 640, protein: 45, carbs: 38, fat: 33 }
---
## Ingredients
### For the braise
- 4 lb | bone-in beef short ribs
- flaky salt, to taste

## Method
### Salt early
One to three sentences. Say the number: 300°F, 90 seconds a side.

![the sear](photos/sear.jpg)

> note: any blockquote inside a step is a cook's note.
```

Rules the site's parser enforces:

- **Ingredients** are `- quantity | name`, split on the first pipe. The quantity is what 1×/2× scaling multiplies; a line with no pipe ("- flaky salt, to taste") never scales. `###` subheadings inside Ingredients group components.
- **Borrowed photos** are credited, not passed off: when the hero isn't ours, `hero_credit: "not our photo — via <source>"` renders over the image. Replace borrowed photos with our own at the next cook.
- **Nutrition facts** are optional frontmatter, always approximate and always per serving: `satiety` is 1–5 (1 a snack, 5 won't need seconds), `serving` is plain words with a US-customary weight, `macros` is `{ calories, protein, carbs, fat }` — calories in kcal, the rest in grams (the macros exception to the units rule). Estimate honestly from the ingredient list; revise when a cook proves otherwise.
- **Steps** are one `###` heading each under `## Method`. Images are standard markdown images pointing into `photos/`. Blockquotes are notes.
- A recipe that fails to parse is dropped from the site with a warning, never a broken deploy. The history keeps the mistake either way.

## Attempt journals

Each attempt PR adds `attempts/<NN>-<descriptor>.md` and edits `recipe.md` as the cook informed:

```markdown
---
date: 2026-07-24
branch: attempt/short-ribs/crispier-skin
summary: 12h dry brine, hotter finish
verdict: Merged — the skin finally shattered. Brine stays.
---
Notes from the session, with photos as ordinary markdown images.
```

## Marking a cook

A commit that corresponds to an actual cooking session carries a git trailer:

```
Open attempt/short-ribs/crispier-skin — dry brine 12h

Cooked: 2026-07-24
```

The site counts attempts by distinct `Cooked:` dates and shows the most recent as "last cooked". A recipe enters the book cooked once: its add commit counts as attempt one even without a trailer. If the first cook happened on a different day than the commit, put a `Cooked:` trailer on the add commit.

## Versions

When a recipe feels canonical, tag the merge commit `<slug>/v1`, `<slug>/v2`, … Tags are namespaced per recipe; bare `v1` is never used.

## Authoring

Two Claude skills drive the whole flow so the format never drifts:

- `/new-recipe` — interview about the dish, write `recipe.md`, open the `new/<slug>` PR.
- `/new-attempt` — open the branch, scaffold the journal, record the cook, open the PR.
