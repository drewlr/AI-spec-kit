# Meadow: rules for an agent

> What an agent may do without asking, what costs money when it runs, and the
> commands that check the work.

Example only, from an invented project. In a real project this file is named
CLAUDE.md or AGENTS.md and sits at the root.

Meadow is a diary and health record app for parents of young children. It keeps
everything on the phone and sells a subscription through both app stores.

## How to work here

You are fast, tireless, and confident whether or not you are right. Nobody can
tell a correct answer of yours from an invented one by reading it, because you
write both the same way, so the proof has to come from somewhere other than your
own reading of the code. Run the command, ask the running system, and watch a
check fail once before you trust it.

Verify a fact when being wrong is expensive, which means when it decides what
gets built, blocks somebody, or would cost more than a few minutes to undo.
Agree a spec before building anything you cannot describe in a sentence, because
given a one line request you will invent the rest of the scope, confidently and
in detail. Write down what you leave unfinished in the same commit, and take it
out again in the commit that finishes it.

Ask first when an action spends money, publishes something to people outside the
project, destroys data, or cannot be undone cheaply. Do everything else without
asking, and say afterwards what you did. When you skip a rule, say in the chat
that you skipped it and why.

## What you may do without asking

- Write code, tests and documents, and commit them to your own branch.
- Run any of the checks below, as often as you like.
- Update `to-be-continued.md`, `change-log.md`, `decisions.md` and
  `test-notes.md`.
- Fix something you noticed on the way, and say afterwards that you did.

## What you have to ask about first

- Merging to main.
- Anything that spends money, including cloud builds and paid API calls.
- Uploading a build to either store, or changing a store listing.
- Changing or deleting rows in the articles spreadsheet.

## What costs money when it runs

- A cloud build spends credits from a paid allowance. Run `npm run build:local`
  instead, unless somebody asks for a cloud build.
- The translation API charges for every call. Tests use the cached copy in
  `fixtures/translations.json`, so do not call the live API from a test.

## Facts about this project that surprise people

- The diary on the phone is the only copy of a parent's data. No server holds a
  backup, so a bug that loses data loses it for good.
- Expo Go cannot run this app, because the keystore uses a native module. Use a
  development build.
- `src/data/articles.ts` is generated. Edit `content/articles.csv` and run
  `npm run build:content`. Never edit the generated file by hand.

## The commands that check the work

- `npm run typecheck`
- `npm test`
- `npm run lint`
- `npm run build:local`, before saying that a change builds.

## Documents you must open, and when

**These are instructions, not a reading list.** Do these, in this order, before
doing anything else.

**At the start of every session, before planning anything:**

1. Read `SPECS.md`. What the product is, every feature, and the one being built
   now.
2. Read `to-be-continued.md`. What only looks finished. Never add an entry to it
   without having read what is already in it.

**Before your first commit of a session, and again before any release:**

3. Read `runbook.md`. It holds every command, every route a build has been
   handed over by, and what went wrong with each.

Then open the rest when its trigger happens, rather than reading them all.

| Document | Open it |
| --- | --- |
| `architecture.md` | Before changing anything that crosses between the app, the store on the phone, and a paid service |
| `decisions.md` | Before proposing a change of direction, so you do not re-propose something already rejected |
| `test-notes.md` | Before treating a green test run as proof you did no harm |
| `data-map.md` | Before adding or removing anything the app collects about a parent or child |
| `content/articles.csv` | Before changing an article, a category or a red flag |
| `lessons.md` | When something here cites a lesson by number, and almost never otherwise. Add to it when something goes wrong that would go wrong on the next project too |
| `change-log.md` | Almost never. Open it to find out when something changed or why, and write to it whenever you finish something |

## Documents nobody has reviewed

- `docs/data-map.md`, drafted by an agent, not read by a lawyer.
- `docs/architecture.md`, written before the offline queue was added, so the
  diagram is behind the code.
