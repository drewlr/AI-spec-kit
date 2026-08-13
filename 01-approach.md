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

## Ask for the honest summary at the end

A useful closing question is: what did you verify, what did you assume, and
what did you leave undone.

Agents will answer that accurately when asked. They will not always volunteer
it, and a piece of work that compiles can still be half finished.
