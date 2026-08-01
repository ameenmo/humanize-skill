# humanize

An [Agent Skill](https://code.claude.com/docs/en/skills) that strips AI-slop from a draft and edits it into something a person would be glad to have written.

It runs two passes:

1. **The slop pass** — the post-2022 tells. Binary contrasts, colon reveals, puffery, synonym cycling, em dashes, the whole bank.
2. **The Ogilvy pass** — the older problems that survive after the slop is gone. Fog, jargon, unverified quotes, and a buried ask.

Then it either reports the findings (`--audit`) or makes the edits, and checks its own output against [`eval.md`](eval.md) before you see it.

It works on blog posts, website copy, articles, tweets, captions, titles, product descriptions, and emails.

**Why two passes:** a draft can pass every AI-detection heuristic and still be bad writing. Removing slop is hygiene. It doesn't make the argument land, the ask clear, or the quotes real.

## Two modes, because "humanize" means two different things

Most de-slop tools quietly pick one of these and get the other wrong.

**Preserve (default).** Remove the AI tells, leave the writer alone. Your vocabulary, cadence, bluntness, humor, and digressions are the point. A rough draft with a real voice should still sound like you afterward. This is what you want for essays, blog posts, anything with an author.

**Conform.** Remove the tells *and* rewrite toward a target voice. This is what you want for brand copy and docs, where the writing has to match a house style rather than a person.

You get conform mode when you pass `--conform` or `--style <path>`, or when a `HUMANIZE_STYLE.md` exists in your project. Otherwise you get preserve. Either way, tell removal runs; only voice targeting is mode-dependent.

## What it catches

| Pattern | Smells like |
|---|---|
| Binary contrast | "It's not X. It's Y." |
| Negative listing | "Not a framework. Not a library. A compiler." |
| Throat-clearing | "Here's the thing…" |
| Faux-insight setup | "What nobody tells you…" |
| Colon reveal | "The best part: it learns." |
| Superficial analysis | "…, highlighting the team's commitment" |
| Importance puffery | "marks a pivotal moment" |
| Weasel attribution | "experts agree", "studies show" |
| Fake-strong verbs | "serves as a centralized hub for" |
| Synonym cycling | the agent, then the assistant, then the tool |
| Fake-profound kicker | the closing metaphor that mic-drops |
| Summary-recap ending | "In conclusion…" |
| Lexical tells | delve, leverage, robust, pivotal, tapestry, testament |
| Formatting slop | em dashes, Title Case Headings, emoji bullets, decorative bold |

The full bank, with the reasoning behind each tell and the false-positive traps, lives in [`references/ai-slop-tells.md`](references/ai-slop-tells.md).

## The Ogilvy pass

In 1982 David Ogilvy sent his agency a one-page memo called "How to Write". Its argument: woolly thinking produces woolly writing, and good writing is learned, not inherited. Five of his ten rules are things a skill can actually check in a text, and they run on every draft:

| Rule | What the skill does |
|---|---|
| Write the way you talk | Reads the draft aloud; fixes wherever it stumbles |
| Short words, sentences, paragraphs | Flags Latinate words with plain equivalents, sentences that lose the thread, fat paragraphs |
| Never use jargon | Flags invented abstractions (`operationalize`, `actionability`), corporate filler, and unexplained domain terms |
| Check your quotations | Flags every quote, stat, date, and attribution as unverified. Never invents or completes one. |
| Make the ask crystal clear | Checks whether a reader could state the next action after one pass |

The other five rules belong to you, not to a skill: read Roman-Raphaelson, sleep on it and read it aloud in the morning, get a colleague to improve it, and if you want action from one person, go ask them instead of writing. The skill surfaces the relevant one on a high-stakes draft rather than pretending to have run it.

That last rule is worth keeping: a writing tool telling you not to send the email is sometimes the correct output.

Full method, including the two-page rule and why jargon is a thinking problem rather than a vocabulary one, in [`references/ogilvy-method.md`](references/ogilvy-method.md).

## Severity, not a wall of nitpicks

Findings come back sorted **Critical / Moderate / Minor**, each with the offending line quoted, one line on why, and the specific fix. A buried ask is critical. A stray filler word is minor. Treating them the same is how audits get ignored.

It also names **what's working**, so you don't edit away your own good stuff on the next pass.

## It checks its own work

After editing, the skill runs its output against [`eval.md`](eval.md), a 35-check pass/fail list covering whether it invented anything, whether it flattened your voice, whether each pattern is actually gone, whether the Ogilvy rules hold, and whether it over-corrected. Any fail and it fixes the draft and re-checks before you see it.

## Install

### Claude Code

```bash
git clone https://github.com/ameenmo/humanize-skill.git ~/.claude/skills/humanize
```

Then `/humanize` in any session, or just say "humanize this".

For a single project instead of globally, clone into `.claude/skills/humanize/` in the project.

### Cursor, Codex, Windsurf, or any other agent

`SKILL.md` is plain markdown with no tool calls in it, so any agent that can read a file can follow it. Clone the repo anywhere and point your agent at it:

```bash
git clone https://github.com/ameenmo/humanize-skill.git
```

Then either paste `SKILL.md` into the conversation, or add a line to your `AGENTS.md` / `.cursorrules`:

```
When asked to humanize text or remove AI slop, follow ./humanize-skill/SKILL.md.
```

## Use

```
/humanize <text or file path> [--audit] [--preserve|--conform] [--style <path>] [--type email|social|longform] [--keep-em-dash]
```

| Flag | Effect |
|---|---|
| `--audit` | Report findings only. No edit. |
| `--preserve` | Force preserve mode even if a style file exists. |
| `--conform` | Force conform mode. |
| `--style <path>` | Load a voice profile from a specific file (implies `--conform`). |
| `--type` | Force the concision profile instead of inferring it. |
| `--keep-em-dash` | Leave em dashes alone. |

Examples:

```
/humanize draft.md
/humanize --audit "Our platform leverages cutting-edge AI to unlock unprecedented value."
/humanize --type social draft-caption.txt
```

## Give conform mode a voice

Preserve mode needs no setup: it works from your draft. Conform mode needs a target, and its built-in default is direct, concise, and no-em-dash. If that isn't the voice you want, drop a `HUMANIZE_STYLE.md` in your project root and the skill will use it instead (and switch to conform mode automatically):

```markdown
# Voice

- Warm and conversational. Second person. Contractions always.
- Em dashes are fine, I use them deliberately.
- Never open with a question.
- Sentences can run long if the rhythm earns it.
```

Resolution order: `--style <path>` → `HUMANIZE_STYLE.md` → a voice section in your `CLAUDE.md` / `AGENTS.md` / `.cursorrules` → the built-in default. See [`HUMANIZE_STYLE.example.md`](HUMANIZE_STYLE.example.md).

## What it won't do

It edits for readers, not for detectors. AI-detection tools are unreliable in both directions, and text tuned to beat them tends to read worse. This skill removes the patterns that make writing feel machine-made; it makes no claim about any detector's score, and on an audit it won't tell you whether AI wrote something. Detectors guess. Named patterns are evidence you can check yourself.

It won't verify your facts either. It flags quotes, statistics, and attributions in a **Needs your check** section and leaves them to you. It will never invent or complete a quote to fill a gap.

And it won't over-correct. One em dash or one rule-of-three isn't slop; the signal is the pile. Technical and legal copy is *supposed* to be plain and repetitive, and the skill leaves it that way. If a draft is already clean, it says so and stops.

## Files

| File | What it is |
|---|---|
| `SKILL.md` | The rules and the workflow. The whole skill runs from here. |
| `eval.md` | Pass/fail checks the skill runs on its own output. |
| `references/ai-slop-tells.md` | Extended bank of AI tells and why each one is a tell. |
| `references/ogilvy-method.md` | The Ogilvy method in full, and what a skill can't do. |
| `HUMANIZE_STYLE.example.md` | Template for a conform-mode voice profile. |

## Sources and credit

- **David Ogilvy**, "How to Write" (internal memo, Ogilvy & Mather, 1982). The rules are paraphrased here, not reproduced. Ogilvy's first rule points at *Writing That Works* by Kenneth Roman and Joel Raphaelson, which is the source text behind most of the memo.
- **[Nicolas Cole](https://x.com/Nicolascole77)** for the memo transcription and the commentary that turned it into something checkable ([July 2026](https://x.com/Nicolascole77/status/2072662998326415447)).
- **[petergyang/no-ai-slop](https://github.com/petergyang/no-ai-slop)** (MIT) for several sentence-shape patterns this skill was missing (colon reveals, faux-insight setups, fake-profound kickers, synonym cycling), for the self-check-against-an-eval-file approach, and for the severity-sorted audit format. Patterns were reimplemented here rather than copied, but the debt is real. Worth using on its own.
- The AI-tell bank also draws on Wikipedia's "Signs of AI writing", academic word-frequency studies of post-2022 published text (the PubMed "delve" spike being the best-known), and various editor and copywriter writeups.

## License

MIT. See [LICENSE](LICENSE).
