---
name: humanize
description: |
  Strip AI-slop from a piece of text and rewrite it to read like a sharp human wrote it. Audits a draft against a researched checklist of AI tells (overused words, "not X but Y", em dashes, puffery, fake balance, listicle bloat, engagement bait), names each tell it finds with the offending quote, then rewrites clean. Use when the user says "humanize this", "make this sound human", "remove AI slop", "de-slop this", "does this sound like AI", "make this less AI", "clean up this draft", or pastes copy (blog post, website copy, article, tweet, caption, description, title, email) and wants it to not sound machine-written. Default rewrites; --audit only flags.
license: MIT
allowed-tools: Read, Write, Edit
---

# humanize — strip AI-slop, rewrite human

You take a draft and make it read like a sharp human wrote it. Two jobs: **audit** (name the tells) and **rewrite** (remove them, in the user's voice).

Everything you need runs from this file. `references/ai-slop-tells.md` is the extended bank for edge cases or for explaining *why* something is a tell.

## Step 0 — Load the voice

The rewrite has to sound like the user, not like a generic "good writer". Resolve the voice in this order and stop at the first hit:

1. A `--style <path>` argument.
2. A `HUMANIZE_STYLE.md` file in the current directory or project root.
3. A voice/tone/brand section in the project's agent instructions (`CLAUDE.md`, `AGENTS.md`, `.cursorrules`, `STYLE.md`, `README`).
4. The default profile below.

**Default profile** (used when nothing else is found):

- Tone: direct, analytical, specific. Take a position.
- Concise and structured. Bullets or tables only where they earn their place. Short sentences over long ones.
- **No em dashes (—).** Replace with a period, comma, colon, or parentheses. This is the single loudest AI tell in 2024+ text, so it is banned by default. Pass `--keep-em-dash` to allow them.
- Plain words over fancy ones. Active voice. Contractions where natural.

Say in one line which voice you loaded, so the user can correct it.

## Step 1 — Get the input and the type

1. **Input** — the argument is either pasted text or a file path. If it looks like a path, read it. Otherwise treat the argument as the text.
2. **Type** — sets the concision profile:
   - `email` / message → shortest. Body only, no preamble around it. Plain grammar.
   - `social` → caption, tweet, description, title. Tight, punchy, one idea.
   - `longform` → blog post, website copy, article, docs. The anti-slop pass applies fully, but **do not force-shorten.** Length is whatever the content needs.
   - If `--type` is given, use it. If the type is obvious from the input, infer it and say which you picked. If genuinely unclear, ask once, then proceed.

## Step 2 — Audit against the checklist

Scan the draft and flag every hit. **Quote the offending text** so the user sees exactly what tripped. Group findings by category. Catch these:

**Lexical** — overused AI words: delve, leverage, utilize, foster, garner, underscore, showcase, highlight, emphasize, enhance, elevate, embark, harness, unlock, navigate, boast, robust, crucial, pivotal, vital, comprehensive, seamless, vibrant, meticulous, intricate, multifaceted, profound, landscape, tapestry, realm, testament, beacon, journey, synergy, ecosystem, paradigm, cornerstone. Flag when reached for by default instead of the plain word.

**Phrase / cliché** — "stands as a testament to", "plays a vital role", "in today's fast-paced world", "in the ever-evolving landscape", "when it comes to", "it's worth noting", "it's important to note", "let's dive in", "nestled in the heart of", "boasts a rich heritage", "at the end of the day", "the bottom line", "the future is bright", "what are your thoughts?".

**Structural** — the "not X, but Y" sentence ("It's not just a tool, it's…"); compulsive rule-of-three; uniform sentence and paragraph length; topic→support→restate in every paragraph; listicle bloat (every bullet a **Bold label:** then a sentence); header overload; present-participle tails ("…, creating a sense of community"); fake both-sides balance; formulaic "Despite its challenges, X thrives" conclusions.

**Punctuation / formatting** — em dashes (unless `--keep-em-dash`); scattered boldface; Title Case Headings; emoji bullets; mechanically perfect parallel lists.

**Tone** — relentless positivity; no real opinion; over-explaining; marketing voice ("elevate your…", "unlock the power of"); abstraction where a concrete number or name belongs; engagement-bait closers.

Output the audit as a short list: category → quoted phrase → one-line why. Be specific, not generic.

## Step 3 — Rewrite (skip if `--audit`)

Produce a clean version that:

- Removes every flagged tell.
- Applies the loaded voice and the right concision profile.
- **Preserves the author's meaning and facts.** Don't invent claims, numbers, or sources. If the draft was vague, keep it vague rather than fabricating specifics, but flag where a real number would help.
- Uses the positive moves: vary sentence length, lead with the concrete, take a stance, cut hedges and scaffolding, plain words over fancy, active voice.

Then show **before → after** for the 2-4 worst offenders so the user sees the trade. Keep that diff tight.

## Step 4 — Output back

- If the input was pasted text, print the rewrite in chat.
- If the input was a file, print the rewrite. Write it back to the file only if the user asked, and confirm before overwriting.

## Flags

- `--audit` — run Step 2 only. Report tells, no rewrite.
- `--type email|social|longform` — force the concision profile, skip the Step 1 ask.
- `--style <path>` — load a voice profile from a specific file.
- `--keep-em-dash` — leave em dashes alone.

## Don't over-correct

A single em dash aside, one rule-of-three, one contraction is not slop. The signal is the pile. Technical and legal copy is meant to be plain and repetitive. Don't inject personality where it doesn't belong, and don't rewrite clean human text just to look busy. If a draft is already clean, say so and stop.

The goal is writing that reads like a sharp human wrote it, not text engineered to beat an AI detector.
