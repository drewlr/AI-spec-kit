# Meadow: runbook

> How the work is done here, from where new code goes to how it reaches a
> person, and which document to update when something changes.

Example only, from an invented project.

## How we build things here

- **One thing at a time, checked before each commit.** A change touching more
  than ten files needs a line in the pull request saying why.
- **Where code goes.** Screens in `src/screens/`, shared pieces in
  `src/components/`, anything that reads or writes the store in `src/storage/`,
  generated data in `src/data/`. Nothing outside `src/storage/` opens the store
  directly.
- **Tests on the storage layer and the purchase flow, and nowhere else.** Both
  hold data or money that cannot be recovered, so write the test before the
  change. Screens and navigation have no tests on purpose, and adding them is
  not part of a feature unless somebody asks for it. `test-notes.md` says what
  that leaves uncovered.
- **Half built work goes behind a flag that is off**, with the rest written into
  `TO-BE-CONTINUED.md` in the same commit. A screen that fakes its result never
  reaches `main` switched on.
- **Done means installed on a phone and used.** Types, tests, lint and a local
  build all passing is the start, not the finish. Anything a parent can see gets
  opened on a real device before it is called done.
- **Release every two weeks if there is anything worth releasing.** Internal
  track first, then the stores two days later if nothing comes back.

## Build and run it

- `npm install`
- `npm run build:content` when `content/articles.csv` has changed.
- `npm run start` for the development server, which needs a development build on
  the phone rather than Expo Go.
- `npm run build:local` for an installable build of both platforms, which costs
  nothing.

A cloud build spends credits from a paid allowance, so ask before running one.

## Check the work

Run all four before saying a change is done. They take about ninety seconds
together.

- `npm run typecheck`
- `npm test`
- `npm run lint`
- `npm run build:local`

## Branches and merges

- Work on a branch named for the change, taken from the latest `main`. Fetch
  first, because another session may have landed something.
- Commit as you go. Push the branch whenever you stop.
- Merging to `main` needs a person. Open a pull request and say what you
  verified and what you assumed.
- Never rebase or force push a branch somebody else is working on.
- `main` is always installable. If a change would leave it half working, put the
  half behind a flag that is off, and write the rest into `TO-BE-CONTINUED.md`.

## Get it to a person

- **A developer's own phone.** Install the local build over USB. Free, five
  minutes.
- **A tester who is not in the room.** TestFlight on iOS, internal testing track
  on Android. Both need the person's email address added first, and TestFlight
  needs a build reviewed by Apple the first time.
- **The public.** Submit through App Store Connect and the Play Console. The
  first review takes days and later ones take hours.

## Routes that do not work

- **Expo Go.** The keystore uses a native module, so the app crashes on launch.
  Somebody tries this every few months.
- **Sending an .ipa file directly.** iOS refuses to install it unless the device
  is registered in the developer account first.
- **Sideloading the Android build to test the store version.** It installs, and
  it is signed with the debug key, so it proves nothing about the release build.

## Steps that need a person

An agent cannot do these, so start them before they block a release.

- Apple and Google developer accounts, both paid and both slow to verify.
- The signing keystore, which lives in the password manager. Losing it means
  Meadow can never be updated again.
- Store listing text, screenshots and the privacy answers.
- Accepting new store agreements, which appear without warning and block
  submission until somebody clicks.

## When to update which document

| When this happens | Update this |
| --- | --- |
| A feature is agreed and about to be built | `SPECS.md`, added to the list and written out in full |
| A feature ships | `SPECS.md`, collapsed back to its one line |
| Something is left half built, behind a flag, or faked | `TO-BE-CONTINUED.md`, in the same commit |
| Something half built is finished | Delete its entry from `TO-BE-CONTINUED.md` and write a line in `change-log.md`, both in the same commit |
| A service, a library that runs on its own, or a new store is added | `architecture.md`, including which features the new box serves |
| Two real options were weighed and one was chosen | `decisions.md` |
| Tests are added to something that had none, or a gap is found | `test-notes.md` |
| A new field is collected, or a field stops being collected | `data-map.md`, and both store privacy forms |
| A command changes, or a route stops working | This file |

Written by an agent on 2026-08-14. A developer has checked the commands.
