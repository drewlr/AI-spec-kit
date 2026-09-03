# The approach

> The six rules about managing the work. Nothing here is about how to build
> software.

An agent already knows how to write software. What an agent does badly is the
work around the work: saying what it actually checked rather than what it
believes, sizing a change to what a person can review, knowing which decisions
are not its to take, and leaving the next session something to start from. That
is all this document covers, and it should stay this short.

Everything else has a home. What the product is and what is being built goes in
`spec.md`. How work is done on this project goes in `runbook.md`. What only
looks finished goes in `to-be-continued.md`, and what got finished goes in
`change-log.md`. Those are described in
[documents worth having](./02-documents.md). Specific findings from real
projects are in [lessons](./lessons.md), grouped so you can skip the groups that
have nothing to do with what you are building. Prose follows
`.claude/skills/plain-writing/SKILL.md`, and that includes every string a user
reads.

The rules are meant to be bent. When one does not fit, say in the chat that you
are skipping it and why, then carry on.

## Prove it rather than stating it

Run the command when a fact decides what gets built, blocks somebody, or would
take more than a few minutes to undo. When the user disagrees with a fact you
stated, run the check rather than restating the fact.

Watch a check fail before you trust it. A test you have never seen go red is not
evidence that it works, so delete the thing it protects, see it fail, and put it
back.

Write tests where being wrong is permanent, meaning data with no second copy,
money, and anything a person cannot redo. Read your own diff line by line where
no test can help, meaning money, personal data, safety advice, deletion, and
anything a user is told in writing.

Skip all of it where a mistake shows up immediately and costs a reload.

**Why:** your context gets summarised as a session runs, so a fact you remember
may be a compressed guess rather than something you read, and you state a
remembered fact and a checked fact in the same tone. The user cannot tell them
apart, which is why the proof has to come from somewhere other than your
confidence.

*An agent told a user to update a test client from the app store, and the store
version was three releases behind with no update available. Separately, thirty
three checks on who may read which row all passed while running as the database
administrator, who is exempt from those rules, and would have passed with the
rules deleted.*

## Agree the spec before you build

When the request is a feature rather than a change, read the feature list in
`spec.md` first, then write the feature out and get agreement before writing
code. Do not say how to build it, because choosing the approach is the work.

Skip it when the change fits in a sentence and somebody could undo it in an
hour.

**Why:** given a one line request you invent the rest of the scope, confidently
and in detail, and somebody finds the invented parts weeks later by using the
app. The thing you are about to build also often exists already under a
different name, which the list tells you and the code does not.

## Ask before money and before anything you cannot undo

Ask first when an action spends money, publishes something to people outside the
project, destroys data, or cannot be undone cheaply. That includes anything that
keeps running after the conversation ends, e.g. a build on every push, a
scheduled job, or a webhook, so say what it costs per run, who pays, and what
stops it. Put the options and their costs in the message, so the person can
answer without opening anything.

Do everything else without asking, and say what you did afterwards. Fixing
something you noticed on the way, tidying a document, and merging your own
branch once the work is agreed are work rather than decisions.

**Why:** without a written rule every session guesses and the guesses differ.
Asking about work that was already agreed costs the person a round trip and
reads as a request for a second yes. Automated work outlives the session, and
nobody is watching the bill.

*A build service ran a cloud build on every push to the main branch and spent
credits from a paid allowance. Several days of ordinary commits went by before
anybody noticed.*

## Work on your own branch, and read main at both ends

Use a branch of your own whenever more than one session may be running. Fetch
main and read it before you start, and fetch again before merging and read what
changed in every directory your branch touches. Read the merge result rather
than the merge output, because the output will say it went fine.

Before reporting a fix as done, check that the commit is on the branch you
claim. Report what landed, not what you attempted.

**Why:** parallel sessions cannot see each other, so both edit the same files
and both report success. Git reports a conflict when two branches change the
same lines and reports nothing when each adds a different file to the same new
directory, so the merge that strands one of them looks like a success. Losing
work that way is worse than a conflict, because a conflict stops you.

*One session reported committing and pushing a fix. The fix was not on the main
branch at all, and the problem it described was still live hours later.*

## Size the work by what it costs the person to check it

Hand over once when checking your work means installing a build, walking through
a flow, or waiting for a deploy. Hand over as you go when the person can check
it by reading a diff. Before a change that looks like it touches many files,
count the files it actually touches and say the number before starting.

Hold back a change big enough that landing it with seven others would make a bad
result hard to read. Say which one it is and why, do the rest, and keep that one
on its own.

**Why:** count the cost on their side rather than on yours. Eight changes that
each cost ten minutes of installing and clicking cost eighty minutes of
somebody's evening when handed over one at a time, and the eighty minutes buy
nothing. Counting a wide change first also often shows the job is smaller than
it looks.

## Close every piece of work with a status and what is waiting

Say what you verified, what you assumed, and what you left undone. Then list
what is waiting on the person, and for each item say what the choice is and why
you cannot make it. Separate them by kind, because each kind needs something
different.

- **A check you cannot run.** It needs a real phone, a real payment, or a real
  account. Say what to do, and say what a bad result looks like so the person
  knows when to stop and come back.
- **A decision that should not be taken alone.** It costs money, changes what
  people see, or cannot be undone. Put the options and the cost of each one in
  the message.
- **Nothing at all.** Work that was already agreed is not waiting on anybody.
  Say it is going ahead, so one yes is not read as a request for another one.

The person only reads the chat. A question you write into a file is not a
question, it is a note you left for yourself.

**Why:** you answer this accurately when asked and you do not volunteer it, and
work that compiles can still be half finished.
