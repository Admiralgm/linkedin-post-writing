# Worked Example: DeepSeek V4 Flash 0731 vs Regular LinkedIn Post

**Date:** 2026-08-04
**Session profile:** AGENT (glm-5.2 / ollama-cloud)
**Character count:** 2,998 / 3,000
**Humanizer score:** 2.0/2.0 (LIKELY HUMAN-WRITTEN) — passed on first analysis

## Task

User provided a raw draft (~3700 chars) about benchmarking DeepSeek V4 Flash
0731 vs Regular using agentic long-chain tasks in HERMES AI framework.
Asked for humanization + LinkedIn formatting with 3000 char limit.

## Key Workflow Decisions

### 1. Draft written with humanizer patterns baked in from start

Instead of writing a generic draft and then running humanizer to fix it,
the rewrite was done applying all 7 methods + 29 pattern removals during
the initial draft. Result: first humanizer analysis passed 2.0/2.0 with
no fixes needed. This is the preferred workflow — write clean once, not
write sloppy then fix.

### 2. Character trimming sequence (5 iterations)

| Iteration | Chars | Action |
|-----------|-------|--------|
| 1 (initial draft) | 3236 | Over limit by 236 |
| 2 | 3056 | Trimmed specs paragraph + pricing paragraph |
| 3 | 3024 | Shortened ChatGPT 5.6 quote (removed "In this benchmark, however," and "distinguished verified facts from unresolved questions" and "database construction") |
| 4 | 3019 | Removed "main" from "main workhorse" (second occurrence) |
| 5 (final) | 2998 | Removed "exactly" + "requirements" from para 3 |

### 3. Quote trimming technique

The most effective single trim was shortening the ChatGPT 5.6 comparison
quote (iteration 2→3, saved ~37 chars). When a post includes a long quoted
block from an external source, the quote is the best place to trim because:
- It's external content — shortening it doesn't change the author's voice
- Quotes are dense — removing clauses preserves meaning
- The humanizer doesn't flag quoted text as AI-slop (it's attributed)

### 4. Content preserved from user's draft

- All technical specs (284B/13B/1M, 753B/40B, $20/month, 6x/15x ratios)
- The ChatGPT 5.6 comparison quote (shortened but all key findings kept)
- The "sly Chinese AI marketing" skepticism angle
- The "did not reach for GLM 5.2 once" punchline
- The OPTIMIZATION conclusion about post-training vs parameter scaling

### 5. Content removed/changed from user's draft

- "continuously galloping AI ecosystem" → "That's how fast this ecosystem moves now"
- "tech geek like myself who wants to be established as a recognized AI expert" → "If you want to be taken seriously as an AI expert"
- "shaken my personal AI cage" → removed (AI-slop vocabulary)
- "preach and teach about it to other people" → removed (filler)
- "It can not be that... or so I thought?!?!" → "Come on." (punchier)

## Final Post

File: ~/Desktop/linkedin-draft.md (2,998 chars)

## Humanizer Metrics (Final)

| Metric | Value | Status |
|--------|-------|--------|
| Burstiness CV | 0.65 | HUMAN-LIKE (>0.45) |
| Perplexity Proxy | 82.1/100 | HUMAN-LIKE (>60) |
| Discourse Markers | 0.0/1000 | HUMAN-LIKE (0 found) |
| AI Vocabulary | 2.2/1000 | HUMAN-LIKE (<3) |
| Em Dashes | 0 | OK |
| Passive Voice | 0.0% | OK |
| Chatbot Artifacts | 0 | OK |
| Overall Score | 2.0/2.0 | LIKELY HUMAN-WRITTEN |