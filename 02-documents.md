# Documents worth having

> The files that make the next session start where the last one finished, in
> rough order of how much each one earns its keep.

You start every session knowing nothing about the project except what you can
read.

None of these files is required. Start with the first two, and add another when
you feel the lack of it rather than because it appears on a list. A project
holding `SPECS.md` and `TO-BE-CONTINUED.md` and nothing else is in better shape
than a project holding all ten written once and never opened again.

Keep them in the repository next to the code, so that somebody updates them in
the same commit as the thing they describe. A document kept anywhere else goes
stale in a week.

Write the fewest documents that change what somebody does. A document that only
describes is a document nobody updates.

There is a filled in example of each one in [examples](./examples/), all
describing the same invented project. Copy the shape and throw the content away.

## Which files are read every session, and which wait for a trigger

Read three files at the start of every session, and open every other one when
something happens.

- `CLAUDE.md` or `AGENTS.md`, the rules file, which your agent loads on its own.
- `SPECS.md`, for what the product is and which feature is being built now.
- `TO-BE-CONTINUED.md`, for what only looks finished.

Everything else gets a trigger, and the rules file says what each trigger is.
Read `runbook.md` before the first commit, read `decisions.md` before proposing
a change of direction, and read `change-log.md` almost never.

Write that split into the rules file rather than leaving an agent to work it
out. An agent that cannot tell the difference either reads everything, which
spends its attention before the work starts, or reads whichever file it happened
to open first.

The three files read every time are the three that have to stay short, and
keeping them short is a separate job from writing them. A rules file over two
pages, a spec over one page, or an unfinished list over two pages stops being
read carefully. The cost is not the reading. Everything after a long file gets
read less carefully too.

*One project's `TO-BE-CONTINUED.md` reached 1958 lines, so an agent following
the rules file read 2344 lines before it could plan anything. Most of those
lines recorded work that was already finished.*

## Conventions that apply to all of them

**Name each file the name used as a heading below.** An agent looking for the
unfinished work then opens `TO-BE-CONTINUED.md` instead of guessing, and the
rules file can point at a path instead of a description.

**Write the three every session files in capitals**, meaning `CLAUDE.md`,
`SPECS.md` and `TO-BE-CONTINUED.md`, and everything else in lowercase. The
capitals are the only signal a directory listing gives about which files to open
first, and they signal nothing unless all three carry them and no other file
does.

**Never let two files compete for the same word.** A project holding both
`SPECS.md` and a `specs/` folder makes every reference to "the spec" ambiguous,
and an agent resolves the ambiguity silently and sometimes wrongly.

**Start every document with one line under the title saying what it covers**,
which is the quoted line under each heading below. You decide what to open from
the first line of a file, and a document that does not say what it covers gets
opened when nobody needs it and missed when somebody does.

**Put one line at the bottom of any document you write** saying who wrote it and
whether anybody checked it, and do not cite an unreviewed document as evidence,
your own included. A guess you wrote down is still a guess, and giving it a
filename makes it look like a source. An agent has quoted a document it wrote
three hours earlier back to the user as though the document settled the
question.

**Name who can lift a "do not edit this wording" note.** Say who can lift it,
what has to happen first, and what to do instead. A note that only forbids gets
worked around by somebody who has a deadline and nobody to ask.

**Delete what a new document replaces, in the same commit as the new document.**
Writing the better version is the easy half. Going back to the older document
and removing what the new one answers is the half that gets skipped, and
skipping it is worse than never writing the second document, because two files
then answer the same question differently and neither says which is right.

The last one happens most often when somebody creates the second file rather
than editing the first. An early document works something out and proposes an
answer inline, the answer later grows enough to deserve a file of its own,
somebody writes the new file, and the old proposal stays where it sits. Nobody
rereads a section whose contents they already know, so the person who finds it
is whoever follows the instructions for the first time.

Split the two kinds of content and let each file hold one kind. The reasoning,
the measurements and the rejected options belong wherever somebody did the
thinking. The value that gets used belongs in exactly one file, and every other
document points at that file rather than repeating the value.

Then write the check, which is usually a few lines. Search every document for
anything shaped like the value, and fail the build if any file other than the
one that owns it holds a copy. The check has to know every shape a value can
take, because a check matching one syntax is a check against one mistake rather
than against the fault. Fail any other document that lays the same thing out
under its own headings as well, because a rival document is easier to spot than
any single wrong value. And watch the check fail once before you trust it, which
[the approach](./01-approach.md) covers.

*One project's check for this passed a repository holding two complete Play
Store listings written the same day, each with a different short description and
a different full description. The check looked for values written in inline
backticks, and the second document put every value in a fenced block.*

## `SPECS.md`

> The project in four lines, every feature it has, and the one being built now.

Write the spec in three parts before the work starts, and get agreement on it
before writing any code.

**The project, in four lines.** Say what it is, who it is for, why it exists,
and how it is built. Write those lines once at the top and leave them there,
because a session that opens the spec and nothing else still needs to know what
the project is. The four lines describe how the project already stands, which is
a fact anybody can check.

**Every feature, one line each.** Give each feature a short name, say what
somebody can do with it, and mark it as shipped, being built, half built, or not
started. Every feature belongs here, including the ones that shipped a year ago,
because the list is what the product is. A feature marked half built points at
`TO-BE-CONTINUED.md` for the detail, so that the two documents do not repeat
each other.

**The feature being built now, under four headings.**

- **What somebody can do when it is finished.** Write what the person does and
  sees. Do not write what the code does.
- **How anybody will know it works.** Write a check somebody can run by hand,
  and write it so that it can fail. "Buy the subscription on a real phone and
  find the receipt in App Store Connect" rather than "purchases work".
- **What is out of scope.** Name the things an agent would otherwise build
  anyway, and the things a reader would assume are included.
- **What is undecided.** Name the questions to answer before or during the work.
  An agent asks about these rather than picking one.

Say nothing in that part about how to build the feature. Choosing the approach
is the work, and a spec that names the approach has taken the decision before
anybody looked at the code.

Do not number the features, do not rank them, and do not write the checks in a
formal notation. A page somebody reads beats a document somebody skims.

When a feature ships, collapse it back to its one line in the list and write the
next one out in full. Move any decision worth keeping to `decisions.md`, and
move anything unfinished to `TO-BE-CONTINUED.md`.

Keep the "how anybody will know it works" checks when you collapse a feature,
in the area file if you have one. The checks are the walkthrough somebody
follows on a phone before the next release, and they go on finding faults long
after the feature ships. On one project the walkthrough is the only thing that
has ever found a fault. Somebody using the app found every problem in that
project and nobody found one by reading the code, so throwing the checks away
with the rest of the detail costs more than the page it saves.

When the product outgrows one page, split it by area rather than by feature,
e.g. `SPECS-diary.md` and `SPECS-articles.md`. Keep the four project lines and
the full list of features in `SPECS.md`, so that one place still shows the whole
product on one screen. Splitting per feature gives you a directory nobody reads
and loses the only view of the product.

A product built without any of this starts from the other end. Write the feature
list first, from the app rather than from the code, and mark each feature
shipped or half built. Then write an area file for each part that has more
behaviour than a line can hold, and write the checks in it as the walkthrough
for that part. Do not try to reconstruct what anybody intended before the work
started, because nobody wrote it down and a guess will read as agreement.

Share the feature names with `architecture.md`, whose diagram uses the same
names, so that anybody can see which part of the system serves which feature.

**Why:** an agent given a one line request invents the rest of the scope,
confidently and in detail, and somebody finds the invented parts weeks later by
using the app. Disagreeing about a page costs ten minutes and disagreeing about
a built feature costs the build.

The full list earns its keep separately from the detail. A session that can see
every feature stops proposing one that already exists under another name, stops
treating the thing it was just asked for as though it were the whole product,
and can tell you what your product is in one screen. No other document does
that, because every other document describes a part.

## `TO-BE-CONTINUED.md`

> Everything that looks finished and is not. Read it before planning anything.

List everything that looks finished and is not, e.g. the button that does
nothing yet, the screen that fakes its result, the setting that drives no
behaviour, and the limit nobody enforces. Write into it in the same commit as
the partial work, read it at the start of a session, and put the newest entry at
the top.

**Take an entry out in the commit that finishes the work, and write what
happened in `change-log.md`.** The file has to shrink as well as grow, and
shrinking is the half that gets skipped, because an agent that has just fixed
something finds the entry already written and adds the fix underneath it. Never
add anything here to record that something got done.

Name something in the app today that does not work the way it looks, in every
entry. If you cannot write the "does not" as a sentence about the app as it
stands, the entry belongs somewhere else. Give each gap its own heading, and
never head an entry with a date or with the request that started it, because a
dated heading is the shape a change log takes and nobody can delete the entries
under one separately.

Group the entries by who has to act, because that is the question a session
opens the file to answer. A decision only the owner can take, a check that needs
a real phone or a paid account, and work an agent could pick up today are three
different things. An agent that cannot tell them apart either asks about work it
was allowed to do or starts work it cannot finish.

Keep it short enough to read in full at the start of every session, and two
pages is a working list. If it will not fit, either the entries are histories
that belong in the change log, or the project holds more half built work than
anybody can carry, and knowing that is useful on its own.

**Human:** read it on a schedule and check the app against it rather than only
reading the file. Do that sweep before every release, and once a month whether
or not you are releasing. Somebody using the app found every gap found so far,
and nobody found one by reading the code.

**Why:** it is the only document that tells the difference between a working
feature and a convincing one, and it doubles as the launch checklist. It does
both only while it stays short enough for somebody to read all of it.

*One project's file reached 1958 lines across 62 sections, and most of them
recorded work finished weeks earlier. Two sections described the same unfinished
item with opposite statuses, because nobody could see the whole file at once.*

## `change-log.md`

> What changed, when, and why, newest first. Written to often, and read rarely.

Add this file once `TO-BE-CONTINUED.md` exists. It appears third because it is
the other half of the document above, and not because it earns its keep third.

Write one entry when a piece of work changes what somebody can see, or changes
what a later session can rely on. Give it the date, say what changed, and give
the reason where the reason is not obvious. Newest at the top.

Do not write an entry for every commit, because the history already holds those
and a file that repeats them gets skimmed. Do not write an entry for work nobody
outside the session would notice.

**Nobody reads it at the start of a session, and its first line should say so.**
Open it when you need to know when something changed or why, e.g. a fault that
appeared between two builds, or a rule somebody remembers differently from what
the code does.

**Why:** without somewhere to put finished work, the finished work stays in
`TO-BE-CONTINUED.md`. An agent that fixes something finds the entry already
written, and adding "this is fixed now" underneath it is easier than deleting
the entry and writing the outcome somewhere else. Do that for a month and the
document read at the start of every session mostly records work that is done.

## `CLAUDE.md`, or `AGENTS.md`

> What an agent may do without asking, what costs money when it runs, and the
> commands that check the work.

Read [examples/CLAUDE.example.md](./examples/CLAUDE.example.md) before you write
your own. It is the one example worth reading in full, because it is the only
document your agent loads without being told.

Put the file at the root, under whichever name your agent reads. Put in it what
an agent may do without asking, what costs money when it runs, the commands that
check the work, which documents nobody has reviewed, and any fact about the
project that is true and surprising, e.g. a tool everybody assumes works that
does not work here.

Keep it under two pages. Cut any rule that has never changed what somebody did.
Keep procedures out of it, because a procedure with steps belongs in
`runbook.md` and a rule that fits in a line belongs here.

**Open it with how to work here.** Copy [the approach](./01-approach.md) into
the project when you create it, and point at your copy from the rules file. Do
not link to this repository, because a project should not depend on the kit
being checked out beside it. Keep the copy as its own file and summarise it in
five lines in the rules file, because a general rule that never reaches the
rules file is a rule nobody follows, and a general rule copied into the rules
file in full will grow the rules file past two pages.

**Then say what an agent may decide, area by area.** A project has parts an
agent should change without asking and parts it should not touch alone, and one
blanket answer covers neither. Write a row per area giving the initiative an
agent has there and the documents that govern it. Screens and copy usually read
"go ahead". Anything holding money or the only copy of somebody's data usually
reads "propose it, do not build it".

**End it with the index**, saying when to open each document rather than what
each one says, because the first line of each file already says that.

**Why:** it is the only document read before the work starts, so it is the only
one that changes behaviour rather than informing it. A rules file that runs to
five pages stops being read carefully, by people as well as by agents.

The index has to be complete, and it goes wrong in both directions. A document
with no row is a document nobody opens, whatever it holds. A row pointing at a
document that no longer does what the row says sends an agent to the wrong file,
and the agent reads what it finds there as the answer. Check the index against
the files that exist whenever you add or remove one, because nothing else will.

### Write the index as instructions, not as information

An index only works when it reads as something to do. The phrasing goes wrong
without announcing itself, and it needs more care than the contents.

An agent reads the whole file and follows the parts phrased as commands. An
agent treats anything phrased as a description as background, meaning true,
noted, and not something to act on now. The two look almost identical while you
write them, and completely different to whoever reads them.

One rules file carried exactly the index this page recommends, headed "Which
document to open, and when", listing each document against the moment it
mattered, including a runbook against "before your first commit". The index was
accurate and complete, and an agent read it as a directory to consult when
stuck. The agent went from the request straight to the code and made fourteen
commits and four builds without opening the runbook, which held a verification
step written down because skipping it had cost a day.

Nothing broke that time, and the file looked correct afterwards. A failure that
leaves no trace is the reason to write the rule down.

- **Say "do this", rather than "that lives here".** A table of documents against
  triggers gives an agent somewhere to look. A numbered list saying "before
  planning, read these two" gives an agent something to do.
- **Put the ordering in**, e.g. "at the start of every session", "before your
  first commit", "before pushing a build to anybody". An agent that knows a
  document is relevant still needs to know the document is relevant now, before
  it starts.
- **Name the check in the rules file as well as in the document it belongs to.**
  Where a step must never be skipped, one line of it belongs where somebody
  reads it first, with a pointer to the detail. A rule that only exists inside
  the document nobody opened is a rule that does not exist.
- **Say what happens when somebody skips it.** "Right code, right build, wrong
  file installed, and it cost a day" gets followed. "Verify before release" gets
  filed.
- **Assume the reader is in a hurry and competent.** A reader in a hurry acts on
  imperatives and skims descriptions, which is why the phrasing does so much of
  the work.

The same applies to a person joining the project, and a person at least asks. An
agent does not ask, and proceeds with whatever it took to be optional.

## `runbook.md`

> How the work is done here, from where new code goes to how it reaches a
> person, and which document to update when something changes.

Write down how to build and run it, how to check it, how branches and merges
work here, how to get a build to a person, and what each route costs. Include
the routes that do not work and why, because somebody will try them. Mark the
steps that need a person with an account or a payment method.

Put the checks that can be automated into the command list, including the
mechanical part of the writing style. Telling an agent to follow a style is
weaker than a command that fails, and this repository proved it. Twenty em
dashes reached the documents in one week while the style file banning them sat
in the repository, pointed at from the approach.

Then answer, for this project rather than in general, how the work is actually
done. Say where new code goes. Say which parts get tests and which deliberately
do not. Say what counts as done. Say whether half built work goes behind a flag.
Say how often you release and to whom. [The approach](./01-approach.md) says to
test where being wrong is permanent, and this file is where you say which
directories that means here.

End it with the triggers that keep the other documents alive, written as when
this happens, update that file. Adding a service updates `architecture.md`.
Shipping a feature collapses its line in `SPECS.md`. Leaving something half
built writes into `TO-BE-CONTINUED.md`. Choosing between two real options writes
into `decisions.md`. Finishing something deletes its entry from
`TO-BE-CONTINUED.md` and writes a line in `change-log.md`, both in the same
commit.

Write that last trigger most carefully, because it is the only one that takes
something away. Every other trigger adds, so a list of triggers with no removal
rule describes a set of documents that can only grow.

`CLAUDE.md` says what an agent may do and what to watch out for. This file says
how things are done. Put a rule that fits in a line there, and a procedure with
steps here.

**Why:** somebody otherwise loses an hour finding out that a step needs a
credential they do not have, and every other document rots quietly, because
nothing anywhere says when to update them and no test fails when nobody does.

### Keep a replaced way of working, marked and dated

When you replace a procedure, leave the old one in the runbook under a heading
saying that nobody uses it now and giving the date it stopped. Say what it did,
say what replaced it, and say what would bring it back.

The old conditions come back. Somebody revokes a credential, a machine changes,
an allowance runs out, and the route nobody has used for a month is the one that
gets somebody working again that afternoon. Deleting it saves a screen of
scrolling today and costs a rediscovery on the day somebody wants it.

The labelling is the whole of the rule. An old procedure sitting there unmarked
is worse than a deleted one, because the next person follows it.

Procedures work this way and values do not. A value belongs in exactly one file,
and the convention above says to go back and delete the copy that was replaced.
A procedure is a record of what somebody did, and a retired one with a date on
it is a record rather than a rival, as long as the heading says which one is
live.

**Why:** a project that deletes its history keeps rediscovering it, and the
rediscovery lands on whoever is already blocked.

*One project moved its store uploads from a personal login to a project
credential, and the session doing it deleted the paragraphs describing the old
route. The old route is the fallback the upload script still falls back to, and
the conditions that would bring it back, a deleted key or a policy switched on
again, are exactly the conditions under which nobody has time to work it out
from the code.*

## `architecture.md`

> What runs where, what talks to what, and which parts hold personal data.

Say what runs where, what talks to what, and which connections carry personal
data. Write the diagram in Mermaid so that it lives in the markdown file and
renders on GitHub without a separate image to keep in step. The diagram is worth
more than the page.

Label each box with the features it serves, using the names from `SPECS.md`.
Anybody can then see where a feature lives, and which features stop working when
one box does. A box that serves no feature is either dead code or a feature
nobody wrote down.

The page should answer these questions without anybody reading code.

- Does data leave the device or the server.
- What happens when the network is gone.
- Where is the only copy of anything.
- Which parts are optional.

**Why:** you otherwise rebuild a picture of the system from files every session,
and a wrong picture is where confident wrong changes come from.

## `decisions.md`

> The decisions that took real thought, what else was considered, and why. Read
> it before proposing a change of direction.

Write a short entry when a decision took real thought, covering what was chosen,
what else was considered, and why. Two paragraphs. Skip it when the answer was
obvious.

**Why:** a new session will otherwise propose the option you already rejected,
and will propose it convincingly. One entry saying the store's own payments were
chosen because both stores require them ends that conversation in a line.

## `test-notes.md`

> What is tested, what is not, and what a passing test run does and does not
> prove.

Write one paragraph saying what is tested, what is not, and why not.

**Why:** an agent asked to change something treats a passing test suite as proof
it did no harm. If half the code has no tests, the suite proves half as much.

## `data-map.md`

> Every piece of personal data you hold, where it goes, how long it stays, and
> how somebody gets rid of it.

Write this only if you hold anything personal. Say what you collect, why you
collect it, where it goes, how long you keep it, and how somebody gets rid of
it.

**Human:** health data, children's data and similar cases usually need a formal
impact assessment, and a lawyer should read it. An agent can draft it and cannot
sign it off.

**Why:** the same answers fill in the store privacy forms and answer any user
who asks, and all of them have to match what the code actually does.

**Give it a rule that nothing reaches the code without reaching this file, and
expect the rule on its own to fail.** A rule in a document is obeyed by whoever
read the document, and the session that adds a measurement is usually not
reading the data map. [Lessons](./lessons.md) holds the standing version of this
under "a document that describes what you collect goes wrong the day collection
changes", and the part that works is the test rather than the rule.

## `content/`

> Where the facts people argue about live, who edits them, and how they become
> code.

Write this only if the project has content, rules, prices or thresholds that
people argue about. Keep them in one file that is not code, in a folder of their
own, and generate the code from it. Edit the source file and run the generator.
Never hand edit the generated output, and never edit both.

**Why:** somebody who does not read code can then review a change, and you can
change one value and rebuild without touching any code.

*Several hundred articles lived in a spreadsheet that a person edited, and a
script turned it into a data file.*

## Documents not to write

Every heading above names a file to create. Leave these out.

Do not write a document that repeats what the code says, because it will be
wrong within a month and nobody will notice.

Do not write a long specification up front and expect an agent to follow it.
Short rules kept close to the work get followed. Long documents written before
the work started do not.
