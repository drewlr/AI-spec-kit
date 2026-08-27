# AI spec kit

Guidance on building and shipping software with AI agents, written from doing it
rather than from thinking about it. Most of the examples come from two mobile
apps built this way with fast iterations. An agent does most of the work in this
repository with occasional human supervision, so expect some AI slop.

The documents are written as instructions to an agent. Point a session at them,
or copy the parts you want into your own rules file.

1. [Documents worth having](./01-documents.md). The files that make the next
   session better than the last one, what belongs in each, and which three get
   read at the start of every session.
2. [Before a mobile app goes live](./02-mobile-launch.md). A checklist for
   shipping to the App Store and Google Play.
3. [Graphics and visuals](./03-graphics-and-visuals.md). Where to find design
   worth learning from when you cannot afford a designer.

[lessons.md](./lessons.md) sits beside those three and is read differently.
It holds everything that went wrong once, numbered, and nobody reads it from the
top. The other documents cite a lesson by its number, and the two or three
paragraphs that matter on every project belong in your own `CLAUDE.md`, which
the first document above describes.

[examples](./examples/) holds a filled in version of every document in the first
list, all describing the same invented project, so you can see the shape rather
than only the description.

The repository also carries a writing style at
`.claude/skills/plain-writing/SKILL.md`, which an agent working here loads on
its own. It is one house style, so swap in your own if you have one.

## Guiding principles

**Parsimony.** A rule earns its place by changing what an agent does. Most of
the rules here started as something that went wrong once, and anything that has
never changed an outcome gets cut. A short list gets followed and a long one
gets skimmed.

**Speed of iteration.** Short rules kept next to the work beat a specification
written before the work started. The point of working this way is how quickly
you can try something, see it fail and change it, and heavy process takes that
away.

**Sparse but significant quality checks.** Put tests, code reading and human
sign off where being wrong is permanent or expensive, e.g. data with no second
copy, money, and advice about health or safety. Skip them where a mistake is
obvious and cheap to fix. Checking everything to the same depth costs more than
it saves, and it teaches everybody to ignore the checks.

**Rules meant to be bent.** Each rule says when it applies, so an agent can tell
when it does not apply. An agent that skips a rule should say so in the chat and
say why. A rule that gets bent often is either wrong or badly written, so fix it
or remove it.

## How to commit here

For an agent adding something to this repository.

- **Branch from the latest main, and fetch first.** Another agent may have
  landed something while you were reading. A change written against an old copy
  of a document deletes whatever arrived in between, and the deletion is easy to
  miss in a large diff.
- **Check whether the rule is already here.** Extend the rule that exists rather
  than adding a second one beside it that says nearly the same thing.
- **Write it in the format the documents use.** A heading that gives an
  instruction, a body saying when the rule applies and what to do, a line
  starting with Why, and one italic line of evidence. Leave the evidence line
  out when no real incident is behind the rule, rather than inventing one.
- **Say when the rule can be skipped.** A rule that fires every time gets read
  as background and stops changing what anybody does.
- **Follow the writing style** in `.claude/skills/plain-writing/SKILL.md`, in
  the commit message as much as in the document.
- **Write the commit message about the change, not about the work.** One subject
  line in sentence case with no full stop, then plain sentences saying what
  problem the change fixes. Do not list the files, because the diff lists them
  already.
- **One rule per commit**, so that a rule can be reverted on its own.
- **Open a pull request rather than pushing to main.** Say in the description
  what you verified and what you assumed.

## How this list is kept

It is a working list, not a finished document. It grows from real projects, at
the moment something is found missing.

When a piece of work turns out to need something that is not written here, it
goes in here as well as getting done. Tests and analytics both arrived that way.
Neither was in the plan, both were needed, and both were things the next project
would need too.

Keep it mostly general. An entry belongs here when it would be true of the next
app somebody builds this way. Anything specific to one project stays in that
project. The mobile launch list is the exception, and it says so.

Two apps feed it at the moment.

## The idea underneath all of it

An agent is fast, tireless, and confident whether or not it is right. You cannot
tell a correct answer from an invented one by reading it, because an agent
writes both the same way, so you have to get the proof from somewhere else.

Everything in these documents follows from that. Write things down so the agent
cannot forget them. Make it prove its claims instead of stating them. Keep the
checks cheap enough to run on every change. None of it is about better
prompting.

## Credits

The idea for the repository came from
[github/spec-kit](https://github.com/github/spec-kit), which seemed to me
somewhat over prescriptive and to need too much maintenance, and that works
against some of the benefits of fast agentic iterations. I might change my mind
with more experience.

The spec here is the shortest version of their idea that still changes what an
agent builds. They write three documents per feature, a directory of templates,
numbered requirements, ranked user stories and acceptance criteria in a formal
notation. This is one page with four headings, thrown away when the feature
ships.
