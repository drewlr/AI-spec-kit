# AI spec kit

Guidance on building and shipping software with AI agents, written from doing it
rather than from thinking about it. Most of the examples come from two mobile
apps built this way with fast iterations. An agent does most of the work in this
repository under occasional human supervision, so expect some AI slop.

## What to read

The documents are written as instructions to an agent. Point a session at them,
or copy the parts you want into your own project.

Two of them apply to any project.

1. [The approach](./01-approach.md). How an agent should manage the work, in a
   page. It says nothing about how to build software, because an agent already
   knows that.
2. [Documents worth having](./02-documents.md). The files that make the next
   session start where the last one finished, and what belongs in each.

The rest are specific, and each one says at the top what it is specific to. Skip
whichever does not describe what you are building.

3. [Before a mobile app goes live](./03-mobile-launch.md). Specific to shipping
   to the App Store and Google Play.
4. [Graphics and visuals](./04-graphics-and-visuals.md). Specific to anything
   with a designed surface, and to not having a designer.
5. [Lessons](./lessons.md). Specific findings from real projects, grouped by
   subject, with each group saying what it applies to. Nobody reads it from the
   top and none of it is required.

## What to do with it

Copy [the approach](./01-approach.md) into your project when you start it, and
point at your copy from your rules file. A project should not depend on this
repository being checked out beside it.

Then write your rules file, which is `CLAUDE.md` or `AGENTS.md` and the only
document an agent loads without being told.
[examples/CLAUDE.example.md](./examples/CLAUDE.example.md) shows one filled in,
and it is the example worth reading in full.

[examples](./examples/) holds a filled in version of every document in the
second list, all describing the same invented project, so that you can see the
shape rather than only the description.

The repository also carries a writing style at
`.claude/skills/plain-writing/SKILL.md`, which an agent working here loads on
its own. It is one house style, so swap in your own if you have one.

## The idea underneath all of it

An agent is fast, tireless, and confident whether or not it is right. You cannot
tell a correct answer from an invented one by reading it, because an agent
writes both the same way, so the proof has to come from somewhere else.

Everything in these documents follows from that. Write things down so that the
agent cannot forget them. Make it prove its claims rather than state them. Keep
the checks cheap enough to run on every change. None of it is about better
prompting.

## Guiding principles

**Parsimony.** A rule earns its keep by changing what an agent does. Most of the
rules here started as something that went wrong once, and anything that has
never changed an outcome gets cut. A short list gets followed and a long one
gets skimmed.

**Speed of iteration.** Short rules kept next to the work beat a specification
written before the work started. The point of working this way is how quickly
you can try something, see it fail, and change it, and heavy process takes that
away.

**Sparse but significant quality checks.** Put tests, code reading and human
sign off where being wrong is permanent or expensive, e.g. data with no second
copy, money, and advice about health or safety. Skip them where a mistake is
obvious and cheap to fix. Checking everything to the same depth costs more than
it saves, and it teaches everybody to ignore the checks.

**Rules meant to be bent.** Each rule says when it applies, so that an agent can
tell when it does not apply. An agent that skips a rule should say so in the
chat and say why. A rule that gets bent often is either wrong or badly written,
so fix it or remove it.

**A check beats an instruction.** Where a rule can be a command that fails,
write the command. Twenty em dashes reached these documents in one week while
the style file banning them sat in this repository, pointed at from the
approach.

## How to commit here

For an agent adding something to this repository.

- **Branch from the latest main, and fetch first.** Another agent may have
  landed something while you were reading. A change written against an old copy
  of a document deletes whatever arrived in between, and the deletion is easy to
  miss in a large diff.
- **Check whether the rule is already here.** Extend the rule that exists rather
  than adding a second one beside it that says nearly the same thing. Eight
  rules in the approach once said the same thing, because each session that hit
  the fault wrote a new rule instead of adding its evidence to the old one.
- **Write it in the format the documents use.** A heading that gives an
  instruction, a body saying when the rule applies and what to do, a line
  starting with Why, and one italic line of evidence. Leave the evidence line
  out when no real incident sits behind the rule, rather than inventing one.
- **Say when the rule can be skipped.** A rule that fires every time gets read
  as background and stops changing what anybody does.
- **Ask whether it belongs here at all.** A finding about one app belongs in
  that app, or in [lessons](./lessons.md) if the next project would hit it too.
  The approach is for rules about managing the work.
- **Follow the writing style** in `.claude/skills/plain-writing/SKILL.md`, in
  the commit message as much as in the document.
- **Write the commit message about the change, rather than about the work.** One
  subject line in sentence case with no full stop, then plain sentences saying
  what problem the change fixes. Do not list the files, because the diff lists
  them already.
- **One rule per commit**, so that somebody can revert a rule on its own.
- **Open a pull request rather than pushing to main.** Say in the description
  what you verified and what you assumed.

## How this list is kept

It is a working list rather than a finished document. It grows from real
projects, at the moment somebody finds something missing.

When a piece of work turns out to need something that is not written here, it
goes in here as well as getting done. Tests and analytics both arrived that way.
Neither was in the plan, both were needed, and the next project would need both
too.

It also shrinks, and shrinking is the half that gets skipped. The approach
reached 66 rules over 1579 lines before somebody cut it back to a page, and most
of what went was one app's engineering findings written as though they were
general rules.

Keep it mostly general. An entry belongs here when it would be true of the next
app somebody builds this way. Anything specific to one project stays in that
project. The specific documents are marked as specific and say what they are
specific to.

Two apps feed it at the moment.

## Credits

The idea for the repository came from
[github/spec-kit](https://github.com/github/spec-kit), which seemed to me
somewhat over prescriptive and to need too much maintenance, and that works
against some of the benefits of fast agentic iterations. I might change my mind
with more experience.

The spec here is the shortest version of their idea that still changes what an
agent builds. They write three documents per feature, a directory of templates,
numbered requirements, ranked user stories, and acceptance criteria in a formal
notation. This is one page with four headings, collapsed to a line when the
feature ships.
