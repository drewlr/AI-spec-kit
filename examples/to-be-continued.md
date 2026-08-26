# Meadow: what looks finished and is not

> Everything that looks finished and is not. Read it before planning anything.

Example only, from an invented project. An agent writes into this file in the
same commit as the partial work, and reads it at the start of a session.

Each entry says what works, what does not, and what would finish it. Entries are
grouped by who has to act, and the newest is at the top of its group.

**Finishing something deletes its entry from here and writes a line in
`change-log.md`, in the same commit.** Nothing is ever added to this file to
record that something got done.

## Waiting on a decision only the owner can take

### The free tier limits nothing

**Works.** The app knows whether a subscription is active and the settings
screen says which tier the parent is on.

**Does not.** A parent without a subscription can add unlimited entries and
photos. The limit of thirty entries exists in the copy on the paywall and
nowhere in the code.

**To finish it.** Decide what happens at the limit, because stopping a parent
writing about their child's illness is worse than letting the limit slip. Then
enforce it in `src/model/access.ts`. Blocked on that decision.

Added 2026-08-11 by an agent.

## Waiting on a check nobody has run

### Nobody has restored an export on a second phone

**Works.** The export writes every diary entry to a JSON file, and the file
opens.

**Does not.** Nobody has taken an export off one phone and opened it on
another, so the only proof it works is a test against a fixture of three
entries.

**To finish it.** Somebody has to export from a real account and open the file
on a second phone. Only the owner can do that, because it needs two devices.

Added 2026-08-04 by an agent.

## Half built, and an agent can finish it

### The paywall charges nobody

**Works.** The screen shows both prices, the buttons respond, and choosing a
plan sets `subscription.active` in the store.

**Does not.** Nothing is charged. There is no call to the billing library at
all, and the flag is set directly.

**To finish it.** Wire the buttons to the purchase flow, verify the receipt with
the store rather than the phone, and add a restore button, which the store
requires. Roughly a day, and it needs the subscription products created in both
stores first.

Added 2026-08-11 by an agent.

### The notification time picker sends nothing

**Works.** The picker saves a time and shows it again after a restart.

**Does not.** No notification is ever scheduled. Permission is never requested.

**To finish it.** Ask for permission at the right moment, schedule a daily local
notification, and cancel it when the parent turns the setting off. Half a day.

Added 2026-07-29 by an agent.

### An export leaves the photos behind

**Works.** The export writes every diary entry to a JSON file and the file
opens.

**Does not.** Photos are referenced by filename and not included, so an export
restored on a new phone shows broken images.

**To finish it.** Write a zip with the photos alongside the JSON, and test it on
an account with several hundred photos rather than three. Blocked on nothing.

Added 2026-07-22 by an agent.
