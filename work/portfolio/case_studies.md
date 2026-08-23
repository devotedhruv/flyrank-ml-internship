# Portfolio Cases & Brand Framing

## Voice Card
> Direct, grounded, precise, warm, no buzzwords.

---

## Case Study 1: ML Task Framing & Content Quality Pipeline

### 1. The Problem
Our content pipeline relied on manual checks to flag articles for review. Editors spent 60% of their time reviewing obvious, high-performing articles rather than focusing on edge cases, creating a publishing bottleneck.

### 2. What I Did & Decided
- **Framed as Classification:** Instead of predicting raw view counts, I framed it as a binary classification task (`is_high_engagement`: `1` or `0`).
- **Selected a Proxy Target:** Used readership (`views > 5000`) as a proxy for high-value content.
- **Balanced Metric Choice:** Selected **F1-Score (target ≥ 0.82)** to keep low-quality content off the site while keeping the manual review queue small.
- **Proved ML over Fixed Rules:** Showed that rules like `IF word_count > 1000` fail because quality depends on non-linear combinations of length, reader time, and topic.

### 3. What Came Of It
- Built an executable Jupyter notebook (`work/notebooks/w02_ml_task_framing.ipynb`) showing the unit of analysis, dataset slice, and class distribution.
- Created a clear decision rule routing confident `1`s straight to publishing and sending lower-confidence predictions to an editor review queue.

---

## Case Study 2: Rapid Prototyping & AI Web Creation

### 1. The Problem
Building custom web prototypes traditionally takes days of setup, blocking fast iteration on user feedback and making real-time testing impractically slow.

### 2. What I Did & Decided
- **Leveraged AI Toolchains:** Built a rapid prototyping workflow using Vercel v0, Lovable, and Firebase Studio to turn structured specs directly into functional UI.
- **Prioritized Function over Bloat:** Stripped out auto-generated fluff and aligned styling to clean, high-contrast black themes.
- **Core-First Architecture:** Maintained strict manual control over data schema and route handling while delegating layout rendering to AI code generation.

### 3. What Came Of It
- Cut prototype turn-around time from days to under two hours while keeping codebases maintainable.
- Published technical digital content explaining practical "vibe coding" workflows to an audience of tech builders.

---

## Before / After Copy Edit

* **Generic AI Line (Before):** 
  > *"I leveraged a cutting-edge, results-driven machine learning classification algorithm to seamlessly synergize our content strategy and optimize editorial efficiency."*

* **My Edited Version (After):** 
  > *"I framed our content quality check as a binary classification problem, using high readership as a target proxy so editors only review low-confidence submissions."*

---

## Bio & Contact / CTA

* **Bio:** 
  Developer and tech content creator focused on AI tools, web development, and practical machine learning workflows. I specialize in turning ambiguous technical problems into clean, runnable code and automated systems.

* **Call to Action (CTA):** 
  Interested in collaborating on AI tools, custom web architectures, or technical content? Contact me via [GitHub / LinkedIn / Email].
