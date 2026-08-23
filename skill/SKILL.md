---
name: linkedin-post-writing
description: >-
  LinkedIn posts in User's voice, max 3000 chars.
version: 1.0.0
author: Hermes Agent
metadata:
  hermes:
    category: productivity
    tags: [linkedin, social-media, writing, XXXXXX, voice-calibration]
---

# LinkedIn Post Writing — User

Write compelling LinkedIn posts matching User's voice and constraints.
User is an infrastructure engineer (NOT a software developer) based in
Serbia, EU citizen (Serbian + Czech), running a Hermes Agent AI stack.

## When to Use

- User asks for a LinkedIn post, LinkedIn draft, or LinkedIn content
- A forwarded task from another Hermes profile asks you to write a LinkedIn post
- User asks to merge multiple LinkedIn post drafts into one final version
- User asks to expand or edit an existing LinkedIn post draft

## Voice Calibration

User's LinkedIn voice (derived from 3+ previous posts):

### Core Traits
- **Personal and enthusiastic** — first person, story-telling, "I was the geek with THE FASTEST AI IN SERBIA"
- **Infrastructure engineer perspective** — NOT a software developer, NOT a coding benchmark
- **Real numbers, real tasks** — token counts, TPS, latency, hardware specs, actual work done
- **Sovereign AI angle** — data stays local, processed in Serbia, no bytes sent abroad
- **Praise for Orion Telekom AI Factory** — standing ovations, gratitude, "they brought DGX-B200 to Serbia"
- **Excitement markers** — exclamation marks, caps for emphasis ("PERFECT AI Companion"), rhetorical questions ("Isn't that amazing?!?!?!")

### Recurring Themes
- NVIDIA DGX-B200 hardware (Orion AI Factory, Zemun, Serbia)
- Token consumption (millions of tokens, burn rates, cost analysis)
- Quantization (NVFP4 vs BF16, near-lossless, throughput)
- Sovereign AI infrastructure (government workloads, enterprise data, local processing)
- Orion Telekom AI Factory praise (standing ovation, GPU-time billing, gratitude)
- Real-world agentic tasks (scraping, OCR pipelines, multi-agent orchestration, data extraction)
- Model comparison (DeepSeek V4 Flash/Pro, Kimi K2.6, Owl Alpha, GLM-5.2)

### What to NEVER Include
- Coding benchmarks — User is an infrastructure engineer, not a developer
- Generic AI hype — every claim backed by real numbers
- Formal discourse markers — "Furthermore," "Moreover," "It is important to note"
- AI-slop vocabulary — "delve," "tapestry," "landscape," "pivotal," "underscore"
- Symmetrical paragraphs — vary length dramatically
- See `humanizer` skill for the full 29 AI content patterns to avoid

## Workflow

### Step 1: Read the Context File

If a context file is provided (e.g., `linkedin_post_context.md`), read it
with `read_file`. It typically contains:
- Key facts and test data
- Previous LinkedIn posts for style reference
- Author profile and constraints
- Specific findings to include

### Step 2: Write the Draft

Write in User's voice (see Voice Calibration above). Structure:
1. **Hook** — bold claim, personal excitement, story continuation
2. **The setup** — what was tested, hardware specs, key numbers
3. **The test** — what was actually done (math, agentic tasks, real work)
4. **The findings** — accuracy, speed, comparison numbers
5. **The bigger picture** — sovereign AI, why it matters for Serbia
6. **Praise** — Orion Telekom AI Factory, standing ovation
7. **Closer** — personal note, "I enjoyed every token"

### Step 3: Character Count Management

LinkedIn posts max 3000 characters including spaces.

```bash
wc -m /path/to/draft.md
```

**Use `wc -m` (characters), NOT `wc -c` (bytes).** For Unicode text
(em dashes, special chars, en dashes), bytes differ from characters.
An em dash is 3 bytes in UTF-8 but 1 character. LinkedIn counts
characters, not bytes. Using `wc -c` will undercount and you will
silently exceed the limit.

If over 3000:
- Trim redundant words ("you need" → "need", "That is" → "That's" or cut)
- Remove filler phrases
- Combine short paragraphs
- Use `patch` for targeted edits, not full rewrites

If user asks to "expand to near 3000":
- Add opinion/positioning paragraphs
- Add specific technical details from the context file
- Add personal closer (e.g., "I enjoyed every token")
- Verify with `wc -m` after each addition

### Step 4: Run Humanizer Analysis (MANDATORY)

This step is NOT optional. The humanizer catches statistical AI tells
that are invisible to manual review. Run it before AND after edits.

```bash
python3 skills/creative/humanizer/scripts/humanizer_analyze.py \
  --file /path/to/draft.md
```

**Most common fail: em dash overuse (pattern C14).** Technical drafts
use em dashes at 3-5x the human rate. Replace with commas, periods,
or -- (User's style). This single fix often clears the only failing
metric in an otherwise clean draft.

Target metrics after humanization:
- Burstiness CV > 0.45 (HUMAN-LIKE)
- Perplexity > 60 (HUMAN-LIKE)
- Discourse markers: 0 found
- AI vocabulary: 0 found
- Em dashes: 0 (or at most 1 per 1000 words)
- Overall score: 2.0/2.0 (LIKELY HUMAN-WRITTEN)

### Step 5: Apply Humanizer Fixes

Based on the analysis report, apply fixes using `patch`:

- **Em dashes (C14)**: Replace with commas, periods, or -- (User's style)
- **AI vocabulary (B7)**: Remove "delve", "landscape", "pivotal", etc.
- **Discourse markers**: Remove "Furthermore", "Additionally"
- **Burstiness**: Add short punchy sentences between long ones
- **Passive voice (B13)**: Convert to active voice
- **Rule of three (B10)**: Break forced triplets into pairs or singles

### Step 6: Re-Count and Re-Analyze

After humanizing, character count may have changed. Re-run `wc -m`
and re-run the analyzer. The character-limited humanization loop is:

1. Write draft → 2. `wc -m` → 3. If over limit, patch-trim → 4. Run
   humanizer_analyze.py → 5. If fails, patch fixes → 6. Re-count +
   re-analyze → repeat until both constraints satisfied

Goal: 2,900-3,000 characters with humanizer score 2.0/2.0.
Land near the limit but not over.

### Step 7: Save the Draft

Save to the path specified in the task. Use `write_file`.

### Step 8: Reply via CMUX (if forwarded task)

If this is a forwarded task from another Hermes profile, reply back using
the 3-step cmux sequence:

```bash
cmux send --workspace <UUID> "MESSAGE" && \
cmux send --workspace <UUID> " " && \
cmux send-key --workspace <UUID> Enter
```

**Pitfall:** `cmux send --workspace <UUID> ""` fails with "send requires
text". Use `" "` (space) as the second command, not an empty string.
This is already documented in the `forward` skill.

### Step 9: End with FINISH

Always end with `FINISH FINISH FINISH` + brief summary. This signals
completion to the orchestrating agent in the LLM Fusion pattern.

## Multi-Draft Merge Pattern

When asked to merge N drafts from different Hermes profiles into one final
post:

1. Read all N draft files with `read_file`
2. Read the context file for style reference
3. Identify the BEST elements from each draft:
   - Best hook/opening line
   - Best technical explanation
   - Best sovereign AI framing
   - Best Orion praise
   - Best closer
4. Synthesize into one post (do NOT just concatenate — rewrite as a
   single coherent narrative)
5. Verify character count with `wc -m`
6. Save to the specified final path
7. Notify the orchestrator via cmux

See `forward` skill Mode D (LLM Fusion) for the general parallel dispatch
pattern.

## Character Trimming Strategy

When a draft exceeds 3000 chars, not all trims are equal. Ranked by
effectiveness:

1. **Quoted external text** — The single best trim target. If the post
   includes a long quote (e.g., a model comparison verdict), shorten it
   by removing clauses. The quote is external content, so trimming it
   doesn't change the author's voice, and the humanizer doesn't flag
   quoted text as AI-slop. One quote trim can save 30-50 chars.

2. **Redundant adverbs** — "exactly", "actually", "really",
   "requirements", "for context". These add chars without meaning. Easy
   2-5 char wins, but you need many of them.

3. **Duplicate descriptors** — "main workhorse" when "workhorse" was
   already established. "same model name, same parameter count, same
   infrastructure requirements" → drop "requirements".

4. **Filler phrases** — "for context", "in order to", "knowing that I
   have". See humanizer pattern E23.

5. **Last resort: cut a paragraph** — If you're still 50+ over after the
   above, merge two short paragraphs into one. Never cut the hook, the
   quote, or the closer.

### Write Clean Once

The preferred workflow is to apply humanizer patterns DURING the
initial draft, not after. Writing generic prose then fixing it wastes
one full analysis cycle and usually requires 3-5 patch iterations. When
the draft is written with burstiness, specificity, and no AI-slop from
the start, the first humanizer analysis often passes 2.0/2.0 with zero
fixes needed. See `references/deepseek-v4-flash-0731-post-2026-08-04.md`
for a worked example where this approach worked on the first pass.

## Pitfalls

1. **Em dashes are the #1 humanizer fail.** Technical drafts use em
   dashes (—) at 3-5x the human rate. User uses -- (double hyphen) or
   commas, never em dashes. Replace ALL em dashes before running the
   humanizer analysis. This is the most common single fix needed.

2. **Character limit creep**: First drafts often exceed 3000 chars. Always
   check with `wc -m` (NOT `wc -c` — bytes differ from chars for Unicode)
   before saving. Iterate with `patch` to trim.

3. **Empty string cmux send**: `cmux send --workspace UUID ""` fails. Use
   `" "` (space) instead. Already documented in `forward` skill.

4. **Voice drift toward generic AI-slop**: The default LLM writing style
   is formal, symmetrical, and full of discourse markers. User's voice
   is personal, punchy, and enthusiastic. Load `humanizer` skill and
   apply its 29 AI content pattern removal if the draft reads like generic
   AI output.

5. **Coding benchmark framing**: User is NOT a software developer. Never
   frame tests as "coding benchmarks." Frame as infrastructure behavior,
   latency, throughput, multi-agent orchestration, real-world data
   pipelines.

6. **Missing Orion praise**: Every post about Orion AI Factory must include
   explicit praise. "Standing ovation" is a recurring motif. Do not omit.

7. **Missing sovereign AI angle**: The "data stays local, processed in
   Serbia, no bytes sent abroad" theme is mandatory for Orion posts.

8. **FINISH FINISH FINISH truncation**: The system may truncate responses
   that include `FINISH FINISH FINISH` with a summary. If the response
   gets truncated, the user may see repeated empty FINISH messages. This
   is a system behavior, not a content issue. Keep the summary SHORT to
   avoid truncation.

## Related Skills

- `humanizer` — Apply to all user-facing prose. Removes AI-slop patterns,
  varies sentence structure, injects human voice.
- `forward` — Cross-agent CMUX messaging. Contains the 3-step cmux send
  protocol and LLM Fusion (Mode D) parallel dispatch pattern.
- `cross-profile-coordination` — Multi-profile coordination patterns.
- `professional-document-creation` — For formal documents (proposals,
  reports, letters). LinkedIn posts are social media content, not formal
  documents — use this skill instead.

## Reference Files

- `references/glm-5-2-nvfp4-post-2026-07-26.md` — Worked example: GLM-5.2
  NVFP4 LinkedIn post session. 3-draft LLM Fusion merge pattern, character
  count management, user expansion request, best-elements table, CMUX reply
  protocol, and key lessons.
- `references/deepseek-v4-flash-0731-post-2026-08-04.md` — Worked example:
  DeepSeek V4 Flash 0731 vs Regular benchmark post. Write-clean-once workflow
  (humanizer passed 2.0/2.0 on first analysis), 5-iteration char trimming log,
  quote-trimming technique, content preservation/removal table.