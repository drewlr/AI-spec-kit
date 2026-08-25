---
name: plain-writing
description: >-
  Write and edit prose in this repository's plain style: simple everyday words,
  complete sentences, no dashes, no jargon, no analogies, no filler, and full
  clear explanations. Use this whenever you draft or revise any prose, such as
  documents, README files, research notes, proposals, summaries, and commit and
  PR descriptions. Also use it whenever you are asked to simplify, clean up,
  tighten, reword, or make writing clearer or easier to read. Default to this
  style unless a different one is asked for. Do not apply it to code itself,
  only to the words around it.
---

# Plain writing

One house style, kept here as a snapshot so an agent working in this repository
loads it without being told. Swap in your own if you have one, and expect this
copy to drift from wherever you took it from.

Apply to all prose written for a person. Goal: text anyone can read once and understand.

## Words and tone
- Simple everyday words. 'use the cache to make queries faster' not 'leverage the cache for a robust query experience'. Avoid AI-overused words: delve, tapestry, landscape, robust, leverage, reach for, spearheading.
- No jargon. Use an established technical term only when it's the most precise word, and define it briefly. If it isn't in a dictionary, don't use it.
- No puffery. Cut: really, real, matters, worth, carries weight, boasts, a testament to, pivotal, renowned, quietly. State the point or cut the sentence.
- Repeat the word instead of swapping in a synonym.
- Contractions are fine.
- Don't invent hyphenated adjectives. Common ones like 'well-crafted' are fine; a made-up compound is not.
- Keep it boring, descriptive, literal. No catchy phrase, slogan, clever label, or metaphorical summary, including in headings and topic sentences. 'Applicable legal constraints' not 'Legal requirements as a floor'.

## Sentences and paragraphs
- Complete sentences, each with a subject and verb. No fragments. Don't stitch unrelated ideas together with colons or semicolons. Do join related ideas with 'and', 'because', or 'so'.
- Explain fully. Plain isn't terse. Expand a cramped sentence so each point gets its own sentence.
- Topic sentence first, then support. Introduce more support with 'For example', 'Moreover', or 'Or'.
- Two clauses per sentence at most. Three or more, split into sentences, or use bullets for a list. Give one example with 'e.g.', never three example sentences in a row.
- Prefer longer explanatory sentences over short punchy ones. End a sentence because the thought is complete, not for effect.
- Be precise. Name the specific thing that changes. 'you can measure whether each change helped, so you keep or revert it' not 'improvement stops being guesswork'.

## Punctuation and formatting
- No dashes, em or en, including in ranges. Use a period, 'and', or 'to' for ranges. No middle dot separators.
- Colon only to introduce a list, never to join clauses or set up a point.
- Straight quotes, not curly.
- Plain formatting. Sentence case headings. Bold only to name the subject a list item explains, not as decoration.

## Patterns to avoid
- No action verbs for inanimate subjects; make a person the actor. 'You can search the logs once the job finishes' not 'the logs become searchable records'. ('the paper argues' is fine.)
- No analogies, metaphors, or imagery. Describe the literal thing.
- No 'not just X, it is Y' or 'not X, but Y'. State what it is.
- No filler like 'it is worth noting that'. Watch for an '-ing' tail that fakes analysis; cut it or give the plain reason.
- Don't stack rhetorical questions. State the problem directly.
- No dramatic pivot: don't set up a point then undercut it.
- Don't attribute a claim to no one ('experts say', 'studies show'). Name the source or cut the claim.
- No vague demonstratives (This, That, These, Those pointing at a whole idea) or vague summary nouns (the result, the outcome, the point). Name the thing. Never open a sentence with a demonstrative pronoun.
- Don't open with a count ('Two cautions', 'Three things to keep in mind'). State the first point, or use a bullet list if the count is useful.

## Documents somebody has to act on

For a notice, a policy, a set of terms, or anything else a reader has to find an
answer in rather than read from the top.

- A heading names its own subject and asks the reader's question. 'Where is data
  kept', not 'Where it is kept'. A pronoun in a heading makes somebody scrolling
  read the heading above it to find out what it points at, and somebody scrolling
  is who headings are for.
- Every item in a list of what a person can do is a complete sentence with that
  person as the subject. 'You can request a copy of all your data at any time,
  through Settings', not 'A copy of everything, which you can take yourself'. A
  list of noun phrases reads as an index of rights rather than as things
  somebody can go and do.
- The condition goes in the sentence that grants the right, never in a later
  paragraph. 'You can turn off online sync in Settings, provided that your
  household is empty.' A right granted in one place and qualified in another is
  read as unqualified.
- Say what happens to the thing, not which control does it. A privacy notice
  says a copy is kept online and can be turned off; it does not say which screen
  carries the tick. Interface detail goes stale on the next release and it is
  not what the reader came for.
- Give a reason only where it changes what somebody would do. Explaining why
  each rule exists doubles the length and trains the reader to skip.
- Group by where a person goes, not by what the thing is called. Three
  paragraphs headed deletion, withdrawal and export become one sentence when all
  three happen in the same place.
- Use the plain verb for what happens, especially about deletion. 'it gets
  deleted all at once', not 'it all goes at once'.
