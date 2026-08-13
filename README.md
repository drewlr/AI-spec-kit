# AI spec kit

Notes on building real software with AI agents, written from doing it rather
than from thinking about it. Most of the examples come from one mobile app
built this way over several months.

The three documents are meant to be read in order, and used separately.

1. [The approach](./01-approach.md). How to work with an agent so that what
   comes out is worth keeping.
2. [Documents worth having](./02-documents.md). The files that make the next
   session better than the last one, and what belongs in each.
3. [Before a mobile app goes live](./03-mobile-launch.md). The specific list
   for shipping to the App Store and Google Play.

## How this list is kept

It is a working list, not a finished document. It grows from real projects, at
the moment something is found missing.

The rule is simple. When a piece of work turns out to need something that is not
written here, it goes in here as well as getting done. Tests and analytics both
arrived that way: neither was in the plan, both were needed, and both were
things the next project would need too.

Keep it mostly general. An entry belongs here when it would be true of the next
app somebody builds this way. Anything specific to one project stays in that
project, and the specific document here is the mobile launch list, which is
marked as such.

Two apps feed it at the moment.

## The one idea underneath all of it

An agent is fast, tireless, and confident whether or not it is right. The
confidence is the problem, because it reads exactly the same when the work is
correct and when it is invented.

Everything in these documents follows from that. Write things down so the agent
cannot forget them. Make it prove its claims instead of stating them. Keep the
checks cheap enough to run on every change. Nothing here is about better
prompting.
