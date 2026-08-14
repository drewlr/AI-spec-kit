# The approach

## Make the agent prove things, not state them

An agent will tell you a version number, a limit, or an API's behaviour in the
same tone whether it read it or remembered it. Remembered facts go stale, and
some were never right.

The rule is that any claim which decides what you build has to come from a
command, not from the agent's memory. Fetching a version takes one second.
Being wrong about it costs a morning.

A real example. An agent said an app could not run in a test client because the
project needed a newer version of it, and told the user to update from the app
store. The user pushed back, saying they thought the store had an older one.
One command settled it: the store had a version three releases behind, and no
update existed. The advice had been useless, and the agent had been sure.

What to ask for:

- "Check that and show me the output" whenever a number or a limit decides
  something.
- "Run it" rather than "does this work".
- "What did you actually verify, and what are you assuming" at the end of a
  piece of work.

## Distrust documents the agent wrote

An agent will write a document, and then, three hours later, quote that document
back to you as though it were evidence. It is not evidence. It is the same
guess, now with a filename.

Mark anything written without review, in the document itself. One line at the
bottom saying who wrote it and whether anybody checked it is enough. Then a
later session can weigh it correctly, and so can you.

## Give it the checks, not just the task

The most useful thing you can ask for is the thing that proves the work. Types
that compile, a build that completes, a test that fails when the code is wrong.

Tests matter most where a mistake cannot be undone. In the app these notes come
from, one file held the code that saved a parent's diary to their phone, and the
phone was the only copy. Tests were written for that file before changing it.
The first run found a bug that had been there for weeks: after certain reads the
save could skip a field it wrongly believed was already stored, and that field
would be lost with nothing reported. Nobody would have found it by reading, and
no user would have known until their diary went backwards.

Write tests for the parts where being wrong is permanent. Skip them where being
wrong is visible and cheap.

## Check that the test is standing where the user stands

A test proves something about the position it runs from, and an agent will
happily write one that runs from a position no user ever occupies. It passes, it
is named after the thing you wanted, and it is measuring nothing.

The clearest case is a database with rules about who may read which row. An
agent asked to test them wrote thirty three checks, every one of which ran as
the database administrator. Administrators are exempt from those rules. Every
check passed, and would have passed just as well with all of the rules deleted,
because none of them was ever consulted. The file was described in the project's
own setup document as the proof that one family could not read another family's
records, and it never tested that once.

Rewriting the same checks to run as an ordinary signed in user took an
afternoon and immediately found a real hole: anyone with an account who learned
a household's id could add themselves to it and read another family's child
health record, with no invitation. Careful single use invitation tokens sat
above it, tested and working, and entirely optional.

Nothing about this is specific to databases. The same shape turns up wherever a
test runs with more authority than a user has: a file permission checked while
running as root, an API tested with an admin key, a paywall tested by code that
sets the subscription flag itself, a login check tested from inside the session
it is meant to guard.

Two questions catch it, and they are worth asking of any test that guards
something rather than computes something.

- **Who is this test running as, and does that resemble a real user?** Ask it
  plainly. Agents answer accurately and rarely volunteer it.
- **If I delete the thing this is supposed to protect, does the test go red?**
  Have the agent actually do it and show the failure, then put it back. A
  security test you have never seen fail is not evidence that it works.

The second question is the one that generalises. Any test whose failure you have
not personally seen might be testing nothing at all.

## Small steps, checked at every step

Agents are good at producing a lot of plausible code quickly. That is the risk,
not the benefit. A large change that compiles is not a large change that works.

Ask for one thing, check it, then ask for the next. When a change touches
something wide, ask what the blast radius is first. Counting is cheap: in the
app above, moving the storage layer looked enormous until somebody counted and
found six files touched storage and thirty seven only read the data.

## Watch what the work costs while it runs

Automated systems set up by an agent keep running after the conversation ends.

Two examples from the same project. A build service fired a cloud build on
every push to the main branch, and each build spent credits from a paid
allowance. Several days of ordinary commits went before anybody noticed. Later
the same pattern was rebuilt deliberately on a different service where builds
were free, and even then a repository that turned out to be private meant the
free minutes came from a monthly allowance rather than being unlimited.

Before agreeing to anything automatic, ask what it costs per run, who pays, and
what stops it.

## Two agents on one repository will collide

If you run more than one session, they do not know about each other. Both will
edit the same files, both will claim to have fixed the same thing, and one will
report work that never reached the main branch.

Give each session its own branch. Check what actually landed rather than
trusting the report. In the project above, one session reported committing and
pushing a fix, and the fix was not on the main branch at all, so the problem it
described was still live hours later.

## Decide what the agent may do without asking

Write it down once, in a file the agent reads at the start of every session.
Merging to the main branch, spending money, pushing to a store, deleting data:
say which of these need permission and which do not.

Without that, every session guesses, and the guesses differ.

## Read the diff for anything you cannot test

Types and builds catch the mechanical mistakes. They say nothing about whether
the logic is right, whether the wording is honest, or whether a screen collects
something it should not.

Read the changes to anything that touches money, personal data, safety advice,
or deletion. In the same project an agent added a backup copy of the local
store, which was a good idea, and the backup was not covered by the delete
everything function. A user who deleted their health record would have kept a
full copy of it on the device. It was caught by rereading the change, not by
any test.

## Sweep for the things that only look finished

An agent asked to build a screen will build the screen. Asked to build a
paywall it will build prices, a confirm button and a tick list, and whether
anything is charged depends entirely on whether somebody asked for that too.

So look for the gap deliberately, rather than waiting to trip over it. Two
things find it. Keep a to be continued file that the agent writes into whenever
it finishes part of something, described in the next document. And use the
thing yourself on a schedule, because every one of these found so far was found
by using the app rather than by reading the code.

The question that works: pick a feature and ask what happens end to end when a
real person uses it. An agent answers that accurately. It rarely volunteers it.

## Ask for the honest summary at the end

A useful closing question is: what did you verify, what did you assume, and
what did you leave undone.

Agents will answer that accurately when asked. They will not always volunteer
it, and a piece of work that compiles can still be half finished.

## Batch the changes around what the person has to do by hand

An agent finishes a change quickly and wants to hand it over, which is the
right instinct when the person can see the result by reading a diff. It is the
wrong instinct when checking the work means installing a build, walking through
a flow, or waiting for a deploy.

Count the cost on their side, not on yours. If eight changes are waiting and
each one costs them ten minutes of installing and clicking, doing them one at a
time costs eighty minutes of somebody's evening to save nothing. Do the list,
then hand over once.

The exception is a change big enough or risky enough that landing it with seven
others would make a bad result hard to read. Say which one it is, say why, do
the rest, and hold that one on its own.

## Say what is waiting on the person, and why

An agent that works for an hour and then reports what it did has answered half
the question. The other half is what it now needs from the person, and agents
are bad at saying it. A decision that only the person can take gets buried in
the middle of a paragraph about something else, or written into a markdown
file, or left as a comment in the code.

The person only reads the chat. A question written into a file is not a
question, it is a note the agent left for itself.

So ask for the same thing at the end of every piece of work. Give a short list
of what is waiting, and for each item say what the choice is and why it cannot
be made without them. The items fall into three kinds, and separating them
helps, because each kind needs something different.

- **A check the agent cannot run.** It needs a real phone, a real payment, or a
  real account. Say what to do, and say what a bad result looks like, so the
  person knows when to stop and come back.
- **A decision that should not be taken alone.** It costs money, or it changes
  what people see, or it cannot be undone. Put the options and the cost of each
  one in the message itself, in enough detail to answer without opening
  anything.
- **Nothing at all.** Work that was already agreed is not waiting on anybody.
  Say that it is going ahead, so that one yes is not read as a request for
  another one.

The test is whether the person can answer from the message alone. If the answer
starts with going away to read a file, the question was not asked properly.
