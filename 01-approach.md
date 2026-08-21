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
overlapping conditions — one value, read twice.

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

## Say what the screen shows when the supply runs out

Anything that deals items out of a finite supply — a list, a feed, a queue, a
schedule — eventually asks for more than there is. Decide what happens at zero
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
already, so the section was empty — for the readers who had used the app most.
The fix was one rule, that a stage may be short but never empty, and one line in
the report so the two stages stay named as a content gap rather than
disappearing behind the floor.*

## A limit enforced at one door is not a limit

When something is capped — a free tier, a quota, a maximum of three — find every
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
same commit. Search for the old shape, not the new name — the copies are what
the code looked like before you touched it.

**Why:** the copy you did not update is now the only one that is wrong, and it
is wrong in the same way the original bug was, so the symptom comes back
looking like a fix that did not work.

*A screen's list-picking rule was extracted into the model and corrected there.
A second screen, which the first one links to, still had the original line
inline. It went on dealing a different list from the one the user had just
tapped through from, and the shared function it was supposed to be using sat one
import away.*

## A number the user can also set is not a derived number

When a figure is worked out from other figures but the user can also type it
themselves, hold both: the derived value and whatever they typed. Show the
derived one while its inputs are filled in and theirs when they are not, and
never overwrite what they typed with a fresh calculation.

The same goes for a stock figure — money in an account, items in a store,
anything that goes down as well as up. Do not derive it by adding up a log of
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
was untestable in this project's setup — importing it pulled in the theme, the
store and the device storage. Moved into a plain module, the same logic took
six tests, one of which caught a locked pair with only one side running
starting the stopped side instead of stopping the running one.*

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
the engine that will run the code — its source or its own reference, not a
tutorial — for any default your code leans on without naming. Then name it,
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
weeks later anyway. A subscription fade was put around the chart — a fixed
height with its overflow hidden — and the chooser inside it lost its list. It
was read as a stacking bug and a build was spent on the stacking order before
anybody looked at the box around it.*

## Do not send somebody to a screen that is already open

Applies wherever a screen finishes and moves the user on: a form that saves, a
confirmation, a payment, a sign in. Go back to what they came from rather than
naming a destination. Most routers asked for a route that is already on the
stack put a second copy of it there rather than returning to the first, and
everything in that copy mounts again — its effects run, its timers start, and
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

When a feature needs published figures — a standard, a rate, a reference table
— fetch them from the body that publishes them and generate the code from the
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
obviously safe, put a newborn's measurement out by 0.14 of a z-score — five
percentile points — because the curves move fastest in the first fortnight.
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

## Price anything that keeps running before you set it up

Before setting up a build, a scheduled job, a webhook or anything else that runs
after the conversation ends, say what it costs per run, who pays for it, and
what stops it. Ask before turning it on.

**Why:** automated work you set up outlives the session, and nobody is watching
the bill.

*A build service ran a cloud build on every push to the main branch and spent
credits from a paid allowance. Several days of ordinary commits went by before
anybody noticed.*

## Work on your own branch and report what landed

Use a branch of your own whenever more than one session may be running. Before
reporting a fix as done, check that the commit is on the branch you claim, e.g.
with git log against the remote. Report what landed, not what you attempted.

**Why:** parallel sessions cannot see each other, so both edit the same files
and both report success.

*One session reported committing and pushing a fix. The fix was not on the main
branch at all, and the problem it described was still live hours later.*

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

When a form takes something structured — an amount and a unit, a name and a
date, an address — give it a field per piece and offer the choices you accept.
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
