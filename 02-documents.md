# Documents worth having

> The files that make the next session start where the last one finished, in
> rough order of how much they earn their keep.

You start every session knowing nothing about the project except what you can
read.

None of these is required. Start with the first two, and add another when you
feel the lack of it rather than because it is on a list. A project holding
`spec.md` and `to-be-continued.md` and nothing else is in better shape than a
project with all nine written once and never opened again.

Each heading below is the name of the file. Use those names, so that an agent
looking for the unfinished work opens `to-be-continued.md` instead of guessing,
and so that the rules file can point at a path rather than a description.

Keep them in the repository next to the code, so they are updated in the same
commit as the thing they describe. A document kept anywhere else goes stale in a
week.

Start every one of them with a single line under the title saying what it
covers, which is the quoted line under each heading below. You decide what to
open from the first line of a file, and a document that does not say what it
covers gets opened when it is not needed and missed when it is.

Write the fewest documents that change what somebody does. A document that only
describes is a document nobody updates.

There is a filled in example of each one in [examples](./examples/), all
describing the same invented project. Copy the shape and throw the content away.

## `spec.md`

> The project in four lines, every feature it has, and the one being built now.

Three parts, written before the work starts and agreed before any code.

**The project, in four lines.** What it is, who it is for, why it exists, and
how it is built. Write them once at the top and leave them there, because a
session that opens the spec and nothing else still needs to know what the
project is. The four lines say how the project already stands, which is a fact
anybody can check.

**Every feature, one line each.** Give each feature a short name, say what
somebody can do with it, and mark it as shipped, being built, half built, or not
started. Every feature belongs here, including the ones that shipped a year ago,
because the list is what the product is. A feature marked half built points at
`to-be-continued.md` for the detail, so the two documents do not repeat each
other.

**The feature being built now, in four headings.**

- **What somebody can do when it is finished.** Write what the person does and
  sees. Do not write what the code does.
- **How anybody will know it works.** A check somebody can run by hand, written
  so that it can fail. "Buy the subscription on a real phone and find the
  receipt in App Store Connect" rather than "purchases work".
- **What is out of scope.** The things an agent would otherwise build anyway,
  and the things a reader would assume are included.
- **What is undecided.** The questions to answer before or during the work. An
  agent asks about these rather than picking one.

Nothing in that part says how to build the feature. Choosing the approach is the
work, and a spec that names the approach has taken the decision before anybody
looked at the code.

Do not number the features, do not rank them, and do not write the checks in a
formal notation. A page somebody reads beats a document somebody skims.

When a feature ships, collapse it back to its one line in the list and write the
next one out in full. Move any decision worth keeping to `decisions.md`, and
anything unfinished to `to-be-continued.md`.

Keep the "how anybody will know it works" checks when you collapse a feature,
in the area file if you have one. The checks are the walkthrough somebody
follows on a phone before the next release, and they go on finding faults long
after the feature ships. On Baby What the walkthrough is the only thing that has
ever found one. Every problem in that project was found by somebody using the
app, and none by anybody reading the code, so throwing the checks away with the
rest of the detail costs more than the page it saves.

When the product outgrows one page, split by area rather than by feature, e.g.
`spec-diary.md` and `spec-articles.md`. Keep the four project lines and the full
list of features in `spec.md`, so there is still one place that shows the whole
product on one screen. Splitting per feature gives you a directory nobody reads
and loses the only view of the product.

A product that was built without any of this starts from the other end. Write
the feature list first, from the app rather than from the code, and mark each
one shipped or half built. Then write an area file for each part that has more
behaviour than a line can hold, and write the checks in it as the walkthrough
for that part. Do not try to reconstruct what anybody intended before the work
started, because nobody wrote it down and the guess will read as agreement.

The feature names are shared. The diagram in `architecture.md` uses the same
names, so anybody can see which part of the system serves which feature.

**Why:** an agent given a one line request invents the rest of the scope,
confidently and in detail, and the invented parts get found weeks later by
somebody using the app. Disagreeing about a page costs ten minutes and
disagreeing about a built feature costs the build.

The full list earns its place separately from the detail. A session that can see
every feature stops proposing one that already exists under another name, stops
treating the thing it was just asked for as though it were the whole product,
and can tell you what your product is in one screen. Nothing else here does
that, because every other document describes a part.

## `to-be-continued.md`

> Everything that looks finished and is not. Read it before planning anything.

List everything that looks finished and is not, e.g. the button that does
nothing yet, the screen that fakes its result, the setting that drives no
behaviour, and the limit that is not enforced. Write into it in the same commit
as the partial work, and read it at the start of a session.

**Human:** read it on a schedule and check the app against it rather than only
reading the file. Do that sweep before every release, and once a month whether
or not you are releasing. Every gap found so far was found by somebody using the
app rather than by anybody reading the code.

**Why:** it is the only document that tells the difference between a working
feature and a convincing one, and it doubles as the launch checklist.

## `CLAUDE.md`, or `AGENTS.md`

> What an agent may do without asking, what costs money when it runs, and the
> commands that check the work.

At the root, under whichever name your agent reads. Put in it what you may do
without asking, what costs money when it runs, the commands that check the work,
which documents nobody has reviewed, and any fact about the project that is true
and surprising, e.g. a tool everyone assumes works that does not work here.

Keep it under two pages. Cut any rule that has never changed what somebody did.
Keep procedures out of it, because a procedure with steps belongs in
`runbook.md` and a rule that fits in a line belongs here.

End it with an index of the other documents saying when to open each one, rather
than what each one says, because the first line of each file already says that.
Name the ones to read at the start of every session, which are `spec.md` for
what the product is and `to-be-continued.md` for what only looks finished. Give
every other one a trigger, e.g. read `runbook.md` before the first commit and
`decisions.md` before proposing a change of direction.

**Why:** it is the only document read before the work starts, so it is the only
one that changes behaviour rather than informing it. A rules file that runs to
five pages stops being read carefully, by people as well as by agents.

An index is the reason to keep it short and still have the rest get read.
Telling an agent to read all nine documents spends the session on files the work
never touches, and telling it nothing means it reads whichever file it happened
to open and misses the one that mattered.

### Write the index as instructions, not as information

An index only works if it reads as something to do. This is the part that goes
wrong quietly, and it is worth more care than the contents.

An agent reads the whole file and follows the parts phrased as commands.
Anything phrased as a description it treats as background: true, noted, and not
something to act on now. The two look almost identical while you are writing
them, and completely different to whoever reads them.

A rules file once carried exactly the index this page recommends, headed **"Which
document to open, and when"**, listing each document against the moment it
mattered, including a runbook against "before your first commit". It was
accurate and it was complete, and an agent read it as a directory to consult
when stuck. It went from the request straight to the code and made fourteen
commits and four builds without opening the runbook, which held a verification
step written down precisely because skipping it had cost a day.

Nothing broke that time. That is what makes it worth writing down: the failure
does not announce itself, and the file looks correct afterwards.

So:

- **Say "do this", not "this is where that lives".** A table of documents against
  triggers is a map. A numbered list saying "before planning, read these two" is
  an instruction.
- **Put the ordering in.** "At the start of every session", "before your first
  commit", "before pushing a build to anybody". An agent that knows a document
  is relevant still needs to know it is relevant *now*, before it starts.
- **Name the check in the rules file, not only in the document it belongs to.**
  If a step must never be skipped, one line of it belongs where it will be read
  first, with a pointer to the detail. A rule that only exists inside the
  document nobody opened is a rule that does not exist.
- **Say what happens when it is skipped.** "Right code, right build, wrong file
  installed, and it cost a day" gets followed. "Verify before release" gets
  filed.
- **Assume the reader is in a hurry and competent.** That is exactly the reader
  who acts on imperatives and skims descriptions, which is why the phrasing
  carries so much of the weight.

The same applies to a person joining the project, and they at least ask. An
agent does not ask; it proceeds with what it took to be optional.

## `runbook.md`

> How the work is done here, from where new code goes to how it reaches a
> person, and which document to update when something changes.

Write down how to build and run it, how to check it, how branches and merges
work here, how to get a build to a person, and what each route costs. Include
the routes that do not work and why, because somebody will try them. Mark the
steps that need a person with an account or a payment method.

Then answer, for this project rather than in general, how the work is actually
done. Where new code goes. Which parts get tests and which deliberately do not.
What counts as done. Whether half built work goes behind a flag. How often you
release and to whom. `01-approach.md` says to test where being wrong is
permanent, and this file is where you say which directories that means here.

End it with the triggers that keep the other documents alive, written as when
this happens, update that file. Adding a service updates `architecture.md`.
Shipping a feature collapses its line in `spec.md`. Leaving something half built
writes into `to-be-continued.md`. Choosing between two real options writes into
`decisions.md`.

`CLAUDE.md` says what you may do and what to watch out for. This file says how
things are done. Put a rule that fits in a line there, and a procedure with
steps here.

**Why:** somebody otherwise loses an hour finding out that a step needs a
credential they do not have, and every other document rots quietly, because
nothing anywhere says when to update them and no test fails when nobody does.

## `architecture.md`

> What runs where, what talks to what, and which parts hold personal data.

Say what runs where, what talks to what, and which connections carry personal
data. Write the diagram in Mermaid so it lives in the markdown file and renders
on GitHub without a separate image to keep in step. The diagram is worth more
than the page.

Label each box with the features it serves, using the names from `spec.md`.
Anybody can then see where a feature lives, and which features stop working when
one box does. A box that serves no feature is either dead or a feature nobody
wrote down.

The page should answer these without anybody reading code:

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

**Why:** a new session will otherwise propose the option that was already
rejected, and propose it convincingly. One entry saying the store's own payments
were chosen because both stores require them ends that conversation in a line.

## `test-notes.md`

> What is tested, what is not, and what a passing test run does and does not
> prove.

Write one paragraph saying what is tested, what is not, and why not.

**Why:** an agent asked to change something treats a passing test suite as proof
it did no harm. If half the code has no tests, the suite proves half as much.

## `data-map.md`

> Every piece of personal data you hold, where it goes, how long it stays, and
> how somebody gets rid of it.

Only if you hold anything personal. Write what you collect, why you collect it,
where it goes, how long you keep it, and how somebody gets rid of it.

**Human:** health data, children's data and similar cases usually need a formal
impact assessment, and a lawyer should read it. An agent can draft it and cannot
sign it off.

**Why:** the same answers fill in the store privacy forms and answer any user
who asks, and all of them have to match what the code actually does.

## `content/`

> Where the facts people argue about live, who edits them, and how they become
> code.

Only if the project has content, rules, prices or thresholds that people argue
about. Keep them in one file that is not code, in a folder of their own, and
generate the code from it. Edit the source file and run the generator. Never
hand edit the generated output, and never edit both.

**Why:** somebody who does not read code can then review a change, and you can
change one value and rebuild without touching any code.

*Several hundred articles lived in a spreadsheet that a person edited, and a
script turned it into a data file.*

## Do not write these

Every heading above is a file to create. These are the ones to leave out.

Do not write a document that repeats what the code says, because it will be
wrong within a month and nobody will notice.

Do not write a long specification up front and expect an agent to follow it.
Short rules kept close to the work get followed. Long documents written before
the work started do not.
