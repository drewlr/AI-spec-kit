# Documents worth having

An agent starts every session knowing nothing about your project except what it
can read. These are the files that make the next session start where the last
one finished.

Keep them in the repository, next to the code, so they are updated in the same
commit as the thing they describe. A document in a separate place goes stale in
a week.

## The rules file

Usually `CLAUDE.md` or `AGENTS.md` at the root. The agent reads it before
anything else, so it is the only document that changes behaviour rather than
informing it.

Put in it the things you are tired of repeating, and the things that cost money
or are hard to undo.

- What the agent may do without asking, and what it must ask about.
- Anything that costs money when it runs, and the cheaper way to do the same
  thing.
- Facts that are true of this project and surprising, e.g. a tool everyone
  assumes works that does not work here.
- The commands that check the work.
- Which documents are drafts nobody has reviewed.

Keep it short. A rules file that runs to five pages stops being read carefully,
by people as well as by agents.

## The architecture description

One page and one diagram. What runs where, what talks to what, and which lines
carry personal data.

The diagram is worth more than the page. Draw it in Mermaid so it lives in the
markdown file and renders on GitHub without a separate image to keep in step.

The questions it should answer without anybody having to read code: does data
leave the device or the server, what happens when the network is gone, where is
the only copy of anything, and which parts are optional.

## The decision log

Short entries, one per real decision: what was chosen, what else was considered,
and why. Two paragraphs each.

This is the document that pays for itself fastest with an agent, because a new
session will otherwise propose the thing you already rejected, and it will
propose it convincingly. An entry saying "we chose the store's own payments
because both stores require it" ends that conversation in one line.

Write an entry when the answer took real thought, and skip it when it did not.

## The build and release runbook

How to build the thing, how to get it to a person, and what each route costs.

Include the routes that do not work and why, because somebody will try them.
Include the things that need a human with an account, so nobody wastes an hour
finding out that a step needs a credential they do not have.

## The data map, if you hold anything personal

What you collect, why, where it goes, how long you keep it, and how somebody
gets rid of it.

You need this to fill in the store privacy forms, and you need it to answer a
user who asks. If you handle health data, data about children, or anything
similar, you probably also need a formal impact assessment, and a lawyer should
read it. An agent can draft it and cannot sign it off.

## The list of what is not built

The most undervalued document. Everything that looks finished and is not: the
button that does nothing yet, the screen that fakes its result, the limit that
is not enforced.

Without it, a later session reads the code, sees a paywall, and assumes payments
work. In the app these notes come from, the paywall showed prices, had a confirm
button, and charged nobody. Anybody could have believed it was done.

Keep it honest and keep it current. It is also your launch checklist.

## The test notes

What is tested, what is not, and why not. One paragraph is enough.

An agent asked to change something will treat a passing test suite as proof it
did no harm. If half the app has no tests, say which half.

## The domain source of truth

If your app has content, rules, prices, or thresholds that people argue about,
keep them in one file that is not code, and generate the code from it.

In the app above, several hundred articles lived in a spreadsheet that a person
edited, and a script turned it into a data file. An agent could change a
single article's category and rebuild without touching any code, and the edit
was reviewable by somebody who does not read code at all.

## What not to bother with

Do not write a document that repeats what the code says. It will be wrong within
a month and nobody will notice.

Do not write a long specification up front and expect an agent to follow it.
Agents follow short rules that are close to the work far better than long
documents written before the work started.
