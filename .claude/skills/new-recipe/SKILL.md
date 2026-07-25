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
   - `## Ingredients`: lines are `- quantity | name`, first-pipe split, metric preferred. No pipe for unquantified items ("- flaky salt, to taste"). `###` subgroups for distinct components.
   - `## Method`: one `###` per step, 1–3 sentences each. Say the number — temperatures, times, weights. Reference photos as `![desc](photos/<name>.jpg)` where a process shot would genuinely help (the site shows a quiet placeholder until the photo exists). Cook's cautions as `> note:` blockquotes.
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
   Do NOT merge the PR. An open `new/*` branch means the recipe is on the stove; Andy merges after the first cook (recording it with /new-attempt conventions — the `Cooked:` trailer goes on a commit from the session).

5. Tell Andy the PR URL and what the first cook should pay attention to.
