# Five Models, One You, and the Spread Between Them

A model doesn't know you. With memory and chat-history search on, it samples a slice of what you've said and infers from it — not because it "knows" you, but because it can read patterns in your past conversations to estimate where you fall on a scale. Over time, your history across apps like ChatGPT, Claude, Grok, or Gemini builds up a *sample* of how you write, what you ask, and what you avoid. This works best with real history — ideally 20+ genuine conversations on at least one app.

The fix for any single model's overconfidence isn't a better prompt. It's more samplers. Run the same read across several models and stop reading the answers — read the *disagreement*. When multiple models land on the same score, that's a signal worth taking seriously. When they fan out — 36 points on one axis — that trait is either genuinely context-dependent or not recoverable from text. Either way the spread tells you more than any one model's certainty. This is just inter-rater reliability pointed at yourself.

MBTI, Enneagram, and the Big Five are tools for self-reflection, not fixed labels — the framework traces back to Carl Jung's personality theory, later developed into the MBTI by Isabel Briggs Myers and her mother Katherine Briggs. A read is a starting point for your own thinking. This is not a substitute for a mental health professional or clinician; take what's useful and set aside the rest.

## Method

Paste the prompt into 3–5 models you've actually talked to (20+ conversations each, memory enabled). Don't tell any model what another said — you want independent reads, not influenced ones. Each model returns prose plus a JSON block. Collect the JSON, drop it into the dashboard, and look at how much the scores differ between models.

## The prompt

> Perform a personality analysis of me from our conversation history. Don't ask questions — use what you've observed. I want accuracy, not flattery, and I'm comparing your output against other models.
>
> Score **Big Five / OCEAN** 0–100 (Openness, Conscientiousness, Extraversion, Agreeableness, Neuroticism), each with a specific observed pattern as evidence. Then give **Enneagram** core type, wing, and confidence, citing the conversational behavior that typed me — how I handle being wrong, disagreement, what engages me. Then **MBTI** four-letter type with per-axis confidence; flag close calls.
>
> Note where you have thin data instead of guessing.
>
> Then output ONLY this JSON block — no prose inside it, no markdown fences:
>
> ```json
> {"model":"", "ocean":{"o":0,"c":0,"e":0,"a":0,"n":0},
>  "enneagram":{"type":0,"wing":0,"confidence":0.0},
>  "mbti":{"type":"","axis_confidence":{"ei":0.0,"sn":0.0,"tf":0.0,"jp":0.0}}}
> ```

## Reading the output

Convergence across models is your steadier signal; a wide spread means the data is thin, not that a hidden depth surfaced — so hold any single score loosely. Categorical axes get the same treatment: an Enneagram type or MBTI letter the raters can't agree on is one they can't pin from your text. If models can't agree on a trait, the issue is the data, not you.

And hold the whole snapshot loosely. A personality read is a tool, and tools can be repurposed. The same inference that helps you reflect can be used by someone else to profile you, or to build a narrative about you that isn't true. The protection isn't a better prompt — it's keeping your own record.

## What the research actually says

The two regimes don't agree, and that's the point. When researchers **fine-tune a dedicated classifier on thousands of real chats against ground-truth personality tests**, it profiles people meaningfully — an ETH Zurich team trained RoBERTa models on 62,090 chats from 668 users and beat chance across several traits, with extraversion inference improving roughly 44% over baseline on relationship and personal-reflection conversations. That's the privacy threat: a party holding your full history, with labels and a trained model, can read you — and the same paper frames this as enabling profiling, targeted influence, and what its authors call "cognitive surrender," the tendency to lean on AI instead of your own judgment.

But that is **not** what this prompt does. Asking a stock chat model to type you from memory — unlabeled, untrained — is the weak regime. Peer-reviewed benchmarks put inter-rater agreement near zero (Cohen's κ < 0.10) and validity at r ≈ 0.27, dropping to r ≈ 0.12 in default assistant mode. So the capability is real enough to fear from a data-holder who trains on you, and weak enough to distrust from a chat box that doesn't. The variance you measure here is the honest version of that gap.

## Track drift over time

A single read is a snapshot — and a snapshot can be cherry-picked by someone else to build a story about you. The defense is holding your own record. Save each run, date it, and watch the trend over months. When you hold the baseline, no one else's single snapshot gets to define you, and you can see when a model's read drifts — which tells you something changed, in you or in the model. The dashboard exports each run as a small dated JSON file you keep; the record lives in your files, not in the page or any provider, and nothing is uploaded.

There are two views. The **consensus** trend shows how the averaged read moves over time. The **per-model** view breaks that apart — one line per model per trait — so you can see whether a single model's read climbs while the others hold flat (that's the model changing, not you) versus every model moving together (likelier you). Per-model lines only connect when the model's name matches exactly across runs, so label each model the same way every session or the same rater reads as two.

Read drift skeptically. A change between two runs has three possible causes you can't fully separate: you changed, the model changed, or it's run-to-run noise — the same model types you differently on different days because sampling is stochastic. Distrust a single jump; trust a sustained trend across several runs. A future version will run each model several times per session and plot the band rather than the point, so noise shows up as band width and only movement beyond the band reads as real.

## Further reading

- [Can LLMs Infer Personality Traits from Chat History?](https://arxiv.org/abs/2604.19785) — arXiv (Cögendez, Zimmermann, Zufferey, ETH Zurich). *The threat model: fine-tuned RoBERTa classifiers on 62k real chats + ground truth; better than chance, +44% for extraversion on relationship/reflection chats.*
- [Can AI ascertain personality from your ChatGPT history?](https://techxplore.com/news/2026-05-ai-personality-traits-chatgpt-history.html) — TechXplore. *Press write-up of the ETH study; "cognitive surrender" and large-scale-profiling framing.*
- [LLMs Can Infer Personality from Free-Form Interactions](https://arxiv.org/abs/2405.13052) — arXiv (Peters, Cerf, Matz). *Moderate accuracy; near-zero (r ≈ .12) in default assistant mode.*
- [Can LLMs Infer Personality from Real-World Conversations?](https://arxiv.org/abs/2507.14355) — arXiv (Zhu, Jin, Coifman). *Real benchmark: weak validity (r = .27), low inter-rater agreement (κ < .10).*
- [Myers-Briggs official overview](https://www.myersbriggs.org/my-mbti-personality-type/myers-briggs-overview/) · [Big Five / OCEAN — Simply Psychology](https://www.simplypsychology.org/big-five-personality.html) · [Enneagram type descriptions](https://www.enneagraminstitute.com/type-descriptions/) · [Carl Jung — Britannica](https://www.britannica.com/biography/Carl-Jung)

---

*Methodology and dashboard built with Claude (Anthropic, Opus 4.8) as part of ongoing AI–human cognitive-defense work. The premise — treat each model as one noisy rater and read the disagreement — came out of red-team and evaluation practice, where inter-rater spread is the data you actually trust.*
