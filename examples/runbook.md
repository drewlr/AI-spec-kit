# Meadow: build and release

> How to build Meadow, how to get it to a person, and which steps need somebody
> with an account.

Example only, from an invented project.

## Build it locally

- `npm install`
- `npm run build:content` when `content/articles.csv` has changed.
- `npm run build:local`, which produces an installable build for both platforms
  and costs nothing.

A cloud build costs credits from a paid allowance, so ask before running one.

## Get it to a person

- **A developer's own phone.** Install the local build over USB. Free, takes
  five minutes.
- **A tester who is not in the room.** TestFlight on iOS, internal testing track
  on Android. Both need the person's email address added first, and TestFlight
  needs a build reviewed by Apple the first time.
- **The public.** Submit through App Store Connect and the Play Console. The
  first review takes days, later ones take hours.

## Routes that do not work

- **Expo Go.** The keystore uses a native module, so the app crashes on launch.
  Somebody tries this every few months.
- **Sending an .ipa file directly.** iOS refuses to install it without the
  device being registered in the developer account first.
- **Sideloading the Android build for a store test.** It installs and it is
  signed with the debug key, so it proves nothing about the release build.

## Steps that need a person

An agent cannot do these, so start them before they block a release.

- Apple and Google developer accounts, both paid, both slow to verify.
- The signing keystore, which lives in the password manager. Losing it means
  Meadow can never be updated again.
- Store listing text, screenshots and the privacy answers.
- Accepting new store agreements, which appear without warning and block
  submission until somebody clicks.

Written by an agent on 2026-08-14. A developer has checked the commands.
