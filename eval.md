# humanize eval

Run this against your own output before you return it. Answer each check pass or fail. On any fail, fix the draft and run the checks again. Do this yourself; no separate evaluator agent needed.

Skip sections that don't apply (the mode checks, or the audit checks, depending on the run).

## Did you break anything

1. Is every claim, number, name, quote, and example still one the writer supplied? Nothing invented.
2. Are specific facts still specific? No detail smoothed into generic importance.
3. Is the core point intact and still the point?
4. Is the cutting proportional to the actual slop, with no compression that stripped character?

## Preserve mode

Skip if you ran in conform mode.

5. Would the writer recognize this as their own writing?
6. Are their vocabulary, cadence, bluntness, humor, uncertainty, and digressions still there?
7. Did you leave strong human sentences alone rather than tidying them for consistency?
8. Did you avoid imposing tone, stance, formality, or sentence-length targets? Those are conform-mode moves.
9. Are hedges, profanity, self-interruptions, and admissions preserved where they read as the writer's own?

## Conform mode

Skip if you ran in preserve mode.

10. Does the draft match the loaded profile on tone, person, formality, and length?
11. Did the profile's rules win over this skill's defaults wherever the two disagreed?

## Tells removed

12. Banned words, filler phrases, and inflated claims gone, unless quoted as examples?
13. Empty adverbs cut only where they added nothing, and kept where they carry emphasis, contrast, or real uncertainty?
14. Binary contrasts, negative listings, throat-clearing openers, rhetorical setups, and faux-insight setups gone?
15. Colon reveals, fake-strong verbs, superficial `-ing` analysis tails, and synonym cycling fixed?
16. Importance puffery and weasel attribution replaced with plain facts and named sources, or flagged for the user where no source exists?
17. Fake-profound kickers **deleted**, not rewritten into better metaphors and not rhythm-preserved?
18. Summary-recap endings cut, so the piece ends on a concrete point, takeaway, or next action?
19. Em dashes handled per the active rule (removed by default; kept under `--keep-em-dash` or a profile that permits them)?
20. Formatting slop gone: emoji headings, decorative mid-sentence bold, headers over tiny sections, bullets that should be prose?
21. False ranges, stock challenge sections, chatbot wrappers, knowledge-limit padding, unraised objections, heading echoes, and irrelevant old-version narration gone?

## The Ogilvy pass

22. Does it read aloud cleanly, with no sentence you stumble over or run out of breath in?
23. Is every long Latinate word there because the plain one would lose meaning?
24. Are long sentences and fat paragraphs split where they were hard to follow, and left alone where they read fine?
25. Is jargon cut, defined once, or named as a thinking problem rather than swapped for a different fancy word?
26. Are exact identifiers, commands, paths, types, errors, product names, and terms of art unchanged?
27. Is each load-bearing quotation, statistic, attribution, or unsupported factual claim sourced or flagged in **Needs your check**?
28. Could the reader state the next action, or the takeaway, after one pass? If the draft has no ask, did you say so?
29. If the piece needs one person to act and a conversation would work better, did you say that?
30. Is the two-page reasoning applied as a flag on bloat, not as a cut to someone's longform draft?

## Read it back

31. Does it avoid robotic symmetry: repeated sentence shapes, uniform paragraph length, topic→support→restate?
32. Does varied rhythm survive, without stacked punchy fragments used as a drumbeat?
33. Does active voice make the actor clear where the actor matters, without forced human subjects?
34. Would this sound natural read aloud to a sharp colleague?
35. Does the output include the full edited draft plus a short **What changed** section?
36. Do the notes lead with the result, use plain words, and omit tool narration?

## Audit runs

Skip unless `--audit`.

37. Is every finding a named pattern with the offending line quoted and a short fix?
38. Did you avoid rewriting the draft, scoring it, and claiming AI authorship either way?
