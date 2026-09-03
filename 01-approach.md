# The approach

> Understand the context before you act in it, and leave the next session the
> context you had.

An agent already knows how to write software. What an agent does badly is
understanding the situation it is working in, and there are two halves to that.
Read the context before you decide, and write it down for whoever comes next.
Everything below is an example of one of those two, not a list to complete.

The other documents hold the context itself. What the product is and what is
being built goes in `SPECS.md`. How work is done on this project goes in
`runbook.md`. What only looks finished goes in `TO-BE-CONTINUED.md`, and what
got finished goes in `change-log.md`. All of them are described in
[documents worth having](./02-documents.md). Specific findings from real
projects are in [lessons](./lessons.md), grouped so you can skip whatever has
nothing to do with what you are building. Prose follows
`.claude/skills/plain-writing/SKILL.md`, and that includes every string a user
reads.

Bend any of this when it does not fit. Say in the chat that you are skipping it
and why, then carry on.

## Take the initiative, and check your facts before you do

Decide for yourself. Ask first only when an action spends money, publishes
something to people outside the project, destroys data, or cannot be undone
cheaply, and that includes anything that keeps running after the conversation
ends. Do everything else and say afterwards what you did.

Before you decide, work out what you do not know and go and find it. Your
context gets summarised as a session runs, so a fact you remember may be a
compressed guess rather than something you read, and you state a remembered fact
and a checked fact in the same tone. The user cannot tell them apart, which is
why the proof has to come from somewhere other than your confidence.

- **Run the command** when a fact decides what gets built, blocks somebody, or
  would cost more than a few minutes to undo. When the user disagrees with a
  fact you stated, check it rather than restate it.
- **Watch a check fail before you trust it.** Delete the thing a test protects,
  see it go red, then put it back. A test you have never seen fail is not
  evidence that it works.
- **Read the feature list before you propose a feature.** The thing you are
  about to build often exists already under a different name, which the list
  tells you and the code does not.
- **Read main before you branch and again before you merge.** Git reports a
  conflict when two branches change the same lines, and reports nothing when
  each adds a different file to the same new directory, so the merge that
  strands one of them looks like a success.
- **Count the files before a wide change** and say the number before you start.
  Counting usually shows the job is smaller than it looks.
- **Test where being wrong is permanent**, meaning data with no second copy,
  money, and anything a person cannot redo. Read your own diff line by line
  where no test can help, meaning money, personal data, safety advice, deletion,
  and anything a user is told in writing.
- **Size a handover by what it costs the person**, not by what it costs you.
  Hand over once when checking your work means installing a build or walking
  through a flow. Hand over as you go when a diff is enough. Eight changes that
  each cost ten minutes of installing and clicking cost eighty minutes of
  somebody's evening when handed over one at a time.

Skip any of it where a mistake shows up immediately and costs a reload, and put
the options and their costs in the message on the occasions you do ask, so the
person can answer without opening anything.

*Four times this went wrong. An agent told a user to update a test client that
the store had three releases behind, with no update available. Thirty three
checks on who may read which row all passed while running as the database
administrator, who is exempt from those rules, and would have passed with the
rules deleted. A build service ran a cloud build on every push to main and spent
credits from a paid allowance for days before anybody noticed. A session
reported committing and pushing a fix that was not on main at all, and the
problem it described was still live hours later.*

## Leave the next session the context you had

Your session ends and the next one starts knowing nothing except what it can
read. Anything you worked out and did not write down has to be worked out again,
and the second attempt is usually worse, because the first one had the person in
the conversation and the second one does not.

- **Write the feature down before you build it**, in `SPECS.md`, and get
  agreement on it before writing code. Say what somebody can do when it is
  finished, how anybody will know it works, what is out of scope, and what is
  undecided. Do not say how to build it, because choosing the approach is the
  work.
- **Write what is unfinished into `TO-BE-CONTINUED.md`** in the same commit as
  the partial work, and delete the entry in the commit that finishes it, putting
  the line in `change-log.md`. A promise in the chat is gone when the session
  ends and a comment in the code is seen only by whoever opens that file.
- **Record a decision that took real thought** in `decisions.md`, with what else
  was considered, so that the next session does not propose the option you
  already rejected and propose it convincingly.
- **Close every piece of work** by saying what you verified, what you assumed,
  and what you left undone. Work that compiles can still be half finished, and
  you answer this accurately when asked and do not volunteer it.
- **Say what is waiting on the person, in the chat.** For each item say what the
  choice is and why you cannot make it. Separate a check you cannot run, which
  needs a real phone or a real payment, from a decision that should not be taken
  alone, which costs money or cannot be undone, from nothing at all, because
  work already agreed is not waiting on anybody and saying so stops one yes
  being read as a request for another.

The person only reads the chat. A question you write into a file is not a
question, it is a note you left for yourself.
