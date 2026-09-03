# Lessons from real projects

> Specific findings from shipping real software with agents, grouped by subject
> and opened when you are working on that subject.

Nobody reads this from the top and none of it is required. It is the long tail
of [the approach](./01-approach.md), which holds the six rules about managing
the work.

Everything here was written because something went wrong once, and each entry
keeps the incident that produced it, because the incident is the reason to
believe the rule. Each group says what it applies to, so you can skip a group
that has nothing to do with what you are building.

An agent already knows how to write software. Nothing here is included because
it is good practice, only because a specific thing went wrong in a way that
produced no error and no failing test.

## Getting a fact right

Applies to any project. An agent states a remembered fact and a checked fact in
the same tone, and these are the four places that has cost the most.

### Read the failing tool's own source before believing its first line

When a tool fails, find the code that printed the message and read the branch
around it. The first line of the output is the line you will believe, and it is
often a warning that prints on every run. Installed dependencies are readable:
the compiled source is sitting in the package directory, and a grep for the
exact string lands on the function that emitted it.

**Why:** a message written for a person at a terminal is not written to be
diagnosed by somebody reading the log afterwards. Warnings and errors arrive in
the same stream, one after another, so the first alarming line reads as the
cause even when the cause is three lines below it. A fix built on that reading
addresses nothing, and it is plausible enough to get written into a document,
where the next person inherits it as fact.

*Four iOS builds failed under a line saying a certificate "is not validated for
non-interactive builds". An agent read that as the cause and wrote down that an
API key would remove the problem. The key was added and the next build failed
in exactly the same place. The source showed the line was a warning printed on
every non-interactive run whatever the account held; the failure was the throw
after it, because no certificate existed at all, and the branch that creates one
is reachable only when a person can answer a prompt.*

### Probe a deployed backend as its weakest caller

After putting a schema, a policy, or a permission change onto a managed
backend, make the request an outsider would make, holding the least trusted
credential the platform hands out, and read the answer. Run the platform's own
linter or advisors as well. The platform grants defaults your files never
mention, and its linter knows those defaults.

Treat a revoke as unfinished until the probe refuses you. A role can hold the
same permission twice, once granted in its own name and once through a group
that every role belongs to, and taking away one leaves the other working.

Skip the probe for a server assembled only from files in the repository, where
nothing grants anything around you. A local test that runs the same files
already covers it, which is also why such a test cannot cover the managed
platform: the platform's defaults are not in the files the test applies.

**Why:** what protects the data is the state live on the server, and the file
in the repository only describes the part of it you wrote. Reading the file
cannot find what the platform added around it.

*A cleanup job written for the scheduler, which never checks who is calling,
turned out to be runnable by a signed out request holding only the key that
ships inside the app, because the platform grants execute on every function to
the anonymous role by default. Revoking that role closed one function of six.
The other five still answered, because they also held the permission through a
group grant, and only the probe showed the difference.*

### Take reference data from its publisher, and check your reading of it back

When a feature needs published figures, e.g. a standard, a rate or a reference
table, fetch them from the body that publishes them and generate the code from
the file. Do not type them in, do not take them from a blog post or a package
that copied them, and do not produce them from memory. Then check your
arithmetic against something in the same file that you did not use: most
published tables carry both the parameters and worked examples, and the examples
are a test suite the publisher wrote for you.

Measure any shortcut rather than assuming it. If you thin the data to fit it on
a device, compute the error the thinning causes, in the units the user will
see, and put the number in the file.

**Why:** figures like these are the part nobody reviews. They look plausible
whatever they say, they are wrong in a way no type checker or test can notice,
and a person acts on them. Generating from the source makes the whole set
correct or obviously broken, never quietly half right.

*A baby app added growth percentiles from the WHO standards. Generating from the
published spreadsheets rather than transcribing turned up two things nobody
would have guessed. The length table steps 0.67 cm at exactly two years, because
the standard switches from measuring a child lying down to standing up at that
age, so interpolating across it hands a two year old a wrong length. And
sampling the tables weekly to save space, which looked obviously safe, put a
newborn's measurement out by 0.14 of a z-score, which is five percentile points,
because the curves move fastest in the first fortnight. Both were found by
measuring rather than by reading.*

### Never let an API's refusal look like a finding

Applies to any script that reads an API to answer a question you will act on.
The server answers three ways and the script has to tell them apart: a real
answer, an empty answer, and a refusal. Most rate limited responses and most
errors decode into an empty list, and an empty list is usually the interesting
finding, so a refusal becomes the conclusion.

Make the fetch return an error that is not empty, take those rows out before
scoring rather than counting them as zero, and say in the write up that you did.
Cache every successful response to a file, so a missing cache file is how you
find the failures. Then slow the script down and run the failures again, one at
a time. Test the check itself against something whose answer you already know.

Skip it when the answer is cheap to correct and somebody would notice it being
wrong straight away.

**Why:** research gets used to make a decision and is then thrown away, so a
wrong number in it is never found later, which is not true of a wrong number in
code.

*A script checked twenty eight possible product names against an app store and
reported seven with no competing product. Seven of those lookups had been
refused for making too many requests, and the script wrote each refusal down as
an empty result. A domain lookup in the same work returned "available" for every
domain including google.app, which is registered.*

## Finding a fault you cannot reproduce

Applies to any project with users on machines you do not own.

### Get evidence from the device and the server before reading the code

Applies when a symptom is reported from a real device and you cannot reproduce
it. Before opening the file you think is at fault, collect two independent
records of the moment it happened: what the person saw, and what the system
received. A screen recording pulled apart frame by frame is a log of the user
interface with timestamps on it. A server or platform log for the same minute
says which requests actually arrived. Line the two up on the clock, converting
the time zone rather than assuming it.

The most valuable thing either gives you is usually an **absence**. A request
that is not in the log did not happen, so everything downstream of it is
innocent, and that eliminates more of the search space in one query than a day
of reading eliminates.

Skip it when you can reproduce the fault yourself, where a debugger beats both.

**Why:** a plausible reading of the code is not evidence, and you cannot tell
your plausible readings apart from your correct ones. Bugs on a device that you
cannot run are exactly where that gap is widest, and each wrong guess ships a
build, costs the person another test, and moves nothing.

*A join screen hung and had been "fixed" three times, each time in the network
code. The video at ten frames a second showed the screen replacing itself in the
frame after the checkbox was tapped, with the button never pressed. The server
log for that minute showed the call had never been made. Between them they ruled
out the whole network path in two queries and pointed at four lines of screen
state. Every previous fix had been to code that was never reached.*

## Checks that prove something

Applies to any project.

### Test the file that gets applied, not the one that describes the result

Where a schema is kept both as a whole file and as a series of migrations, point
the test harness at the migrations too. Applying the whole file proves the
description is valid and proves nothing about the steps anybody actually runs.

**Why:** the two drift in different ways. The whole file is what people read and
so it gets read often; the migrations are what the database gets and they are
run once each, usually by hand, usually against the one database that matters. A
migration that cannot run is a change that silently did not happen, and the
harness reports green either way.

*Two migrations could not be applied to a test database at all. Both granted
permission to a role that the test harness never created, because nothing in the
main schema file had ever needed that role, so both stopped at that line. Nobody
knew, because the harness applied the main file and stopped. Adding one line to
the harness made both migrations testable and the first test run over them
found nothing wrong, which is the point: the value was that being wrong would
now show.*

### A check that reads a derived field reports faults that are not faults

Where a lint, a rule or an automated review decides whether something is allowed,
have it read the column a person filled in rather than a field the build worked
out. Where it needs a fact the generated data does not carry, have the generator
emit that fact as its own field, and say in a comment that the check is the only
thing that reads it.

**Why:** a derived field answers the question the runtime asked, and a check
asking a different question gets a confident wrong answer. The report then fills
with faults that are not faults, and whoever reads it learns to skim, which is
worse than having no report.

*A writing rule barred pronouns for the baby and allowed "she" and "her" where
the article was about the pregnant person. It decided which was which from a
field the generator works out to pick which reader is served the article, and
that field collapses "about the baby and the pregnant person" to "about the
baby". 299 of 633 articles were about a pregnant woman and read as articles
about a baby, so the rule reported 121 faults where 14 were real. An agent
reviewing the writing was handed that report and told not to repeat what it had
already found.*

### One condition must not guard two rules

Give each rule its own condition, even when the two conditions are identical
today.

**Why:** they are identical until one of them changes, and then the other rule
stops running with nothing to show that it has. Widening a condition to fix one
rule silently switches the other one off, and a rule that reports nothing looks
exactly like a rule with nothing to report.

*One condition guarded two writing rules: pronouns, which an article about a
pregnant woman is allowed to use, and gendered words for the reader, which no
article may use. Widening it so 299 articles could say "she" also let "mum" and
"dad" through on all 299. Splitting it took the reported count from 8 to 19, and
none of the 11 was new.*

### Measure a second reviewer by what it closes, not by what it finds

Where you add a second agent to check the first, count how many of its
disagreements the pipeline settles without a person. Where the answer is none,
the second agent is generating work rather than removing it, and what it was
worth keeping is usually a question the first agent was never asked.

**Why:** a second opinion feels like more rigour and reads like more rigour in
the report. It is only worth its cost when disagreement resolves somewhere other
than a person's inbox. Two agents that disagree hand a person every
disagreement, and that person now has to arbitrate between two things that
cannot explain themselves.

*A review pipeline ran a sceptic asking whether each finding was true, then a
parent's advocate that saw the sceptic's verdict and asked what the finding cost
the reader. It disagreed on 46 of 228 findings and all 46 went to the owner, so
it produced almost the whole queue and settled none of it. What it caught that
mattered was a real fault thrown away because the wording offered to fix it was
poor, three times out of 228. That is a second question rather than a second
agent, so the sceptic now answers on the fault and on the wording separately.*

### A finding has to quote the thing it is about, and the pipeline has to check

Where an agent reports a fault by quoting the words at fault, have the step that
collects the findings look for each quote in the artefact it names, and mark the
ones it cannot find.

**Why:** two different things produce a quote that is not there, and both are
worth catching before a person reads them. The agent can misquote, and a finding
built on words nobody wrote is worth nothing. Or the artefact moved after the
agent was given its copy, which means somebody has already fixed it. Neither is
visible from the finding itself.

*89 source addresses were rewritten across a content library while five review
agents were reading a snapshot taken an hour earlier. Fourteen of the rows they
were reviewing changed under them.*

## Work that looks finished and is not

Applies to any project. Each of these renders, compiles, and does nothing.

### What the screen says it is doing must be the thing making it do it

Applies wherever a screen shows progress, a wait, a spinner or a status: work
in flight, a save, a sign in, an upload. Derive what is drawn from the same
value that starts the work, so the two cannot say different things. Not two
expressions that happen to agree today, and not two booleans built from
overlapping conditions, which is one value read twice.

The test to write is the property rather than the case: for every combination of
the inputs, anything drawn as a wait is work genuinely running. It is a loop
over a handful of booleans and it catches the whole class.

Skip it where the screen has one state and no work behind it.

**Why:** when the two drift, the screen says it is busy while nothing is
happening, and there is nothing to see from anywhere. No request is made, so the
server log is empty. Nothing throws, so the crash reporter is silent. No timer
fires, because the timeout was armed by the code that did not run. It is
indistinguishable from a slow network, so it is reported as one, and every
attempt to fix it is spent on the part that was working.

*Joining a household needed the parent to agree and then press a button. The
work was gated on the press and the display was not, so ticking the agreement
box drew "Checking the invite." over a screen where nothing had started, with no
button and no way on. Three rounds of fixes went into the network path, which
had never been reached. The fix was one function returning one value, and a test
that walks all sixty four combinations and asserts the two agree.*

### A limit enforced at one door is not a limit

When something is capped, e.g. a free tier, a quota or a maximum of three, find
every route that reaches the thing being capped and put the check on all of
them, or put it somewhere all of them pass through. The button that a person
thinks of as "the way to make one" is rarely the only way; a link from
elsewhere, a deep link, a notification, a second screen offering the same
action, each is a door.

Test it by naming the doors out loud before you write the check, and again
after. If you can only name one, you have not looked.

**Why:** the cap is usually the business model or a safety rule, so a way round
it is not a cosmetic bug. It also fails silently and asymmetrically: the person
who found the side door never sees an error, so nobody reports it, and the
numbers just quietly stop matching the rule you wrote down.

*An app allowed one reminder without a subscription. The check sat on the Create
button, which said so and sent an unsubscribed parent to the paywall. Two other
screens offered "Remind me about this" and pushed straight to the same form with
a title filled in, and the form never asked. Both had been there for weeks. The
fix moved the check to the screen every route lands on, so the door is the
screen rather than the button.*

### A rule in a list nobody has to obey is not a rule

When several screens do the same kind of thing, do not write the rules down as
a list of bullet points and trust each screen to follow them. Put the rules in
one component the screens have to go through, and write the document as the
explanation of what that component does. A screen that cannot go through it
either gets brought into the shape or gets a written reason.

The test is whether a new screen gets the rules by existing, or by somebody
remembering to read a list first. The second one fails quietly, and it fails on
the oldest screen, which was written before the list.

**Why:** a list of rules is checked by whoever happens to read it. A component
is checked by the compiler. The gap between them is invisible in the code and
obvious to the user, because the odd screen out is the one they notice.

*The rules for charts were nine bullet points in the shared spec, and the app
had five places that drew a chart. Four obeyed most of them. The oldest was
flat bars in the app's accent colour with no word beside the numbers, nothing
to tap, and both of its charts on screen at once, which is every rule on the
list broken at the same time and nobody had noticed for months. Moving it into
the shared component also turned up a bug in it that no test could have caught,
because no test called it: a total was computed in milliseconds and printed by
a formatter that takes minutes.*

### Refuse to generate a degraded row rather than shipping it

Applies to any generator that turns an authored source, e.g. a spreadsheet, a
content file or a schema, into what the product ships. When a row cannot be
resolved in full, stop the build and name the row. Do not emit the row with the
unresolved part left out, and do not patch the gap at runtime with a fallback
that looks like the real thing.

Skip this only where the missing part is optional by design and the output says
so where a person reads it.

**Why:** a degraded row passes every check that a whole row passes. The build
is green, the screen renders, and the gap is one grey line inside one article
that nobody opens on purpose. A build that fails names the fault on the day it
is introduced, and a fallback written at runtime hides it for as long as the
product lives.

*A content generator looked a source's name up on one sheet and never read the
link column beside the name in the same row. 305 citations on 200 articles
shipped with no address and were drawn as grey text a parent could not tap,
while a working link sat in every one of those rows. A runtime map of twelve
names to organisations' front pages was added to cover the gap, so a parent who
tapped one landed on the NHS home page. Nobody noticed for a month, because
nothing failed.*

## Documents that drift from the code

Applies to any project that tells people what it collects.

### A document that describes what you collect goes wrong the day collection changes

Applies to a privacy notice, a data map, a risk assessment, a store data form,
and anything else that tells somebody what a system takes.

When a change starts collecting something, or stops, change every one of those
documents in the same commit. Then make something fail if a later change does
not: render the notice from one file, and have a test compare the copies and the
headings against it.

**Why:** code that starts sending something new looks the same in review as code
that does not, so a document describing the old behaviour keeps passing every
check you have. Nothing in a test run, a type checker or a linter knows what a
notice claims. The gap between the two is invisible until somebody reads both,
and the person most likely to read both is a regulator or a store reviewer.

*A measurement shipped that posted which setup screen somebody reached. Three
documents said the product collected nothing beyond the records it already held:
the privacy notice shown to every user, the risk assessment, and the data map,
which had no row for the new measurement at all even though the project's own
rules required one. All three stayed wrong for a day and were found by an agent
reading them for a different reason. Correcting the notice cost nothing, because
nothing had shipped to a store yet. The same correction after launch shows the
notice again to everybody who has already read it.*

## Data that leaves the device or outlives the session

Specific to apps that sync, queue, migrate or hold the only copy of something.
All of these fail with no error, which is why they are worth writing down.

### Decide what a queue does when the server says no

Applies to any retry queue that sends work to a server: an upload queue, an
outbox, a job runner. The server can answer in three ways, which are yes, no,
and nothing, and and "no" needs its own path before the queue ships. Retrying a
refusal gets the same refusal, and if the queue stops on failure, one refused
entry silences everything behind it for ever. Move a refused entry aside where
it can be inspected, tell your error reporter which entry and why, and let the
rest flow. Keep retrying only the answers that might change: timeouts,
connection drops, server errors.

Do not silently discard the refused entry if the client holds the only copy of
it. Parking is not deleting.

Skip it for queues whose entries are independent, where one failure blocks
nothing, and for fire-and-forget work that is allowed to be lost.

**Why:** the failure wears the wrong costume. Nothing crashes, reads keep
working, and the client looks healthy from every angle; the only symptom is
that the other side never hears from it, which gets reported as the other
side's problem. And it survives updates if the queue is persistent, so
shipping a fix changes nothing until the queue itself is unblocked.

*Two carers shared a baby app household. One batch in the inviter's upload
queue was refused by the server's row policy, the queue's failure path was
"stop and retry next time", and the queue was persistent storage. Everything he
logged for a day queued behind that one batch, through an app update whose
whole point was fixing sync, while his phone pulled her records perfectly. It
was reported as "I receive her data, she doesn't receive mine", and no log on
his phone said anything at all.*

### Never widen a record of what has been sent

Applies to anything that tracks what has already gone out: a sync cursor, an
uploaded set, a delivered list, a cache of what the other side holds. Write into
it **only the specific things you have confirmation for**. Never write a whole
collection because the thing you have in your hand happens to be one.

The trap is a merge. A function that folds arriving data into local data usually
returns the merged whole, because that is what the caller wants to store.
Marking that whole as sent is one keystroke and it is a lie: most of it never
went anywhere. Take the ids from the wire, not from the merge.

The second half of the same rule: record the objects that actually ended up in
your state, not the ones your merge produced on the way. If those are two
different sets, the record describes things that do not exist and every
comparison against it comes out wrong.

Skip it where nothing is compared against the record, or where re-sending is
free and re-sending everything is the design.

**Why:** it fails in the direction with no symptom. A row wrongly marked sent is
never sent again, so there is no error, no retry and no log line, because the
data simply is not there, and only the other side can see the absence. It also
survives restarts if you persist it, and it gets worse the more the user did
before the first sync, which is the opposite of how bugs usually announce
themselves.

*Two carers shared a household. A pull merged the other carer's records into the
phone's own, and the whole merged collection was marked as already uploaded, so
everything the parent had written before their first pull was silently never
sent. The same line also filled the record from one merge while the state
received another, so the arriving rows never matched either and were pushed
straight back: the server log showed a hundred and fifty two writes for three
records. It was reported as "I get her data, she doesn't get mine".*

### Fence demo data out of every path that leaves the device

Applies the moment an app gains both a demo mode and any channel out, meaning
sync, a backup, an export or analytics. Give demo records an identity a fence
can test (one prefix on every id is enough), and check it at each boundary in
both directions: nothing demo leaves the device, and nothing demo is adopted
from outside, because by the time the fence exists somewhere upstream may
already be holding leaked demo rows.

Skip it while the app has no channel out, but write the identity in from the
start, because retrofitting a marker onto records that are already mingled is
the expensive version of this rule.

**Why:** demo data is built to be indistinguishable from real data, so every
system downstream treats it as real. The failure is not embarrassment; it is
fictional records with real-looking authors inside somebody's actual account,
and cleanup that needs a human to decide row by row what is real. Where the
receiving side validates, it is worse in the other direction: the demo rows
are the ones that fail validation, and whatever error path they hit, real data
is behind them in it.

*A baby app's demo added a sample baby with three hundred records by invented
carers. Sync had no fence, so one phone pushed the demo into the couple's real
household, and the other phone adopted "Ada (3 weeks old)" as a real baby next
to their actual child. The demo rows whose baby had not gone up yet were
refused by the server's row policy, and those refusals dammed the upload
queue, which is what turned a cosmetic leak into a day of one parent's records
never reaching the other.*

### A one-off migration only ever reaches what was there when it ran

A field added to a record later is usually filled in by a migration that runs
once, over whatever is on the device at startup. That covers the rows that were
already there and nothing else. Records arrive by more than one route: a sync
pulls rows written by another device, an import reads a file, a restore puts
back a backup made before the field existed. None of those go through the
startup migration, and every one of them lands a record with the new field
empty.

Prefer deriving the value where it is read to filling it in where it is stored.
A reader that can work the value out from what the record already carries is
correct for every route, including the ones nobody has thought of yet. Fill the
stored field in as well if it is worth the write, but do not let anything depend
on the fill having happened.

**Why:** it fails as absence rather than as an error. Nothing throws, no test
covering the migration goes red, and the feature that reads the field shows its
empty state, which is the state it is supposed to show when there is genuinely
nothing. Whoever looks at it sees a screen behaving correctly for the data it
can see, and the data it cannot see is sitting in the same record two fields
along.

*A baby app stored a feed's amount as numbers, and had once stored it only
inside the sentence shown in the records list: "Formula, 120 ml". A migration
read the sentence and filled the numbers in at startup. Rows that came down
from the server arrived with the numbers empty, because they had been written
before those columns existed, and the startup migration had already run. Two of
the four feeding charts had nothing to add up and drew "not enough records yet",
which to a parent who had been feeding their baby all fortnight read as two
charts that were missing. The amount was in the record the whole time.*

### Absence cannot mean intent where a loss looks the same

A system that decides something was deleted because it is no longer present has
also decided that every way of losing it is a deletion. The two are the same
observation. A device that dropped its copy, a state that was rolled back, a
list that was rebuilt from the wrong source: each of them looks exactly like a
person deleting things, and the system will act on it, at the scale of whatever
was lost.

Record the intent instead of inferring it. When a person deletes something, put
that fact somewhere the sync can read. A record that disappears without that
mark is a record this device has lost, and the right response to a loss is to
fetch it again, not to tell everybody else to drop it too.

Bounding the inference by scale instead is tempting and is not good enough. The
rule "one missing item is a deletion, everything missing is a fault" does
nothing when a fault takes half of something, and it swallows a person deleting
the last item of a kind. It is a guess standing in for a fact that is available
for the asking, and the cost of asking is one list.

Record it where the removal happens, in the same expression, so a call site
cannot do one without the other. Keep the record until the instruction to delete
is somewhere durable, then drop it. Sending the same deletion twice is usually
harmless; losing one is not, so err towards keeping it. And clear the record
when the device stops being part of whatever it was syncing with, because those
identifiers now name somebody else's data.

**Why:** the inference is invisible in the code and correct in every test. The
tests delete one item and assert it propagates, which is the behaviour anybody
would write, and the failure only appears when something upstream loses data,
which no test of this component simulates. The blast radius is also inverted
from the usual: the worse the upstream fault, the more the sync destroys.

*A baby app's phone rolled its own state back to an older copy of itself because
one screen handed a whole state object to a merge-style updater. That is a bug
in one screen and would have cost one device its records. The sync then read
every missing record as a deletion and wrote 28 tombstones, which took the same
records off the server and off the other parent's phone. The device that had
lost its copy is the one that got to decide the household had none. The first
fix shipped was the scale rule above; the owner rejected it in one line, on the
grounds that it was still a guess, and he was right.*

## Numbers and advice a person acts on

Specific to products that show people figures about themselves, or tell them
what to do about their health, money or safety.

### A number on a chart has to be sayable as a sentence

Before drawing an aggregate, say out loud what one bar means, in a sentence a
user would recognise. If the sentence needs a clause explaining what was added
to what, the bar is wrong. This catches the common fault, which is pouring a
long period into a short cycle and then plotting the total: adding a fortnight
of something into the hours of one day gives a bar nobody can place in time.

Where the fix is an average, say what it divided by, and prefer the days that
carried a record to the length of the window. Someone who logged three days out
of fourteen otherwise sees every bar cut to a fifth of what actually happened,
which reads as a fact about them rather than as a gap in the record.

And gate the chart on what actually varies. If the bars are hours of the day,
"is there enough to draw" is a question about how many separate days
contributed, not how many bars came out non-zero, or one busy afternoon draws a
pattern.

Skip this for a plain series, where each point is one thing that happened and
the axis is time.

**Why:** an aggregate that means nothing still looks like a chart. It has axes,
gridlines, a shape and a caption, so nobody looking at it asks what a bar is,
and the person it is drawn for reads a fact off it that was never in the data.

*A baby app charted when a baby sleeps by adding a fortnight of recorded sleep
into twenty four hourly buckets. The axis showed fourteen hours against two in
the morning. Nobody caught it for weeks, because a bar of fourteen hours on a
chart about sleep looks like a lot of sleep rather than like a sum of a
fortnight, and the sentence was never said out loud.*

### Do not ship a number the field's own guidance says not to give

Before building something that predicts, scores or targets a person's behaviour,
read what the field says about that number existing at all. Sometimes the answer
is not "we cannot predict it accurately enough" but "nobody should be given this
figure", and the two have completely different consequences: the first is a
reason to improve the model, the second is a reason to delete the feature.

Where you do keep a prediction, say how it was worked out, in the words of the
thing being predicted rather than in the words of the algorithm, and measure it
against what actually happened so the screen can say how often it has been
right. A prediction nobody has checked is a number you are asking to be
believed.

**Why:** an interface makes anything it displays look like a target. People
change their behaviour to meet a number an app shows them, which is the whole
reason to show one, and that is exactly why showing the wrong one does harm that
no amount of accuracy would have fixed.

*An app was asked for predictions on two logs. For one of them the published
guidance was explicit that watching the clock leads people to act against their
own interest, and that the normal range at a single age is wide enough that any
single figure is wrong for most of the people reading it. That prediction was
dropped and the reasoning written into the spec, so it does not get proposed
again. The other one was kept, given a plain sentence saying how it was worked
out, and made to report how often it had actually held.*

### Report the published guidance, do not weigh the evidence behind it

Applies to any article, notice or answer an agent writes on a subject where a
health service, a regulator or a standards body has published guidance for the
reader. Say what the guidance is, name the source, and keep the source's own
terms. Say a thing is uncertain only where the source says so. Do not set
evidence about one situation against guidance about another.

Skip this where the reader is the expert and has asked for the evidence, e.g. a
clinician reading a review of the literature.

**Why:** an agent that weighs the evidence writes in the same confident tone
whether it is right or not, and the result reads as more authoritative than the
guidance it disagrees with. A reader cannot tell a considered dissent from a
pattern of hedging words, and the cost of being wrong lands on the reader. The
guidance exists because somebody with the standing to weigh the evidence
already did.

*An article for pregnant readers was titled "Lying on your back, and why the
advice is not settled". It said the advice to sleep on your side "rests largely
on caution and expert opinion rather than strong evidence", and cited a review
of exercise positions and a study of yoga poses. The health service's advice is
about sleeping, is based on stillbirth studies, and applies from 28 weeks. The
article was live from week 24 to week 40 and arrived on the reader's home
screen unasked, while the article carrying the actual advice expired at week
27.*

## Code on a path that runs constantly

Specific to apps with a user interface on one thread, which is most of them.

### Keep the cost of a change proportional to the change

Applies whenever you write something that runs on every change, every render
or every event: a save, a comparison, a subscription to shared state, a
listener. Ask what it costs when the data is a hundred times bigger than it is
today, and make the cheap check first. Comparing two things by identity costs
nothing and answers most of the question; reading, hashing or serialising them
costs more the more there is.

The same applies to anything you add that runs on its own. A crash reporter,
an analytics client or a logger is not free and does not sit outside the app:
read what its defaults turn on before shipping it, and turn off what you did
not ask for.

Skip it where the data is bounded and small by construction, e.g. a fixed list
of settings.

**Why:** the user interface is drawn on one thread in most frameworks, and
input is handled on that same thread, so anything expensive you do on a common
path is felt as taps that do nothing and scrolling that sticks. It never
appears as an error, no test fails, and the app looks correct in every
screenshot, so the report that comes back is that the whole app feels broken
rather than anything pointing at what you wrote.

*Saving compared the new state to the old by turning every field into text,
which meant a diary, every log and every stored photo path were serialised
each time anything at all changed, including a message appearing at the bottom
of the screen. It had been survivable until a crash reporter with performance
tracing on by default and an analytics client flushing every ten seconds were
added on top of it, and then taps across the whole app started being dropped.
Comparing by identity first, and turning off the instrumentation nobody asked
for, cost four lines.*
