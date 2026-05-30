# CLAUDE.md — Inter-Rater Self

Project context for Claude Code. Read before editing.

## What this repo is

A public, dependency-free static site: one interactive HTML page (`index.html`) plus a long-form markdown write-up (`inter-rater-self.md`), served via GitHub Pages. The idea: run the same personality read across several LLMs and read the inter-rater disagreement, not the answers. It is a cognitive-defense / data-sovereignty piece, not a personality quiz.

## Hard invariants — do not violate without explicit instruction

1. **The two deliverables must stay in sync.** `index.html` (the page prose) and `inter-rater-self.md` say the same things in the same language. Until the build pipeline exists, any change to framing, claims, or the prompt must be applied to **both** files in the same commit. Flag drift if you see it.

2. **No browser storage, ever.** No `localStorage`, `sessionStorage`, cookies, or IndexedDB. Saved runs are downloaded as files and re-uploaded by the user. This is both a hard rendering constraint and the entire privacy thesis — data stays in the user's files, nothing is uploaded or persisted by the page.

3. **Single self-contained `index.html`.** No build step, no bundler, no npm deps. The only external resources are the two Google Fonts `<link>` tags. Inline all CSS and JS. It must run by opening the file.

4. **Citation accuracy is non-negotiable.** This is published under a real name.
   - The ETH Zurich study is **arXiv 2604.19785** (Cögendez, Zimmermann, Zufferey): fine-tuned **RoBERTa** classifiers, ternary classification, beat chance on several traits, **+44% relative for extraversion** on relationship/reflection chats. There is **no "61%"** in that paper — do not reintroduce it.
   - The weak-regime numbers (κ < 0.10; r ≈ 0.27; r ≈ 0.12 default-assistant) come from arXiv 2507.14355 and 2405.13052. Keep them attached to the right papers.
   - **Verify every external link resolves before committing.** Never invent a citation or a statistic.

5. **Preserve the two-regime framing.** Trained-classifier-on-your-history = the real threat; stock-prompt-from-memory = the weak toy. Never let the page imply the prompt method is accurate.

6. **Keep the drift caveats.** A single jump can be you, the model, or run-to-run noise — distrust single jumps, trust sustained trends. Per-model trend lines key on the **exact** model-name string; the name-match warning stays in the UI.

7. **Tone:** dry, precise, mechanism-first. No hype, no cold-reading language, no rebutting absent text.

## Before any push — validation

Run a headless smoke test and confirm zero JS errors and that: the example renders on load; pasting JSON computes the spread; the prompt copy button works; saving a run downloads dated JSON containing `per_model`; reloading ≥2 dated run files renders the consensus trend and the 5 per-model panels.

```bash
# one-time
pip install playwright --break-system-packages && python -m playwright install chromium
# then run your smoke test against index.html and assert no pageerror events
```

## Roadmap (not yet built)

- **Repeated-sampling pipeline:** run each model N times per session, store all samples, plot mean ± band per trait. Noise becomes band width; only movement beyond the band reads as signal.
- **Single-source build:** generate both `index.html` and `inter-rater-self.md` from one source of truth; add CI to link-check citations and run the render smoke test on every PR. Once this lands, invariant #1 is enforced by the build instead of by hand.

## Common tasks

- *Edit the prompt:* change it in `index.html` (`#promptText`) and the `## The prompt` block in the md, identically.
- *Add a citation:* search for it, fetch it, confirm it resolves, then add to both the page's Further Reading and the md, with an accurate one-line description.
- *Touch the dashboard JS:* re-run the smoke test before committing.
