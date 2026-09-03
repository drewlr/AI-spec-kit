# Meadow: spec

> The project in four lines, every feature it has, and the one being built now.

Example only, from an invented project. In a real project this is `SPECS.md`.
The project lines and the feature list stay. The feature written out in full is
replaced as each one ships.

## The project

- **What.** A diary and health record for one child, kept on the parent's phone.
- **Who for.** Parents of children under three, mostly in the first year, mostly
  writing at three in the morning with one hand.
- **Why.** The paper red book gets lost and a notes app has no structure, so
  parents arrive at appointments unable to answer the questions they were asked
  to track.
- **How it is built.** React Native on iOS and Android, everything in an
  encrypted store on the device, no backend, subscription through both app
  stores.

## Every feature

| Feature | What somebody can do | State |
| --- | --- | --- |
| Diary | Write a dated entry about a child, with photos, and read it back | Shipped |
| Growth | Record weight, length and head size, and see them on a chart | Shipped |
| Articles | Read a few hundred articles, filtered by the child's age | Shipped |
| Red flags | See which symptoms mean going to a doctor today | Shipped |
| Search | Find an old entry by word or by date | Shipped |
| Export | Save every entry to one file and send it somewhere | Half built, photos are missing, see `TO-BE-CONTINUED.md` |
| Backup and restore | Save a file and put the diary back on a new phone | Being built |
| Subscription | Pay yearly to unlock the whole app | Half built, charges nobody, see `TO-BE-CONTINUED.md` |
| Free tier | Keep thirty entries without paying | Not started |
| Daily reminder | Get a notification at a chosen time | Half built, sends nothing, see `TO-BE-CONTINUED.md` |
| Delete everything | Remove every trace of the child from the phone | Shipped |
| Crash reporting | Nothing, it is for us, and the parent can turn it off | Shipped |

## Building now: backup and restore

## What somebody can do when it is finished

A parent opens settings, taps "Save a backup", and gets one file they can send
to themselves however they like. On a new phone they install Meadow, tap
"Restore a backup", pick the file, and see their diary as it was, photos
included.

## How anybody will know it works

- Export from an account with four hundred photos and a year of entries, on a
  five year old Android phone. The file is produced, the app does not run out of
  memory, and it finishes inside two minutes.
- Restore that file onto an empty install, then open ten entries at random and
  compare them against the old phone. Every field and every photo matches.
- Truncate the file on purpose and restore it. The app says the file is damaged
  and leaves the existing diary untouched.
- Restore with the phone in aeroplane mode. It works, because nothing about this
  needs a network.

## What is out of scope

- Any automatic or scheduled backup. The parent presses the button.
- Any copy held by us. The file goes wherever the parent sends it, and it never
  reaches a server of ours.
- Restoring onto a phone that already has entries. The first version refuses,
  because merging two diaries is a different feature.
- Fixing the missing photos in Export. Backup and restore is a separate feature
  and Export keeps its own line in the list.

## What is undecided

- Whether the file is encrypted with a passphrase the parent chooses. It
  protects a child's health record sitting in somebody's email, and a forgotten
  passphrase makes the backup worthless. Ask before building either version.
- The file format. A zip is the obvious answer, and a single file with the
  photos embedded is easier to email. Decide once we know how large a real
  export turns out to be.

Nothing above says how to build it. The file format is in the undecided list
because it changes what the parent can do with the file, not because the spec is
choosing the approach.

Written by an agent on 2026-07-20, agreed with the team on 2026-07-21. The
passphrase decision goes to `decisions.md` when it is taken.
