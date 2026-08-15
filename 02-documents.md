# Documents worth having

> The files that make the next session start where the last one finished, in
> rough order of how much they earn their keep.

You start every session knowing nothing about the project except what you can
read.

None of these is required. Start with the first two, and add another when you
feel the lack of it rather than because it is on a list. A project with a spec
and a to be continued file and nothing else is in better shape than a project
with all nine written once and never opened again.

Keep them in the repository next to the code, so they are updated in the same
commit as the thing they describe. A document kept anywhere else goes stale in a
week.

Write the fewest documents that change what somebody does. A document that only
describes is a document nobody updates.

There is a filled in example of each one in [examples](./examples/), all
describing the same invented project. Copy the shape and throw the content away.

## Start every document with one line saying what it covers

Put one sentence at the top of every document, under the title and before
anything else, saying what the document covers. Keep it to a line. The quoted
line under each heading below is the one that document should carry.

**Why:** you decide what to open from the first line of a file, and a document
that does not say what it covers gets opened when it is not needed and missed
when it is.

## Keep a spec for each feature while it is being built

> What we are building next, how anybody will know it works, and what is
> deliberately left out.

One page in `specs/`, one file per feature, written before the work starts and
agreed before any code is written.

Open with four short answers about the project rather than the feature: what it
is, who it is for, why it exists, and how it is built. Four lines is enough, and
every spec repeats them, because a session that opens one spec and nothing else
still needs to know what the project is.

Then four headings, and nothing else.

- **What somebody can do when it is finished.** Write what the person does and
  sees. Do not write what the code does.
- **How anybody will know it works.** A check somebody can run by hand, written
  so that it can fail. "Buy the subscription on a real phone and find the
  receipt in App Store Connect" rather than "purchases work".
- **What is out of scope.** The things an agent would otherwise build anyway,
  and the things a reader would assume are included.
- **What is undecided.** The questions to answer before or during the work. An
  agent asks about these rather than picking one.

Do not number the requirements, do not rank them, and do not write the checks in
a formal notation. A page somebody reads beats a document somebody skims.

Delete the spec when the feature ships. Move any decision worth keeping to the
decision log, and anything unfinished to the to be continued file.

**Why:** an agent given a one line request invents the rest of the scope,
confidently and in detail, and the invented parts get found weeks later by
somebody using the app. Disagreeing about a page costs ten minutes and
disagreeing about a built feature costs the build. The second heading is the one
everybody skips, and it is the reason a paywall that charges nobody can look
finished in a screenshot and in the code.

## Keep a to be continued file

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

## Keep a rules file at the root

> What an agent may do without asking, what costs money when it runs, and the
> commands that check the work.

Name it CLAUDE.md or AGENTS.md. Put in it what you may do without asking, what
costs money when it runs, the commands that check the work, which documents
nobody has reviewed, and any fact about the project that is true and surprising,
e.g. a tool everyone assumes works that does not work here.

Keep it under two pages. Cut any rule that has never changed what somebody did.

**Why:** the rules file is the only document read before the work starts, so it
is the only one that changes behaviour rather than informing it. A rules file
that runs to five pages stops being read carefully, by people as well as by
agents.

## Keep one page of architecture and one diagram

> What runs where, what talks to what, and which parts hold personal data.

Say what runs where, what talks to what, and which connections carry personal
data. Write the diagram in Mermaid so it lives in the markdown file and renders
on GitHub without a separate image to keep in step. The diagram is worth more
than the page.

The page should answer these without anybody reading code:

- Does data leave the device or the server.
- What happens when the network is gone.
- Where is the only copy of anything.
- Which parts are optional.

**Why:** you otherwise rebuild a picture of the system from files every session,
and a wrong picture is where confident wrong changes come from.

## Log decisions that took real thought

> The decisions that took real thought, what else was considered, and why. Read
> it before proposing a change of direction.

Write a short entry when a decision took real thought, covering what was chosen,
what else was considered, and why. Two paragraphs. Skip it when the answer was
obvious.

**Why:** a new session will otherwise propose the option that was already
rejected, and propose it convincingly. One entry saying the store's own payments
were chosen because both stores require them ends that conversation in a line.

## Keep test notes

> What is tested, what is not, and what a passing test run does and does not
> prove.

Write one paragraph saying what is tested, what is not, and why not.

**Why:** an agent asked to change something treats a passing test suite as proof
it did no harm. If half the code has no tests, the suite proves half as much.

## Keep a build and release runbook

> How to build the thing, how to get it to a person, and which steps need
> somebody with an account.

Write down how to build the thing, how to get it to a person, and what each
route costs. Include the routes that do not work and why, because somebody will
try them. Mark the steps that need a person with an account or a payment method.

**Why:** somebody otherwise loses an hour finding out that a step needs a
credential they do not have.

## Keep a data map if you hold anything personal

> Every piece of personal data you hold, where it goes, how long it stays, and
> how somebody gets rid of it.

Write what you collect, why you collect it, where it goes, how long you keep it,
and how somebody gets rid of it.

**Human:** health data, children's data and similar cases usually need a formal
impact assessment, and a lawyer should read it. An agent can draft it and cannot
sign it off.

**Why:** the same answers fill in the store privacy forms and answer any user
who asks, and all of them have to match what the code actually does.

## Keep domain facts in one file outside the code

> Where the facts people argue about live, who edits them, and how they become
> code.

When the project has content, rules, prices or thresholds that people argue
about, keep them in one file that is not code and generate the code from it.
Edit the source file and run the generator. Never hand edit the generated
output, and never edit both.

**Why:** somebody who does not read code can then review a change, and you can
change one value and rebuild without touching any code.

*Several hundred articles lived in a spreadsheet that a person edited, and a
script turned it into a data file.*

## Do not write these

Do not write a document that repeats what the code says, because it will be
wrong within a month and nobody will notice.

Do not write a long specification up front and expect an agent to follow it.
Short rules kept close to the work get followed. Long documents written before
the work started do not.
