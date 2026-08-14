# Meadow spec: a backup the parent can restore

Example only, from an invented project. Written before the work started, agreed
before any code, and deleted when the feature ships. In a real project it lives
at `specs/backup-restore.md`.

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
  because merging two diaries is a different feature with its own spec.

## What is undecided

- Whether the file is encrypted with a passphrase the parent chooses. It
  protects a child's health record sitting in somebody's email, and a forgotten
  passphrase makes the backup worthless. Ask before building either version.
- The file format. A zip is the obvious answer, and a single file with the
  photos embedded is easier to email. Decide once we know how large a real
  export turns out to be.

Written by an agent on 2026-07-20, agreed with the team on 2026-07-21. The
passphrase decision goes to the decision log when it is taken.
