# Meadow: rules for an agent

> What an agent may do without asking, what costs money when it runs, and the
> commands that check the work.

Example only, from an invented project. In a real project this file is named
CLAUDE.md or AGENTS.md and sits at the root.

Meadow is a diary and health record app for parents of young children. It keeps
everything on the phone and sells a subscription through both app stores.

## What you may do without asking

- Write code, tests and documents, and commit them to your own branch.
- Run any of the checks below, as often as you like.
- Update `to-be-continued.md`, `decisions.md` and `test-notes.md`.
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

## Documents nobody has reviewed

- `docs/data-map.md`, drafted by an agent, not read by a lawyer.
- `docs/architecture.md`, written before the offline queue was added, so the
  diagram is behind the code.
