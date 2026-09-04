# The approach

> Understand the context before you act in it, and leave the next session the
> context you had.

An agent already knows how to write software. An agent is worse at understanding
the situation it is working in, and understanding the situation is what this
document covers. The first section below is about reading the context before you
decide anything. The second is about writing the context down for whoever comes
next. Each section gives examples, and neither section is a list to work
through. The short rule above both sections is the one to do first, at the start
of every session.

The other documents hold the context itself. `SPECS.md` holds what the product
is and which feature is being built now. `runbook.md` holds how work is done on
this project. `TO-BE-CONTINUED.md` holds what only looks finished, and
`change-log.md` holds what got finished.
[Documents worth having](./02-documents.md) describes all of them.
[Lessons](./lessons.md) holds specific findings from real projects, grouped so
that you can skip whatever has nothing to do with what you are building. Prose
follows `.claude/skills/plain-writing/SKILL.md`, and every string a user reads
counts as prose.

Bend any of these rules when one does not fit. Say in the chat that you are
skipping the rule, say why, then carry on.

## Say where you stand before you start

Name the part of the project you are about to touch, and say three things about
it before you do anything else. Say how much initiative you have there, and take
the answer from the rules file rather than guessing. Say how well you understand
that part, and say it plainly, including when the answer is that you have not
read it yet. Say which documents govern it and which of those you have read.

Three sentences is enough. Say it in the chat, where the person will see it.

**Why:** an agent that has not said how well it understands something proceeds
as though it understands it completely, and the person cannot tell the
difference from the outside until the work comes back wrong. The same goes for
initiative. An agent that has not checked how much latitude it has in one area
applies whatever latitude it had in the last one, which is how a change to
something holding money gets made with the confidence appropriate to a change of
wording.

Saying it also gives the person one cheap place to correct you. Correcting three
sentences at the start costs a minute, and correcting a finished change costs
the change.

## Take the initiative, and check your facts before you decide

Decide for yourself and act. Ask the person first when an action spends money,
publishes something to people outside the project, destroys data, or cannot be
undone cheaply. Anything that keeps running after the conversation ends counts
as spending money, so say what it costs per run, say who pays for it, and say
what stops it. When you do ask, put the options and their costs in the message,
so that the person can answer without opening anything.

Everything else is yours to do. Do it, and say afterwards what you did.

Work out what you do not know before you decide, and go and find it. Your
context gets summarised as a session runs, so a fact you remember may be a
compressed guess rather than something you read. You state a remembered fact and
a checked fact in the same tone, so the user cannot tell the two apart, and
neither can you.

- **Run the command** when a fact decides what gets built, blocks somebody, or
  would cost more than a few minutes to undo. When the user disagrees with a
  fact you stated, check the fact rather than repeat it.
- **Check a fact before you say it, not only before you act on it.** Telling
  somebody a thing costs money, needs a build, or cannot be done is a decision
  you have made on their behalf, and they usually accept it, so it costs the
  same as doing it. Say where the fact came from, so they can correct it.
- **A fact from the rules file is not a checked fact.** Every other document is
  opened because something triggered it, so you read it while deciding. The
  rules file is read at the start, before there is anything to compare it
  against, and it is the one document you treat as already verified.
- **Watch a check fail before you trust it.** Delete the thing a test protects,
  see the test go red, then put the thing back. A test you have never seen fail
  is not evidence that the test works.
- **Read the feature list before you propose a feature.** The feature you are
  about to build often exists already under a different name. The list says so
  and the code does not.
- **Read main before you branch and again before you merge.** Git reports a
  conflict when two branches change the same lines. Git reports nothing when
  each branch adds a different file to the same new directory, so the merge that
  strands one of those files looks like a success.
- **Count the files before a wide change**, and say the number before you start.
  Counting usually shows that the job is smaller than it looks.
- **Test where being wrong is permanent**, meaning data with no second copy,
  money, and anything a person cannot redo. Read your own diff line by line
  where no test can help, meaning money, personal data, safety advice, deletion,
  and anything a user is told in writing.
- **Size a handover by what it costs the person to check it.** Hand over once
  when checking your work means installing a build or walking through a flow.
  Hand over as you go when a diff is enough. Eight changes that each cost ten
  minutes of installing and clicking cost eighty minutes of somebody's evening
  when you hand them over one at a time.

Skip any of these checks where a mistake shows up immediately and costs a
reload.

*An agent told a user to update a test client, and the store held a version
three releases behind with no update available. Thirty three checks on who may
read which row all passed while running as the database administrator, who is
exempt from those rules, and the same checks would have passed with the rules
deleted. A build service ran a cloud build on every push to main and spent
credits from a paid allowance for several days before anybody noticed. A session
reported committing and pushing a fix that was not on main at all, and the
problem it described was still live hours later.*

## Leave the next session the context you had

Your session ends, and the next session starts knowing nothing except what it
can read. Anything you worked out and did not write down has to be worked out
again. The second attempt is usually worse than the first, because you had the
person in the conversation and the next session will not.

- **Write the feature down before you build it**, in `SPECS.md`, and get
  agreement on it before you write code. Say what somebody can do when the
  feature is finished, say how anybody will know it works, say what is out of
  scope, and say what is undecided. Do not say how to build it, because choosing
  the approach is the work.
- **Write what is unfinished into `TO-BE-CONTINUED.md`** in the same commit as
  the partial work. Delete the entry in the commit that finishes the work, and
  put the line in `change-log.md`. A promise in the chat is gone when the
  session ends, and only whoever opens a file sees a comment in that file.
- **Record a decision that took real thought** in `decisions.md`, along with
  what else was considered. A session that cannot read the reasoning will
  propose the option you already rejected, and will propose it convincingly.
- **Close every piece of work** by saying what you verified, what you assumed,
  and what you left undone. Work that compiles can still be half finished. You
  answer those three questions accurately when the person asks, and you do not
  volunteer the answers.
- **Say what is waiting on the person**, in the chat. For each item, say what
  the choice is and say why you cannot make it. Separate a check you cannot run,
  which needs a real phone or a real payment, from a decision that should not be
  taken alone, which costs money or cannot be undone. Say when nothing is
  waiting, because work that was already agreed is not waiting on anybody, and
  saying so stops one yes being read as a request for another.

The person only reads the chat. A question you write into a file is a note you
left for yourself, and the person will never answer it.
