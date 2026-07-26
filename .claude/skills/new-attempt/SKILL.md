---
name: new-attempt
description: Open a repeat attempt at an existing recipe, journal the cook, and open the PR. Use when Andy is cooking (or about to cook) something from the book again, or wants to try a variation.
---

# New attempt

An attempt is one cooking session of an existing recipe, kept forever as an offshoot in history. Every attempt merges — failed ones too. See the repo README for conventions.

## Steps

1. **Identify recipe + descriptor.** The recipe is a folder under `recipes/`. Ask what tonight's question is ("does a 12h dry brine beat the current version?") and derive a short kebab-case descriptor (`crispier-skin`, `less-sugar`).

2. **Open the branch and scaffold the journal:**
   ```
   git checkout main && git pull
   git checkout -b attempt/<slug>/<descriptor>
   ```
   Create `recipes/<slug>/attempts/<NN>-<descriptor>.md` where `<NN>` is the next zero-padded number in that folder:
   ```markdown
   ---
   date: <today, YYYY-MM-DD>
   branch: attempt/<slug>/<descriptor>
   summary: <one line: what is being tried>
   verdict: Open — cooking in progress.
   ---
   <what's being changed this session and why>
   ```
   Commit and push immediately — the open branch is the site's "on the stove right now" signal:
   ```
   git add . && git commit -m "Open attempt/<slug>/<descriptor> — <summary>" && git push -u origin attempt/<slug>/<descriptor>
   ```

3. **During/after the cook, record it.** Update the journal with notes (and photos into `recipes/<slug>/photos/`, processed per the new-recipe skill's Photos section — crop, +15% saturation, strip metadata). Edit `recipe.md` with whatever the cook proved — the attempt informs the master version in the same PR. If the session changed the ingredient list, re-estimate the nutrition frontmatter (`satiety`, `serving`, `macros`) to match. Andy's step-specific feedback becomes `> note:` lines on those steps, in his words ("rice was not salty enough last time"). Commit messages state the reason: "Cut sugar to 1 tbsp — sauce was cloying". The commit representing the actual session carries the trailer (blank line before it, nothing after):
   ```
   Record the cook — glaze finally clings

   Cooked: <YYYY-MM-DD>
   ```

4. **Close it out.** Set the journal `verdict:` in commit-message voice, honestly:
   - worked: "Merged — uncovering is what makes the glaze cling."
   - failed: "Failed — the crust never recovered. Recipe unchanged."
   Then `gh pr create` with the verdict as the body. Merge with a true merge commit only (never squash/rebase). The merge body's first line becomes the event label on the site's live tree, so it carries the verdict:
   ```
   gh pr merge <N> --merge --subject "Merge pull request #<N> from andothomas/attempt/<slug>/<descriptor>" --body "<verdict line>"
   ```
   A failed attempt still merges — its journal is the record, recipe.md just stays untouched.

5. **Version tag** (optional, when Andy says the recipe now feels canonical): tag the merge commit `<slug>/vN` (next N for that recipe) and push the tag.

## Margin notes without a session

When Andy has feedback from a past cook but nothing new is on the stove, skip the branch: add `> note:` lines to the relevant steps in his words and commit straight to main — "Margin notes from the first cook — marinade timing, rice salt". No `Cooked:` trailer; it corrects the record without claiming a new attempt. The landing tree ignores these (it shows only recipe events), but each recipe's History tab keeps the diff.
