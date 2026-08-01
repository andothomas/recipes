---
name: new-recipe
description: Write a new recipe and open its new/<slug> PR. Use when Andy wants to add a recipe to the book, has a dish idea, or provides a recipe .md authored elsewhere.
---

# New recipe

You are adding a recipe to a git recipe book (see the repo README for the full conventions). The premise: recipes are written by an AI, tested by a human. Your job is to write a considered first draft and land it through the real flow.

## Steps

1. **Get the dish.** If Andy provided a finished `.md` file, skip to step 3 and validate it. Otherwise, interview briefly: the dish, constraints (equipment, time, servings), cuisine leanings, anything they already know they want. Two or three questions, not a questionnaire.

2. **Write the recipe** at `recipes/<slug>/recipe.md`. Slug is short kebab-case (`short-ribs`, not `gochujang-braised-beef-short-ribs`). Follow the README grammar exactly:
   - Frontmatter: `title` (sentence case), `lede` (≤40 words, confident, specific), `eyebrow` (2–3 ALL-CAPS words joined by " / "), `tags` (1–3, lowercase), `serves` (quoted string), `total_time` (quoted string like "3h 45m"), `hero: photos/hero.jpg`.
   - Nutrition frontmatter, per serving and honestly approximate — compute it from the ingredient list, don't guess round numbers: `satiety` (1–5: 1 a snack, 3 a light meal, 5 won't need seconds), `serving` ("1 bowl (about 20 oz)" — plain words plus a US-customary weight), `macros: { calories: N, protein: N, carbs: N, fat: N }` (calories kcal, the rest grams — the macros exception). Unquantified "to taste" items are excluded; that's part of why it says approximate.
   - `## Ingredients`: lines are `- quantity | name`, first-pipe split. US customary units throughout — weights in lb (roughly a pound and up) or oz (below that), volumes in cups/tbsp/tsp; never grams or ml. Exception: macros and nutrition talk (protein, carbs, fat) stay in grams. No pipe for unquantified items ("- flaky salt, to taste"). `###` subgroups for distinct components.
   - `## Method`: one `###` per step, 1–3 sentences each. Say the number — temperatures, times, weights. Reference photos as `![desc](photos/<name>.jpg)` where a process shot would genuinely help (the site shows a quiet placeholder until the photo exists). Do NOT draft `> note:` blockquotes — cautions that matter belong in the step prose. Notes are reserved for Andy's own margin notes from real cooks, added afterward in his words.
   - Voice: confident, specific, faintly dry. Sentence case. No emoji, no exclamation marks, no "delicious"/"amazing"/"elevate". Instructions address the cook ("Pat the ribs dry").
   - Adding a recipe records its first cook — the add commit counts as attempt one. If the cook happened on a different day than the commit, add a `Cooked: YYYY-MM-DD` trailer. Do not invent details of the cook beyond what Andy reports.

3. **Validate.** Check every rule above mechanically. If the file came from elsewhere, fix format violations only — never editorial content.

4. **Land it:**
   ```
   git checkout main && git pull
   git checkout -b new/<slug>
   git add recipes/<slug>/
   git commit -m "Add <title> — first draft"
   git push -u origin new/<slug>
   gh pr create --title "Add <title>" --body "<one line on the dish and what the first cook should answer>"
   ```
   Do NOT merge on your own. An open `new/*` branch means the recipe is on the stove — the site gives it a full preview page, a clickable card, and the gold tip of the landing tree. The first cook happens while the branch is open: photos and fixes land as branch commits, and a commit from the session carries the `Cooked:` trailer when the cook date differs from the add commit.

   When Andy calls it, merge with a true merge commit (never squash/rebase). The merge body's first line becomes the event label on the site's live tree, so it must say what happened:
   ```
   gh pr merge <N> --merge --subject "Merge pull request #<N> from andothomas/new/<slug>" --body "Add <title>"
   ```

5. Tell Andy the PR URL and what the first cook should pay attention to.

## Photos

Andy's photos arrive as HEICs off his phone. Process before committing — the repo is public:

- Convert to JPEG, `-auto-orient` first (phone EXIF rotation lies to naive crops).
- Crop in on the food: square for bowls (they're round), wider for pans and spreads. Trim rooms, counters, and clutter; a hint of hand is fine.
- Saturation +15% (`-modulate 100,115,100`) — the book's look, applied consistently.
- Resize to 1600–2000px on the long edge, quality ~85, and `-strip` all metadata (phone photos carry GPS).
- Filenames must match what recipe.md references (`hero.jpg` and the step shots).

```
magick IMG_XXXX.HEIC -auto-orient -crop <geometry> +repage -modulate 100,115,100 -resize 2000x -strip -quality 85 recipes/<slug>/photos/hero.jpg
```

Commit as "Process hero photo from the first cook" — the `Cooked:` trailer rides this commit when it's the one recording the session.

If a photo isn't Andy's (a borrowed hero because the cook went unphotographed): no saturation grade — the look is for our photos only — just resize and strip metadata, set `hero_credit: "not our photo — via <source>"` in the frontmatter, and say so in the commit message. Replace it with a real photo at the next cook.
