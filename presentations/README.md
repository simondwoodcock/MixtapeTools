# Presentation Tools

> **What's in this folder:** Philosophy, principles, and prompts for creating effective slide presentations with AI assistance.

---

## Contents

### `rhetoric_of_decks.md` — Practical Principles (Condensed)

**What it is:** A condensed guide to what makes presentations work. The actionable principles extracted from years of giving and watching academic talks.

**How to use it:**
- **Read it** to internalize the principles
- **Reference it** when designing a new deck
- **Paste it** into Claude when you want it to follow these principles

**Core concepts:**
- **The Three Laws**: Beauty is function, cognitive load is the enemy, slides serve speech
- **Aristotle**: Ethos (credibility), Pathos (emotion), Logos (logic)
- **MB/MC equivalence**: Every slide should have the same marginal benefit to marginal cost ratio
- **Titles are assertions**: "Treatment increased distance by 61 miles" not "Results"
- **Bullets are defeat**: Find the structure hiding in your list

---

### `rhetoric_of_decks_full_essay.md` — The Full Intellectual Framework

**What it is:** A 600+ line essay tracing rhetoric from Aristotle through LLMs. The complete intellectual foundation behind the practical principles.

**Covers:**
- The history of rhetoric (Aristotle, Cicero, Quintilian, Augustine)
- Technology's transformation of persuasion (printing press through PowerPoint)
- The economics of attention (Netflix streaming vs. theater viewing)
- Defamiliarization (Shklovsky's "making the stones feel like stones")
- The academic job market talk as case study
- What LLMs see when trained on presentations
- Tufte's critique and its limits

**When to use it:** When you want to understand *why* the principles work, not just *what* they are.

---

### `deck_generation_prompt.md` — The Prompt (now also a skill: `/beautiful_deck`)

> **This prompt is now available as a Claude Code skill.** Invoke `/beautiful_deck` instead of pasting the prompt. See [`skills/beautiful_deck/`](../skills/beautiful_deck/) for documentation. The skill codifies the full pipeline: audience triage, original theme design, Narrative → Application → Picture → Codeblock → Technical sequencing, code-first figure generation, zero-warning compile loop, `/tikz` visual cleanup, and rhetoric + graphics audit sub-agents. The prompt below is preserved here as reference for the philosophy behind the skill.

**What it is:** A tested prompt for generating Beamer presentations with Claude Code, including an iterative multi-agent review process.

**How to use it:**
1. Open Claude Code in your project directory
2. Have existing content ready (notes, old slides, paper draft)
3. Customize the bracketed sections in the prompt
4. Paste the prompt
5. Follow the iterative workflow

**The Iterative Workflow:**

```
┌─────────────────────────────────────────┐
│  Step 1: Build deck with MB/MC          │
│          equivalence, compile           │
└──────────────────┬──────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────┐
│  Step 2: Fix ALL compilation warnings   │
│          (no matter how small)          │
└──────────────────┬──────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────┐
│  Step 3: Check silent visual errors     │
│          (Tikz coords, ggplot labels)   │
└──────────────────┬──────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────┐
│  Step 4: Recompile, check flow          │
│          and MB/MC equivalence          │
└──────────────────┬──────────────────────┘
                   │
                   ▼
              ┌────┴────┐
              │ Pass?   │──No──┐
              └────┬────┘      │
                   │           │
                  Yes          │
                   │           │
                   ▼           │
          ┌────────────────┐   │
          │ Human reviews  │   │
          └────────────────┘   │
                               │
               ┌───────────────┘
               │
               ▼
        Back to Step 1
```

**Key insight:** The goal is NOT maximum cognitive density. The goal is **smoothness** — consistent MB/MC ratio across all slides. Exception: deliberate "jump scares" for rhetorical effect.

---

## The Silent Killers

LaTeX compilation succeeds with two types of output: **warnings** and **errors**. Errors stop compilation; warnings don't. But warnings matter enormously for visual quality.

### LaTeX Warnings You Must Fix

**Overfull `\hbox`**: Content is too wide for its container. LaTeX pushes text into the margin. You'll see a black box in draft mode or text bleeding off the slide.

**Underfull `\hbox`**: Content is too sparse. LaTeX stretches whitespace awkwardly to fill the line, creating uneven spacing.

**Overfull/Underfull `\vbox`**: Same problems but vertical—content either overflows the page bottom or leaves awkward vertical gaps.

**Why these matter:** They indicate your layout doesn't fit. The visual artifact may be subtle (slightly uneven spacing) or obvious (text clipped at margins), but it always looks unprofessional.

### Visual Errors That Don't Warn

LaTeX warnings catch box overflow issues but **NOT** coordinate or positioning problems in TikZ, ggplot2, or matplotlib. These compile silently but look wrong:

**TikZ:**
- Labels not where you think they are (coordinates miscalculated)
- Timeline endpoints misaligned with content
- Arrows pointing to wrong nodes
- Shape constraints forcing misplacement

**ggplot2 / matplotlib:**
- Axis labels cut off at figure boundary
- Legends obscuring data points
- Text sizing inconsistent across panels
- Tick marks misaligned with gridlines

**Why warnings don't catch these:** LaTeX doesn't know what you *intended*. If you specify `\node at (5,3)` but meant `(3,5)`, the code is syntactically valid—it just draws the wrong picture.

### The Two-Pass Workflow

1. **Compile and fix all warnings** — No overfull/underfull boxes allowed
2. **Visually inspect TikZ/figures** — Claude cannot verify coordinates from code alone; either look at the PDF or have Claude read the PDF and describe what it sees
3. **Recompile after fixes** — New warnings may emerge

The goal is zero warnings AND visual correctness. LaTeX warnings are necessary but not sufficient.

---

## Examples

### `examples/rhetoric_of_decks/` — The Philosophy Deck

A 45-slide Beamer presentation that teaches the rhetoric of decks philosophy itself. This deck practices what it preaches:
- Custom professional color palette (DeepNavy, Teal, WarmOrange)
- TikZ diagrams throughout
- MB/MC equivalence across slides
- Titles as assertions
- Zero compilation warnings

Contains: LaTeX source, compiled PDF, figure generation R script, all figure PDFs.

### `examples/gov2001_probability/` — A Lecture Deck

A complete probability lecture deck (Harvard Gov 2001) showing:
- A full probability lecture deck
- Custom Beamer theme (inline in preamble)
- The rhetoric principles in practice
- What the iterative workflow caught
