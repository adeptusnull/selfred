# Inter-Rater Self

Treat each LLM as one noisy rater. Run the same personality read across several models, then read the **disagreement** instead of the answers. Where models converge, you have a steadier signal; where they fan out, the data is thin. It's inter-rater reliability pointed at yourself — and a small act of data sovereignty, since you keep your own record rather than trusting one provider's snapshot.

- **Live page:** `https://<YOUR-USER>.github.io/inter-rater-self/`
- **Long-form write-up:** [`inter-rater-self.md`](./inter-rater-self.md)
- **Interactive dashboard:** [`index.html`](./index.html) (paste each model's JSON, see the spread; save dated runs and track drift over time)

## What it does

1. Prompt 3–5 models you've actually talked to; each returns prose plus a strict JSON block.
2. Paste the JSON into the page. It renders an OCEAN radar overlay, per-trait variance (range / σ / CV), and categorical agreement on Enneagram and each MBTI axis.
3. Save each run as a dated JSON file you keep. Re-run later, drop the files back in, and watch two trend views — averaged **consensus** drift, and **per-model** drift (which rater moved).

## Privacy

Nothing is uploaded or stored server-side. The page is a single static file; saved runs live in your own files. That's the point: no one else's single snapshot gets to define you.

## What this is not

A stock chat model typing you from memory is the *weak* measurement regime — peer-reviewed benchmarks put inter-rater agreement near zero (κ < 0.10). The accurate, invasive profiling happens when someone *fine-tunes a classifier on your full history against ground truth* (see references in the write-up). This tool measures rater disagreement, not personality, and is not a substitute for a clinician.

## Roadmap

- Repeated-sampling pipeline: run each model N times per session and plot the error band, so run-to-run noise shows up as band width and only movement beyond it reads as real.
- Single-source build that generates `index.html` and `inter-rater-self.md` from shared content, with CI link-checking and a headless render smoke test.

---

Built with Claude (Anthropic, Opus 4.8) as part of ongoing AI–human cognitive-defense work.
