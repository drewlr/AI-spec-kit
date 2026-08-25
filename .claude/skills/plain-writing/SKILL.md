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
- Linear order. Subject first, then the verb, then the context the sentence needs. 'Some charts offer reference data next to your baby's data' not 'Where the app puts your baby against a published standard, such as the growth charts, it shows you where the number sits'. What must not lead is a description standing in for the subject, because the reader reaches the middle of the sentence before they know what it is about.
- A condition can lead, and usually should. 'If you are worried about yourself or your baby, you should speak to the people who look after you' not 'You should speak to the people who look after you if you are worried about yourself or your baby'. The condition says which case the sentence covers, so anybody it does not cover can stop reading at the comma, and the main clause after it still starts with its own subject.
- Merge sentences that share a subject and a condition. Two sentences that both start 'You should' are one sentence with 'and' in it.
- A section's first sentence stands alone. It cannot take its subject or its verb from the heading above it. 'Who decides about your baby' followed by 'You do.' leaves anybody who skipped the heading with nothing, and a heading is the part people skip. Write 'You decide about your baby' and let the heading repeat it.
- Name the subject rather than describing it. 'Statistics and metrics reported by the app' not 'Anything worked out from your own logs'. A description in the subject slot makes the reader work out what the sentence is about before they can start reading it.
- Do not compress a definition into an idiom. 'are calculations based on records that you logged' not 'is arithmetic on what you typed'. The short version is more fun to write and takes longer to understand.
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
- One source for the document, and a test that the copies match it. Where the same words appear in an app and on a website, render both from one file and check the rendered copies in the test suite. A generator that somebody has to remember to run is a generator nobody runs: a notice edited and deployed on 24 August 2026 left the website serving the previous day's wording for a day, and the repository's own README warned about exactly that, which is not the same as preventing it.

- Group by where a person goes, not by what the thing is called. Three
  paragraphs headed deletion, withdrawal and export become one sentence when all
  three happen in the same place.
- Use the plain verb for what happens, especially about deletion. 'it gets
  deleted all at once', not 'it all goes at once'.
