# Documents worth having

You start every session knowing nothing about the project except what you can
read. These are the files that make the next session start where the last one
finished.

Keep them in the repository next to the code, so they are updated in the same
commit as the thing they describe. A document kept anywhere else goes stale in a
week.

Write the fewest documents that change what somebody does. A document that only
describes is a document nobody updates.

## Keep a rules file at the root

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

Write a short entry when a decision took real thought, covering what was chosen,
what else was considered, and why. Two paragraphs. Skip it when the answer was
obvious.

**Why:** a new session will otherwise propose the option that was already
rejected, and propose it convincingly. One entry saying the store's own payments
were chosen because both stores require them ends that conversation in a line.

## Keep a build and release runbook

Write down how to build the thing, how to get it to a person, and what each
route costs. Include the routes that do not work and why, because somebody will
try them. Mark the steps that need a person with an account or a payment method.

**Why:** somebody otherwise loses an hour finding out that a step needs a
credential they do not have.

## Keep a data map if you hold anything personal

Write what you collect, why you collect it, where it goes, how long you keep it,
and how somebody gets rid of it.

**Human:** health data, children's data and similar cases usually need a formal
impact assessment, and a lawyer should read it. An agent can draft it and cannot
sign it off.

**Why:** the same answers fill in the store privacy forms and answer any user
who asks, and all of them have to match what the code actually does.

## Keep a to be continued file

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

## Keep test notes

Write one paragraph saying what is tested, what is not, and why not.

**Why:** an agent asked to change something treats a passing test suite as proof
it did no harm. If half the code has no tests, the suite proves half as much.

## Keep domain facts in one file outside the code

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
