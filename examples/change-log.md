# Meadow: change log

> What changed, when, and why, newest first. Written to often, and read rarely.

Example only, from an invented project.

Nobody reads this file at the start of a session. Open it when you need to know
when something changed or why, e.g. a fault that appeared between two builds, or
a rule that somebody remembers differently from what the code does.

`TO-BE-CONTINUED.md` holds what is still unfinished, and it is the file that
gets read every session. When a piece of work is finished, its entry comes out
of that file and what happened is written here, in the same commit.

One entry per piece of work that changed what a parent can see, or that changed
what a later session can rely on. Not one per commit, because the history
already holds those.

## 2026-08-12. A parent can turn a diary entry into a photo book

The book is generated on the phone from the entries in a date range, and it
opens in whatever the phone uses to read a PDF. It was built on the phone rather
than on a server because the diary never leaves the device, and sending the
photographs away to make a book would have been the first thing that did.

## 2026-08-09. Saving a diary entry stopped blocking the screen for a second

Saving compared the old state to the new one by turning every field into text,
so any change at all serialised the whole diary and every photo path, and touch
handling runs on the thread that was doing it. It compares by reference first
now. The fix went into `src/state/sqliteRepository.ts`, which is the file the
phone runs, and not into `localRepository`, which is the web build.

## 2026-08-04. The export covers every table rather than the diary alone

It was writing diary entries and nothing else, so a parent who exported before
switching phones lost their measurements and their appointments and had no way
to know. `exportEverything` now walks every collection in `AppState`, and a test
fails if a new collection is added without being covered.

## 2026-07-30. Articles are generated from the spreadsheet rather than typed

`content/articles.csv` is what a person edits, and `npm run build:content` turns
it into `src/data/articles.ts`. The generated file was being edited by hand as
well, so two people changed the same article in two places and the next
generation threw one of them away.
