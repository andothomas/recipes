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

3. **During/after the cook, record it.** Update the journal with notes (and photos into `recipes/<slug>/photos/`, referenced as markdown images). Edit `recipe.md` with whatever the cook proved — the attempt informs the master version in the same PR. Commit messages state the reason: "Cut sugar to 1 tbsp — sauce was cloying". The commit representing the actual session carries the trailer (blank line before it, nothing after):
   ```
   Record the cook — glaze finally clings

   Cooked: <YYYY-MM-DD>
   ```

4. **Close it out.** Set the journal `verdict:` in commit-message voice, honestly:
   - worked: "Merged — uncovering is what makes the glaze cling."
   - failed: "Failed — the crust never recovered. Recipe unchanged."
   Then `gh pr create` with the verdict as the body. Merge with a true merge commit only (never squash/rebase); the merge message carries the verdict. A failed attempt still merges — its journal is the record, recipe.md just stays untouched.

5. **Version tag** (optional, when Andy says the recipe now feels canonical): tag the merge commit `<slug>/vN` (next N for that recipe) and push the tag.
