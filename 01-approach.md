# The approach

> Rules for an agent writing code, each saying when it applies and when to skip
> it.

Rules for an agent working on a codebase with occasional human supervision. Each
rule says when it applies, what to do, and why it exists.

The rules are meant to be bent. When one does not fit the situation, say in the
chat that you are skipping it and why, then carry on.

## Get evidence from the device and the server before reading the code

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

## Verify claims when being wrong is expensive

Run a command when a fact decides what gets built, blocks somebody, or would
take more than a few minutes to undo. Version numbers, rate limits, an API's
behaviour, file counts and whether a commit landed all qualify. Skip the check
when the fact is cheap to correct and you would notice the mistake straight
away.

When the user disagrees with a fact you stated, run the check rather than
restating the fact.

**Why:** your context gets summarised as a session runs, so a version you
remember may be a compressed guess rather than something you read. You state a
remembered fact and a checked fact in the same tone, so the user cannot tell
them apart.

*An agent told a user to update a test client from the app store. The store
version was three releases behind and no update existed.*

## Read the failing tool's own source before believing its first line

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

## Agree a spec before building anything you cannot describe in a sentence

Write the spec first when the request is a feature rather than a change, and get
agreement on it before writing code. Add the feature to the list in `spec.md`,
then write it out in full: what somebody can do when it is finished, how anybody
will know it works, what is out of scope, and what is undecided. Do not say how
to build it, because choosing the approach is the work. The shape is in the
second document.

Read the feature list before proposing anything. The thing you are about to
build often exists already under a different name.

Skip it when the change fits in a sentence and somebody could undo it in an
hour.

**Why:** given a one line request you invent the rest of the scope, confidently
and in detail, and somebody finds the invented parts weeks later by using the
app. The heading that earns the page is how anybody will know it works, because
without it you are finished when the code compiles.

## Work in small steps, and count before a wide change

Do one thing, check it, then do the next. Before a change that looks like it
touches many files, count the files it actually touches and say the number
before starting.

**Why:** a large change that compiles is not a large change that works, and you
produce plausible code faster than anybody can review it. Counting first often
shows the job is smaller than it looks.

*Moving a storage layer looked enormous until somebody counted. Six files wrote
to storage and thirty seven only read the data.*

## Decide what a queue does when the server says no

Applies to any retry queue that sends work to a server: an upload queue, an
outbox, a job runner. The server can answer in three ways, which are yes, no, and nothing, and
and "no" needs its own path before the queue ships. Retrying a refusal gets the
same refusal, and if the queue stops on failure, one refused entry silences
everything behind it for ever. Move a refused entry aside where it can be
inspected, tell your error reporter which entry and why, and let the rest flow.
Keep retrying only the answers that might change: timeouts, connection drops,
server errors.

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

## Never widen a record of what has been sent

Applies to anything that tracks what has already gone out: a sync cursor, an
uploaded set, a delivered list, a cache of what the other side holds. Write into
it **only the specific things you have confirmation for**. Never write a whole
collection because the thing you have in your hand happens to be one.

The trap is a merge. A function that folds arriving data into local data usually
returns the merged whole, because that is what the caller wants to store. Marking
that whole as sent is one keystroke and it is a lie: most of it never went
anywhere. Take the ids from the wire, not from the merge.

The second half of the same rule: record the objects that actually ended up in
your state, not the ones your merge produced on the way. If those are two
different sets, the record describes things that do not exist and every
comparison against it comes out wrong.

Skip it where nothing is compared against the record, or where re-sending is
free and re-sending everything is the design.

**Why:** it fails in the direction with no symptom. A row wrongly marked sent is
never sent again, so there is no error, no retry and no log line, because the data
simply is not there, and only the other side can see the absence. It also
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

## Write tests where being wrong is permanent

Write tests before changing code that saves, deletes, migrates or syncs data
that has no second copy. Do the same for money and for anything a user cannot
redo. Skip tests where a mistake shows up on screen immediately and costs a
reload.

**Why:** a passing test is the only evidence you did no harm to code whose
failures are silent.

*Tests written for one file before changing it found a bug that had been live
for weeks. After certain reads the save skipped a field it wrongly believed was
already stored, and the field was lost with nothing reported.*

## Run a guard test as a real user, and watch it fail once

Applies to any test that guards something rather than computes something, e.g. a
permission, a paywall, a login check, or a rule about who may read which row.

Say who the test runs as, and make it somebody a real user could be. Then delete
the thing the test protects, show the test going red, and put it back.

**Why:** a test proves something about the position it runs from, and a test
that runs with more authority than a user has measures nothing while passing.
The same shape turns up as a file permission checked while running as root, an
API tested with an admin key, a paywall tested by code that sets the
subscription flag itself, and a login check tested from inside the session it
guards. A guard test you have never seen fail is not evidence that the guard
works.

*An agent wrote thirty three checks for the rules about who may read which row
and ran every one as the database administrator, who is exempt from those rules.
They all passed, and they would have passed with the rules deleted. Rerunning
them as an ordinary signed in user found that anybody with an account who
learned a household's id could add themselves to it and read another family's
child health record.*

## Probe a deployed backend as its weakest caller

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

## Keep the cost of a change proportional to the change

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

## What the screen says it is doing must be the thing making it do it

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

## Fence demo data out of every path that leaves the device

Applies the moment an app gains both a demo mode and any channel out, meaning sync, a
backup, an export or analytics. Give demo records an identity a fence can test
(one prefix on every id is enough), and check it at each boundary in both
directions: nothing demo leaves the device, and nothing demo is adopted from
outside, because by the time the fence exists somewhere upstream may already be
holding leaked demo rows.

Skip it while the app has no channel out, but write the identity in from the
start, because retrofitting a marker onto records that are already mingled is the
expensive version of this rule.

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

## Say what the screen shows when the supply runs out

Anything that deals items out of a finite supply, e.g. a list, a feed, a queue or a
schedule, eventually asks for more than there is. Decide what happens at zero
and write it into the spec, in the same words for the person and for the code.
Running short and running out are different states and usually want different
answers: short is often fine and even intended, whereas nothing at all is a
screen with nothing on it, which no rule you wrote was trying to produce.

When you do put a floor under it, make your own verification count the floor
separately from real supply. A fallback that reports as a delivery turns your
one honest measurement of the shortage into a green light.

**Why:** every rule in the dealing logic can be right and the result still be a
blank screen, because nobody wrote the rule for zero. It shows up late, only for
the users who got furthest, and only after enough time that it looks like a
regression rather than the design.

*An app dealt reading material across a journey, taking from what was written
for the current stage and topping up from anything still relevant that the
reader had not already been given. Both rules were correct. At two stages in the
middle, a reader on the faster setting had been given everything relevant
already, so the section was empty, and it was emptiest for the readers who had used the app most.
The fix was one rule, that a stage may be short but never empty, and one line in
the report so the two stages stay named as a content gap rather than
disappearing behind the floor.*

## Give demo data enough history for the comparison, not only for the display

Sample data has to reach back far enough to exercise every part of the feature
it exists to show. A view that compares one period against the period before it
needs two periods behind it, not one, or the half that does the comparing never
appears on the sample anybody looks at.

**Why:** demo data is where somebody meets a feature before they have data of
their own, and it is also what the people building it look at every day. A
sample that fills the chart and leaves the sentence under it blank shows the
feature with its conclusion missing, and it looks finished to everybody.

*A nappy colour chart drew a fortnight of bars, and under them a line saying
what had changed since the fortnight before. The sample carried exactly one
fortnight, so the bars were there every time and the line was never there at
all. Extending the sample to four weeks, with a change in it that the age
actually produces, made the whole feature visible.*

## A limit enforced at one door is not a limit

When something is capped, e.g. a free tier, a quota or a maximum of three, find every
route that reaches the thing being capped and put the check on all of them, or
put it somewhere all of them pass through. The button that a person thinks of as
"the way to make one" is rarely the only way; a link from elsewhere, a deep
link, a notification, a second screen offering the same action, each is a door.

Test it by naming the doors out loud before you write the check, and again after.
If you can only name one, you have not looked.

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


## A number on a chart has to be sayable as a sentence

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

## Do not ship a number the field's own guidance says not to give

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


## When you move a rule into the model, move every copy of it

If you fix a rule by lifting it out of a screen into shared code, find every
other place that held its own copy and point those at the shared code in the
same commit. Search for the old shape, not the new name, because the copies are what
the code looked like before you touched it.

**Why:** the copy you did not update is now the only one that is wrong, and it
is wrong in the same way the original bug was, so the symptom comes back
looking like a fix that did not work.

*A screen's list-picking rule was extracted into the model and corrected there.
A second screen, which the first one links to, still had the original line
inline. It went on dealing a different list from the one the user had just
tapped through from, and the shared function it was supposed to be using sat one
import away.*

## Read a default from the list it belongs to

When a screen offers a set of choices, the one it starts on has to be read from
that set, not written down a second time somewhere else. Take the first entry,
or mark one entry as the default and take that. Never store the starting choice
as its own value beside the list.

Skip this where there is one option, or where the default is genuinely not any
of them, such as an empty state that has to be chosen out of.

**Why:** the two copies drift, and nothing fails when they do. The list is
edited by whoever adds a choice, and the default is edited by nobody, because it
is somewhere else and reads as settled. What the person sees is a screen that
opens on the wrong thing, which nobody reports as a bug because it looks like a
decision somebody made.

*An app's charts came from a table of which charts each tool offers, and the
component that drew them opened on a hard coded 'count'. A movement counter was
given a time of day chart, put first in its list because it was the one the tool
existed for, and the tool went on opening on the count. The list said one thing
and the screen did another, and both were checked in and both looked correct.*

## A floor under a filter is a filter that never runs

When a rule keeps the best N of something and a second rule guarantees a minimum
of M, `max(M, N)` means the filter only ever decides anything when N is bigger
than M. Work out how often that is against the real data before writing it. If
the answer is almost never, the filter is decoration and the thing it was meant
to exclude is still getting through.

The fix is usually to separate the two jobs. Ranking decides the order. A gate
decides what is eligible at all, and a gate is applied before the floor and is
not overridden by it. A rule that says "prefer the good ones" and a rule that
says "never this one" are different rules and cannot be the same expression.

**Why:** the failure is silent and it looks like the opposite of itself. The
code reads as though the filtering happens, the test that covers it passes on a
pool large enough for the filter to bite, and in production every pool is
smaller than the floor. Whoever wrote it will defend it from the code, because
the code says what they meant.

*An app scored its optional suggestions and kept the better half, with a floor
of five so a thin week still had something to rotate through. Almost no week had
more than ten candidates, so nothing was ever cut. One entry assumed the reader
already had a child; it was rated the lowest relevance in a library of 623 and
was ranked last of the five, and it was still offered to a parent expecting
their first. Two people read the ranking code and agreed it was correct.*

## A one-off migration only ever reaches what was there when it ran

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

## Absence cannot mean intent where a loss looks the same

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

## A correct function called wrongly is where the data goes

Pure functions attract tests because they are easy to test, and the call site
that passes them the wrong argument attracts none. When something goes missing,
check the wiring before the logic: whether the value handed in is the current
one, whether the result is applied where it was meant to go, whether the
function is being called at the moment its assumptions hold.

The shape to watch for is a state updater that merges what it is given. Handing
one a whole state object, rather than a description of the change, writes every
field of it, including the fields that something else updated a moment ago.
Where the updater offers a form that receives the live state, use it, and treat
the other form as being for literal values only.

**Why:** the unit tests pass and keep passing, so the component looks proven and
suspicion goes elsewhere. The reasoning that hides it is real and sounds right:
"that function only touches its own rows, and here is the test." Both halves are
true and neither is about the argument.

*A baby app's sample data switch called a function that adds a demo family
alongside the real one. The function was correct, and a test over every
collection said so. The screen called it with the state from its own last
render and handed the result to a merge, so tapping the switch rewrote the store
with values from before the last sync had landed. The test suite was green
throughout, because nothing tested the tap.*

## A number the user can also set is not a derived number

When a figure is worked out from other figures but the user can also type it
themselves, hold both: the derived value and whatever they typed. Show the
derived one while its inputs are filled in and theirs when they are not, and
never overwrite what they typed with a fresh calculation.

The same goes for a stock figure, meaning money in an account, items in a store,
or anything that goes down as well as up. Do not derive it by adding up a log of
additions. Keep it as its own number the user owns, let the log offer to move
it, and let them correct it.

**Why:** a derived figure has one source of truth and a stock figure has none
the software can see. Adding up the additions climbs for ever, is wrong the
first time anything is taken out, and is wrong in a way the user cannot fix
because the field is not theirs to edit.

*A pumping log offered a total made of the left and right amounts. A parent who
pumps into one bottle has only the total, so the field had to be typed as well
as calculated; holding one value meant switching between them lost what had
been typed. The stored-milk figure had the same shape and a worse failure:
summing the sessions would have told a parent they had two litres in the fridge
after a fortnight of feeding the baby from it.*

## Read a control's position from what the system acts on

When a switch tells someone whether something is happening, work out its
position from the same expression the code actually tests, and not from one of
the values that expression reads. Where the behaviour is "a or b", the switch is
on when either is true.

**Why:** a switch wired to one of its inputs tells the person a feature is off
while it is running. They cannot correct that, because the control they would
use to stop it already says it is stopped, and the only way to find out is to
notice the effect somewhere else.

*Two separate agreements let records leave a phone, keeping a backup and sharing
with a second carer, and the gate was written correctly as either one. The
settings switch labelled "Keep a copy online" read only the backup answer, so a
parent who had agreed to share and never asked for a backup saw a switch saying
off while every record they wrote was being uploaded.*

## An inferred answer stops being one when the question gets its own control

Where a program records a decision the person never made explicitly, on the
grounds that the screen explained what the button would do, that inference holds
only while the button is the only way to answer. The day the same question
appears anywhere else as a switch, the program has two ways of recording one
answer and they will disagree. Move the recording to the control and leave the
button doing the thing.

**Why:** two records of one answer means the person changes the one they can see
while the program keeps acting on the one they cannot. Where the answer has to
be demonstrable afterwards, such as a consent, an inference is not something
anybody can show a regulator or a court.

*Pressing Sign in wrote "a copy of my records may be kept", on the grounds that
the screen said so above the button. That was fair while signing in and keeping
a copy were one act. Settings later grew a switch for the copy, and the app then
asked one question in two places with only one of them a choice, while the
privacy notice described the answer as explicit consent that nobody had ticked.*

## Put a form's rules in the model, not in the form

The arithmetic a form applies, and the state a control holds, belong in plain
functions outside the component. Leave the component with the drawing and the
handlers. Then test the rules directly, with no rendering, no test renderer and
no mocks for the platform.

**Why:** rules written inside a component can only be tested by rendering it,
which drags in the whole framework, and so in practice they are not tested at
all. The rules are the part that is worth being sure about; the layout is the
part a person will look at anyway.

*Two side-by-side timers with a lock that starts and stops both took four
states and a rule about which press decides the direction. As component code it
was untestable in this project's setup, because importing it pulled in the theme, the
store and the device storage. Moved into a plain module, the same logic took
six tests, one of which caught a locked pair with only one side running
starting the stopped side instead of stopping the running one.*

## Offer every field on the edit form that the add form writes

Whatever the form for a new record collects, the form for changing an existing
one has to collect as well. A field that can be set once and never corrected is
worse than a field that does not exist, because the wrong value stays and the
person can see it sitting there.

**Why:** a value entered in a hurry is entered wrongly often enough that
correcting it is part of the feature, and a value people forget at the time is
one they will want to add later. Neither is possible when the edit form is a
subset of the add form, and nobody notices while the field is only stored,
because there is nothing on screen to be wrong.

*A nappy log asked for the colour when a record was written and did not offer it
when one was edited. Nothing was lost, because the save spread the old record
and kept the value, and nothing could be changed either. It went unnoticed for
as long as no screen read the colour back. A chart drawn from it made a wrong
colour visible and uncorrectable on the same day.*

## A rule in a list nobody has to obey is not a rule

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

## Look up a framework's defaults rather than the ones you know

Applies whenever you write against a framework that resembles one you already
know: a mobile toolkit that borrows the web's layout language, an ORM that
looks like SQL, a runtime that looks like another runtime. Find the value in
the engine that will run the code, meaning its source or its own reference rather than a
tutorial, for any default your code leans on without naming. Then name it,
because a stated value costs a word and cannot be remembered wrongly by the
next person either.

Skip it where your own code already sets the value, and where being wrong shows
up as an error rather than as a plausible result.

**Why:** a default you have wrong does not fail. It produces something that
looks deliberate, so nothing throws, no test fails, and the first person to see
it is a user on a device you do not have.

*React Native's default for flexShrink is 0 where the web's is 1. Two controls
side by side each asked for the width its longest option needed, the two came
to more than a phone is wide, nothing was allowed to give way, and the second
one was pushed off the edge of the screen. The eventual fix was one property on
each. The first attempt, made by changing a property rather than by reading
what the engine does with it, replaced the width with a maximum: a view with
only a maximum has no size of its own, so the label inside asking for a
percentage of its parent had nothing to resolve against and collapsed. That
shipped, and every dropdown in the app came back as an empty box with an
arrow.*

## When a fix for a layout fault fails twice, run the engine rather than read it

Applies to a fault you cannot see for yourself: a row that disappears, a panel
that comes out short, a control that lands off the screen, on a device somebody
else is holding. After the second fix that was reasoned out of the code and did
not work, stop reasoning about it. The layout engine is usually sitting in your
dependencies, so build it and lay out the real tree with the real numbers, or
write the smallest program that puts the same boxes somewhere you can look at
them.

Skip it while you still have an untried explanation you can check cheaply, and
wherever the fault reproduces on a machine you have.

**Why:** a layout engine is a program, and reading a program to work out what it
returns is guessing when running it is available. Each reasoned fix also costs
the person testing it a build and an hour, and the third wrong one teaches them
the screen is unfixable rather than that it is misunderstood.

**One trap to know before you start**, because it is invisible in the code and
every number in the arithmetic is right: a percentage size resolved against a
parent whose own size is decided by that same child. The parent has no height
until the child is measured, so the percentage resolves against the child's own
natural height and the child is clamped to a share of itself. Watch for it
wherever a wrapper was added between a panel and the box that used to size it,
such as one holding an animation.

*A bottom sheet had to open showing a grid of buttons and one row under it. It
measured both, passed the number down, and the row was missing on the phone every
time. Three fixes were reasoned out and shipped: pinning the row below the scroll,
then a fixed share of the screen, then a measured height. Each was correct on its
own terms and none of them worked, because the panel was capped at 92% of a
wrapper whose only content was the panel, so it was held at 92% of itself and 48
points came off the bottom of every sheet in the app. The measurement had been
right all along and was simply not being honoured. It was settled by compiling the
layout engine out of the dependency folder and laying out the real tree: 455 points
where 495 were asked for, which is exactly the missing row.*

## A size worked out from the text has to scale with the reader's text setting

Where a control's width or height is calculated from the text it holds, rather
than laid out by it, that calculation has to include the reader's own text size.
Multiply the estimate by the platform's font scale, and multiply any maximum by
it too, or the maximum reintroduces the same fault one size further up.

Prefer a real layout where you can have one. Reach for a calculated size only
where the size has to be stable, for instance a control that must not change
width when the choice inside it changes, because a control that resizes under a
finger reads as the screen moving.

Skip this where the text cannot scale, such as a fixed diagram label.

**Why:** it is invisible to everybody who builds the thing and permanent for
everybody who has the setting turned up, which is a large number of the people
an app about health is for. Nothing throws, nothing logs, and the code reads
correctly, because the arithmetic is right for the size the developer happens to
be looking at.

*A parent reported one dropdown in a baby app showing "Last 14 d…". The label
measured 149 points against a cap of 260, so reading the code found nothing
wrong, twice. They had Android's font size turned up, every label was drawn
larger to match, and a width worked out from a count of characters at 13 point
described a button that was not on their screen. The fix was to scale the
estimate and the cap, and to cap how far the label itself may grow, rather than
to stop it growing at all: turning scaling off fixes the truncation by ignoring
somebody who needs larger text.*

## Prefer a layout that cannot fail to one that has to be got right

When a control can be built either by floating it over the page or by giving it
its own room, take the room. Floating needs a stacking order on one platform
and a different property on the other, and it is clipped by any ancestor,
however far up, that turns out to clip its children. None of that shows up on
the machine you are building on.

Same for anything else with three ways to be invisible and no way to check them
from here: pick the version with one.

**Why:** you cannot see the result. Somebody else installs it, and a control
that renders behind the thing it is meant to cover looks like a control that
does nothing.

Where you keep the floating version anyway, know that a clip beats a stacking
order absolutely. No z-index, elevation or layer reaches out of an ancestor
that hides its overflow, so a list inside one is not painted behind something,
it is cut. Check the ancestors again every time the floating control moves, and
every time something is wrapped around it.

*A chart's chooser opened a list over the chart. It needed a z-index for one
platform, an elevation for the other, and every view between it and the screen
to leave its children unclipped. Pushing the chart down for as long as the list
is open cannot be clipped by anything, and the same page already opened a
folded section that way, so it was also the behaviour the user had met.*

*The floating version was kept, the ancestors were checked, and it broke six
weeks later anyway. A subscription fade was put around the chart, a fixed
height with its overflow hidden, and the chooser inside it lost its list. It
was read as a stacking bug and a build was spent on the stacking order before
anybody looked at the box around it.*

## Do not send somebody to a screen that is already open

Applies wherever a screen finishes and moves the user on: a form that saves, a
confirmation, a payment, a sign in. Go back to what they came from rather than
naming a destination. Most routers asked for a route that is already on the
stack put a second copy of it there rather than returning to the first, and
everything in that copy mounts again, so its effects run, its timers start, and
anything it opens on arrival opens a second time. Name a route only as the
fallback for arriving with nothing behind you, which is what a cold start from
a link looks like.

Where a screen opens something the moment it arrives, make that happen once for
the run of the app rather than once per mount, so it cannot double whatever
else lands on the stack.

Skip it where the destination cannot already be open, e.g. a route reachable
from exactly one place.

**Why:** a duplicate screen is indistinguishable from the right screen, so
nobody reports two of anything. They report a button that has to be pressed
twice, or a question that comes back after they answered it, and both read as a
control that does not work.

*An app moved itself into a new mode and finished by replacing that screen with
the home route, which was already at the bottom of the stack. Two copies of
home mounted, each asked the one question the app asks on arriving in the new
mode, and the question stacked. The first press dismissed one copy and revealed
the other. It was reported as a confirm button that had to be pressed several
times before it registered, and the button had worked the first time.*

## Take reference data from its publisher, and check your reading of it back

When a feature needs published figures, e.g. a standard, a rate or a reference table,
fetch them from the body that publishes them and generate the code from the
file. Do not type them in, do not take them from a blog post or a package that
copied them, and do not produce them from memory. Then check your arithmetic
against something in the same file that you did not use: most published tables
carry both the parameters and worked examples, and the examples are a test
suite the publisher wrote for you.

Measure any shortcut rather than assuming it. If you thin the data to fit it on
a device, compute the error the thinning causes, in the units the user will
see, and put the number in the file.

**Why:** figures like these are the part nobody reviews. They look plausible
whatever they say, they are wrong in a way no type checker or test can notice,
and a person acts on them. Generating from the source makes the whole set
correct or obviously broken, never quietly half right.

*A baby app added growth percentiles from the WHO standards. Generating from
the published spreadsheets rather than transcribing turned up two things
nobody would have guessed. The length table steps 0.67 cm at exactly two
years, because the standard switches from measuring a child lying down to
standing up at that age, so interpolating across it hands a two year old a
wrong length. And sampling the tables weekly to save space, which looked
obviously safe, put a newborn's measurement out by 0.14 of a z-score, which is five
percentile points, because the curves move fastest in the first fortnight.
Both were found by measuring rather than by reading.*

## Test the wording where the wording is the feature

When what makes a feature safe or unsafe is how it is phrased rather than what
it computes, write tests on the phrasing. Assert the words that must never
appear, and assert the sentence that must always appear. Include the kind words
in the banned list, not only the harsh ones.

**Why:** a numerically correct feature can still be the wrong product. The
arithmetic has tests, so a later change that keeps the numbers and softens the
sentence into a verdict passes everything and nobody notices until a user
reads it.

*Percentiles were agreed on the condition that they state where a measurement
sits and never judge it. The test bans "good", "fine" and "on track" alongside
"low" and "behind", because a parent whose baby is on the ninth centile reads
the absence of the kind words as the bad news.*

## Read the diff where a test cannot help

Read your own changes line by line when they touch money, personal data, safety
advice, deletion, or anything a user is told in writing. Types and builds catch
mechanical mistakes and say nothing about whether the logic is right or the
wording is honest.

**Why:** the failures that matter here are ones the compiler is happy with.

*An agent added a backup copy of a local store, which was a good idea, and the
backup was not covered by the delete everything function. A user who deleted
their health record would have kept a full copy on the device.*

## Write down what is unfinished, in the same commit

Whenever you build part of something, write the rest into `to-be-continued.md`
in the same commit. Say what works, what does not, and what would finish it. Do
not leave it as a code comment, which only the next person to open that file
sees, and do not leave it in the chat, which is gone when the session ends.

Read the file at the start of a session, before planning anything.

**Why:** you produce convincing surfaces quickly. A screen with real prices, a
confirm button and a tick list looks finished in a screenshot and in the code,
and charges nobody.

*One project's list held a rating that was stored and never read, a notification
time picker that sent nothing, a paywall that charged nobody and a free tier
that limited nothing.*

## Delete the entry when you finish the work, in the same commit

Finishing something is two edits, not one. Delete its entry from
`to-be-continued.md` and write what happened in `change-log.md`, both in the
commit that finishes the work. Never write "this is fixed now" underneath the
entry, and never head an entry in the unfinished list with a date, because a
dated entry cannot be deleted on its own once the next thing is written under
it.

Skip this only where the work is genuinely finished and was never written down
as unfinished, which is most small changes. Then there is nothing to delete, and
a change log line is worth writing only if somebody would later ask when it
changed.

**Why:** the unfinished list is read at the start of every session, so its
length is charged to every session. It only shrinks if somebody takes things
out of it, and the person best placed to do that is whoever just finished the
work.

*One project's `to-be-continued.md` reached 1958 lines across 62 sections, most
of them recording finished work. Two sections described the same unfinished item
with opposite statuses, because nobody could see the whole file at once, and the
document an agent was told to read first had become the most expensive one in
the repository to read.*

## Price anything that keeps running before you set it up

Before setting up a build, a scheduled job, a webhook or anything else that runs
after the conversation ends, say what it costs per run, who pays for it, and
what stops it. Ask before turning it on.

**Why:** automated work you set up outlives the session, and nobody is watching
the bill.

*A build service ran a cloud build on every push to the main branch and spent
credits from a paid allowance. Several days of ordinary commits went by before
anybody noticed.*

## Work on your own branch, and read main before branching and before merging

Use a branch of your own whenever more than one session may be running. Fetch
the main branch and read it before you start. Fetch again before merging and
read what changed in every directory your branch touches, paying most attention
to a directory that now exists on both sides. Read the merge result rather than
the merge output, because the output will say it went fine.

Keep branches short and merge main into yours once a day. Before reporting a fix
as done, check that the commit is on the branch you claim, e.g. with git log
against the remote. Report what landed, not what you attempted.

Check the branch by name before you commit, and check it again whenever a
session resumes from a summary, because the summary says which branch the work
should be on and not which one is checked out. A log of the latest commit says
nothing about the branch, since it shows whatever HEAD is on. If a commit did
land on the wrong branch, do not switch branches and carry on. Switching puts
the other branch's files back in the working tree, so anything bundled or
published after the switch is the old code with a fresh label. Read the commit
a publish reports against the commit you meant to publish before you say it
went out.

Skip it when you are the only session working, which you can check rather than
assume.

**Why:** parallel sessions cannot see each other, so both edit the same files
and both report success. Git reports a conflict when two branches change the
same lines and reports nothing when each adds a different file to the same new
directory, so two sessions can build a component with the same job under
different names and the merge that strands one of them looks like a success.
Losing a file that way is worse than a conflict, because a conflict stops you
and this does not, and the tests still pass because they were written against
the branch that survived.

*One session reported committing and pushing a fix. The fix was not on the main
branch at all, and the problem it described was still live hours later.*

*Another session resumed from a summary with main checked out and committed
there. The push of its own branch said it was up to date, which was true. It
then checked that branch out, which put the previous round's files back, and
published them to both platforms under the new message. The publish printed
the previous commit's hash, and nobody read it until the served update was
checked.*

## Take initiative, and ask only where the cost is real

Ask first when an action spends money, publishes something to people outside the
project, destroys data, or cannot be undone cheaply. Read the project's rules
file, which settles the cases it covers. When you do ask, put the options and
their costs in the message, so the person can answer without opening anything.

Do everything else without asking, and say what you did afterwards. Fixing
something you noticed on the way, tidying a document, and merging your own
branch once the work is agreed are all work rather than decisions.

**Why:** without a written rule every session guesses, and the guesses differ.
Asking about work that was already agreed costs the person a round trip, and it
reads as a request for a second yes.

## A "do not edit this wording" note has to name who can lift it

Wording that must not be changed without expert sign off is worth protecting
with a note in the file. Write the note so that it names three things: who
signed the wording off, who may lift the rule, and what a change has to preserve
even when it is allowed. A note that only says the words are final is a rule
that either blocks the work it was written for, or gets ignored by whoever is
told to do it anyway.

When somebody with the authority does lift it, record in the same file that they
did, on what date, and what changed, so the next reader knows the review the
note refers to no longer covers what is there.

Skip this where the wording is ordinary product copy. It is for the cases where
being wrong is expensive: medical, legal, safety and regulatory text.

**Why:** the two failure modes are opposite and both happen. A rule with no
exception route makes the owner's own instruction look like something to be
argued with, so it gets overridden in a chat message that nobody can find later.
A rule that is silently overridden leaves text that carries the authority of a
review it no longer had.

*A baby app's red flag list carried a header saying every line was reviewed and
must not be edited. The owner asked for the whole list to be rewritten for
readability and said in the chat that no further approval was needed. Both were
right. What was missing was any way to record that in the file, so the next
reader would have found reviewed sounding wording that no clinician had seen.
The header now names the approval, the date, and the rule that no rewrite may
narrow what the list catches.*

## Mark documents nobody has reviewed

Put one line at the bottom of any document you write saying who wrote it and
whether anybody checked it. Do not cite an unreviewed document as evidence, your
own included.

**Why:** a guess you wrote down is still a guess, and giving it a filename makes
it look like a source.

*An agent quoted a document it had written three hours earlier back to the user
as though it settled the question.*

## Batch your work around what the person has to check by hand

Hand over once when checking your work means installing a build, walking through
a flow, or waiting for a deploy. Do the whole list first. Hand over as you go
when the person can check the result by reading a diff.

Hold back a change that is big enough or risky enough that landing it with seven
others would make a bad result hard to read. Say which one it is, say why, do
the rest, and keep that one on its own.

**Why:** count the cost on their side rather than yours. Eight changes that each
cost the person ten minutes of installing and clicking cost eighty minutes of
somebody's evening when you hand them over one at a time, and the eighty minutes
buy nothing.

## Make a build say what it is, and give it one name

Put the version somewhere the person running the app can read it out, visible in
every build rather than only in a development one. Move the number on for every
build you hand over, from a script the build runs itself, so it cannot be
forgotten. Name the artefact after the version and the commit, and publish it to
one place.

Do not gate that line on a development flag. The build a person is holding is a
release build, so a line behind `__DEV__` is invisible in exactly the build
where the question gets asked.

**Why:** "almost none of the things I mentioned have been actioned" is what a
correct build reads like when the wrong file was installed. Two days of work
were published to one location while the link in the README served a build from
two days earlier, and both files were called `app-release.apk`. Nothing about
the code was wrong and there was no way to find that out. A number in the app
and a name on the file turn a day of confusion into one question.

## Ask for the pieces, not for a formatted string

When a form takes something structured, e.g. an amount and a unit, a name and a
date, or an address, give it a field per piece and offer the choices you accept.
Do not take one text box and parse it. If a value can only be one of six things,
those six are buttons.

Let the choice you offer follow the choice already made: a unit list that
changes with the category cannot record a head circumference in pounds.

Skip this where the input really is free text, such as a note or a title. The
rule is about values with a shape, not about every box on a form.

**Why:** a parser makes the person learn your format before they can use the
feature, and it fails quietly on everything it does not recognise. One app asked
for "4.9 kg · 55 cm · 38 cm" in a box and read it back with three regular
expressions. Anything typed that they did not match saved a row with no numbers
in it, and the log listed that row as blank. The person who typed it was told
nothing.

## Close every piece of work with a status and what is waiting

End every piece of work by saying what you verified, what you assumed, and what
you left undone. Then list what is waiting on the person, and for each item say
what the choice is and why you cannot make it. Separate them by kind, because
each kind needs something different.

- **A check you cannot run.** It needs a real phone, a real payment, or a real
  account. Say what to do, and say what a bad result looks like so the person
  knows when to stop and come back.
- **A decision that should not be taken alone.** It costs money, changes what
  people see, or cannot be undone. Put the options and the cost of each one in
  the message, in enough detail to answer without opening anything.
- **Nothing at all.** Work that was already agreed is not waiting on anybody.
  Say it is going ahead, so one yes is not read as a request for another one.

The person only reads the chat. A question you write into a file is not a
question, it is a note you left for yourself.

**Why:** you answer this accurately when asked and you do not volunteer it, and
work that compiles can still be half finished.

## Write prose in this repository's style

Follow `.claude/skills/plain-writing/SKILL.md` for every piece of prose you
write for a person, including documents, commit messages, pull request bodies
and chat summaries. It does not apply to code or code comments.

**Why:** padded prose gets skimmed, and a rule that gets skimmed is not a rule.

## Measure a dark theme, do not judge it by eye

Every colour in a dark theme has a number: the contrast between it and the
surface it sits on. Text needs 4.5 to 1 to be read comfortably, and anything a
person has to see and press, such as a border, a divider or an icon, needs 3 to
1. Work each one out and write the numbers beside the palette, so the next
person changing a colour knows what it has to clear.

The trap is that a dark theme is usually built by somebody looking at a bright
screen in a bright room, where a very dim grey on near black still reads. On a
phone at arm's length it disappears. The author cannot see the fault, and the
person who can is using the thing in the dark, which is the one time they will
not stop to report it.

A dim palette also feels like the considerate choice, which is what makes it
hard to argue with. What keeps a dark theme gentle is the low brightness of the
screen. Contrast between the ink and the card is what makes it legible, and
lowering it takes away the legibility without giving back any of the comfort.

When a palette has to be fixed, take the target from something in the product
that already reads well. A measurement of the one part somebody says they can
see is worth more than a number chosen from a standard, because it is the same
screen, the same room and the same eyes.

**Why:** contrast is the one part of visual design that is arithmetic rather
than taste, so it is the one part that can be settled rather than argued about,
and it is the part most often left to whoever happens to be looking.

*One app's night theme drew a hairline round every card at 1.15 to 1, which is
invisible, its muted text at 2.34 and its warning red at 2.39. Nothing looked
wrong in a screenshot on a desktop. The owner used it in bed and said the
buttons and the lines round the pills could not be seen. The fix took the
border strength from the one thing he could see, a pinned tile outlined in the
accent colour at 3.2 to 1, and drew every border at that strength.*

## Describe what a thing is, and skip what it is not

Write the description. A sentence saying what something is not leaves the
reader holding the wrong idea and still looking for the answer, and it costs
the same space the description would have taken.

Use a negative in one place only: to correct a belief the reader almost
certainly already has. Give the description first, correct the belief once, and
carry on.

The words to watch for are "not", "rather than", "instead of", "no longer" and
"unlike", arriving before the reader has been told what the thing is. The same
goes for a list of everything a feature will not do, which reads as thorough
and tells somebody nothing they can act on.

**Why:** a reader learns from the description, and the negative asks them to
rule things out one at a time and arrive nowhere. It is a habit rather than a
choice, because a negative is easy to write when the writer has not settled
what the thing actually is, and it hides that from both of them.

*An agent explaining a deployment system wrote "a channel is neither of those.
It is one line on the server that points a name at a branch. Nothing else." The
person reading it pointed out that two of the three sentences told them
nothing, and that the space would have been better spent on what a channel
holds and who reads it.*

## Apply the writing style to interface copy, hardest of all

Every string a user reads goes through the same style rules as a document. A
button, a toast, a row's subtitle, a validation message, an empty state, a
confirmation. Load the style before writing the words rather than after somebody
objects to them, because interface copy is the prose least likely to be reviewed
and most likely to be read.

Two faults appear over and over in interface copy written by an agent, and both
are worth checking for by name.

The first is a missing subject. A short line under a control gets written as a
fragment with nobody doing anything, because the fragment is shorter and the
control is right there. "Takes 17 records with it. There is no way back." A
person then has to work out what takes them and what has no way back. Write
whole sentences with the person or the app as the subject, saying what will
happen: "This will also delete the 17 records written down against Oliver."

The second is drama. Warnings, deletions and errors invite a heightened
register, and an agent will reach for it. Somebody about to delete something
already knows it is serious, and language that presses the point reads as the
software being pleased with itself rather than as care. State what will happen
and say plainly that it cannot be undone. Nothing more is needed.

Confirmations have a shape that works. Name the action and what it covers, say
it cannot be undone, offer the way to keep a copy, and say what is not affected.
In that order, in complete sentences.

**Why:** a document nobody likes gets rewritten later. Interface copy ships and
stays, and it is read by every user at the worst moment, which is when they are
about to destroy something. It is the prose with the largest audience and the
least review, and treating it as a label rather than as writing is how it ends
up as fragments that alarm without informing.

*An app added a control for deleting one child's profile from a family record.
The code was correct, the confirmation asked twice, and the words under the
button read "Takes 17 records with it. There is no way back." The owner's
verdict was that the feature worked and the writing was really bad. The
replacement said the same thing in two sentences with subjects in them, and was
less frightening while carrying more information.*

## Key a measurement on something that is not copy

Give every screen, step or state you measure a stable key that no user ever
sees, separate from its title. Send the key. Never send the visible words.

**Why:** a title is copy, and copy gets rewritten. A funnel keyed on the title
renames a step the first time somebody improves a sentence, and that step's
history ends there. Nothing breaks, no test fails, and the app carries on
working, so the loss is invisible until somebody tries to compare this month
against last month and finds two steps where there was one. The rewrite that
causes it is usually a good change made by somebody who has never opened the
analytics.

*An onboarding funnel was specified as "the screen's name". The screens had no
identifiers, only titles, in a project whose owner rewrote onboarding copy most
weeks. Adding a key to each screen took ten minutes before the first row was
written, and would have cost the entire history to add later.*

## Check what the platform records before trusting what your schema does not

When a design's safety rests on a field you did not store, go and look at what
the platform stored anyway. Query the logs. Do not read the pricing page and
infer.

**Why:** you control your tables and you do not control the layer underneath
them. Gateways, edge networks and function runtimes keep their own request logs,
and those routinely hold the caller's address, the exact time and the path,
which is enough to line a log line up against a row you thought was anonymous.
The retention is usually short and usually invisible from inside the
application, so a design can be correct about its own schema and wrong about the
system.

*A measurement was designed around the premise that no address was kept, which
was true of the table. Asking the platform's own logs returned 933 rows from one
day carrying the caller's IP, its country and the exact request time, on the
same project. The window was about a day, which made the finding survivable
rather than fatal, but it was a finding rather than a guess only because
somebody ran the query.*

## Read the running system before you describe it

A document that describes something outside the repository, like a database, a
hosting account or a store listing, is a snapshot of what somebody saw once. It
does not change when the thing it describes changes, and nothing fails when it
stops being true. So before you follow such a document, and before you edit it,
ask the system itself through whatever connection you have, and write down both
what you read and the date you read it.

Prefer a check anybody can repeat. A query against a system table, or a call to
a management API, is better than a sentence saying it is set up, because the
next person can run it again and get the current answer rather than yours.

**Why:** setup pages go stale in the direction that costs the most. They are
written when a thing is first configured and read months later when somebody is
trying to finish it, and every step taken from a stale page is taken with
confidence.

*A baby app's Supabase setup page said one migration had to be applied, that
nothing was uploaded to the server yet, and that the app had no Google sign in
button. Reading the project through its management connection showed two
migrations outstanding rather than one, in an order that mattered, because the
second created a trigger calling a function the first defined. Sync had been
built the day after the page was written and the app had had a Google button
for weeks. Each sentence was true when it was typed. Following the page would
have left the database half migrated and a sign in button failing in front of
a parent.*

## A handover route that keeps every copy will stop working

When a build has to reach a person, do not send it through anything that keeps
history. Version control keeps every version of every file for ever, so a route
that commits a hundred megabyte artefact commits it permanently. Deleting it
later frees nothing: the object is still reachable from an earlier commit, and a
clone still takes it.

Use a store that expires, a store you can delete from, or the destination
itself. Where the artefact's real home is a store or a registry, upload straight
there and skip the intermediate copy.

Skip this for anything small enough that a thousand of them would not matter.
The rule is about size times frequency, not about artefacts.

**Why:** the cost is invisible per build and unbounded in total. Each commit is
defensible on its own and the tenth is indistinguishable from the first, so
nobody objects at the moment of the decision. What eventually fails is not the
route but everything around it: the clone, the fetch, the checkout, and finally
the disk on the machine doing the building.

*A repository handed every Android build over on a branch. The runbook recorded
the cost honestly, "about 92 MB of history every time", and filed the repository
reaching 2 GB as a known problem that had slowed clones. Nothing changed,
because every individual build still worked. Nine days later `.git` was 5.9 GB,
a build died halfway through with "No space left on device", and recovering it
meant deleting the caches that made builds fast, which turned an eight minute
build into a forty minute one. The route that was cheap every single time had
become the reason a build could not be made at all.*

## Never let an API's refusal look like a finding

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

## Separate a test build from a real one by its keys, not by its branch

Applies as soon as anybody outside the project installs a build. A branch cannot
carry the difference between a test version and a real one, because the same
code points at whichever database its configuration names. What separates them
is the keys, the addresses and the flags a build reads when it starts.

Write down which values make a build a production one, keep them out of every
other build, and give the app a way to say on screen which set it is using. A
second project on the same service, holding nothing anybody would miss, is
usually free and removes the question.

**Why:** a tester who cannot tell which database they are writing to will report
a problem against the wrong one, and you will spend an afternoon looking in a
database where nothing happened.

## Ship a background service in four places, or do not ship it

Applies to anything that sends data off a person's device, e.g. crash reporting,
usage counting, or a hosted store. Adding one takes a minute, because it is a
key and a function call, and what it obliges you to do takes longer. The gap
between the two is where a real problem gets made.

The same service has to appear, saying the same thing, in all four of these, and
all four change in the same commit as the key:

- The record of what leaves the device.
- The privacy notice, both inside the product and wherever it is published.
- Whatever declarations the distribution platforms require.
- The permission the product asks for, where one is needed.

**Why:** a service that is counting people and is not in the privacy notice is
the failure to avoid, and nothing in the build will tell you it has happened.

## Hand on the list in the shape it was seen, not flattened

When one screen shows grouped things and another screen has to move through
them, hand over the groups. A list of lists, with the group that was tapped
named in the link, not one long list with the groups run together.

Flattening looks harmless because every item is still there in the right order.
What is lost is the boundary, and the boundary is the only thing that says where
a run of items ends. The next control at the last item of a group then does
something rather than nothing, and what it does is jump to a group the person
never opened.

The same rule covers how much is handed on. Hand over what was drawn, not
everything that matched: a shelf cut to twelve is a group of twelve, and a
control that walks past the twelfth walks into things that were never on the
screen.

**Why:** the fault is invisible from the code and invisible in use. Nothing
crashes, nothing looks wrong, and the item that arrives is a real item. The
person is simply somewhere they did not ask to be, with no way to tell how they
got there, and the end of a section that ought to say "that is all of this" says
nothing at all.

*An app's library screen drew six topics as six rails, and the arrows inside an
article walked a single flattened list of all six. The arrow at the end of Sleep
opened the first article of Feeding. It had been that way for weeks and read as
"the arrows are not quite right" rather than as a defect, because every article
it opened was a real article. The fix was to write the rails down as rails and
to carry the rail's name in the link, so an article that sits both in a topic
and on the saved shelf walks whichever one it was tapped in.*
