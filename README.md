# humanize

An [Agent Skill](https://code.claude.com/docs/en/skills) that strips AI-slop from a draft and rewrites it to read like a person wrote it.

It does two things:

1. **Audit** — scans the draft against a researched checklist of AI tells and quotes each hit back to you with a one-line reason.
2. **Rewrite** — removes the tells and rewrites in *your* voice, then shows a before/after diff for the worst offenders.

It works on blog posts, website copy, articles, tweets, captions, titles, product descriptions, and emails.

## What it catches

| Category | Examples |
|---|---|
| Lexical | delve, leverage, utilize, foster, robust, pivotal, tapestry, landscape, testament |
| Cliché | "stands as a testament to", "in today's fast-paced world", "it's worth noting" |
| Structural | "It's not just X, it's Y", compulsive rule-of-three, listicle bloat, participle tails |
| Formatting | em dashes, Title Case Headings, scattered boldface, emoji bullets |
| Tone | relentless positivity, no stance taken, marketing voice, engagement-bait closers |

The full bank, with the reasoning behind each tell, lives in [`references/ai-slop-tells.md`](references/ai-slop-tells.md).

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
/humanize <text or file path> [--audit] [--type email|social|longform] [--style <path>] [--keep-em-dash]
```

| Flag | Effect |
|---|---|
| `--audit` | Report the tells only. No rewrite. |
| `--type` | Force the concision profile instead of inferring it. |
| `--style <path>` | Load a voice profile from a specific file. |
| `--keep-em-dash` | Leave em dashes alone. |

Examples:

```
/humanize draft.md
/humanize --audit "Our platform leverages cutting-edge AI to unlock unprecedented value."
/humanize --type social draft-caption.txt
```

## Make it sound like you, not like me

The skill defaults to a direct, concise, no-em-dash voice. If that isn't yours, override it. Drop a `HUMANIZE_STYLE.md` in your project root and the skill will use it instead:

```markdown
# Voice

- Warm and conversational. Second person. Contractions always.
- Em dashes are fine, I use them deliberately.
- Never open with a question.
- Sentences can run long if the rhythm earns it.
```

Resolution order: `--style <path>` → `HUMANIZE_STYLE.md` → a voice section in your `CLAUDE.md` / `AGENTS.md` / `.cursorrules` → the built-in default. See [`HUMANIZE_STYLE.example.md`](HUMANIZE_STYLE.example.md).

## What it won't do

It rewrites for readers, not for detectors. AI-detection tools are unreliable in both directions, and text tuned to beat them tends to read worse. This skill removes the patterns that make writing feel machine-made; it makes no claim about any detector's score.

It also won't over-correct. One em dash or one rule-of-three isn't slop. The signal is the pile. Technical and legal copy is *supposed* to be plain and repetitive, and the skill leaves it that way. If a draft is already clean, it says so and stops.

## Sources

The tell bank is distilled from Wikipedia's "Signs of AI writing", academic word-frequency studies of post-2022 published text (the PubMed "delve" spike being the best-known), and editor and copywriter writeups.

## License

MIT. See [LICENSE](LICENSE).
