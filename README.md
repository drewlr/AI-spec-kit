# AI spec kit

Guidance on building and shipping software with AI agents, written from doing it
rather than from thinking about it. Most of the examples come from two mobile
apps built this way with fast iterations. An agent does most of the work in this
repository with occasional human supervision, so expect some AI slop.

The three documents are written as instructions to an agent. Point a session at
them, or copy the parts you want into your own rules file.

1. [The approach](./01-approach.md). Rules for how an agent should work.
2. [Documents worth having](./02-documents.md). The files that make the next
   session better than the last one, and what belongs in each.
3. [Before a mobile app goes live](./03-mobile-launch.md). A checklist for
   shipping to the App Store and Google Play.

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
