# Meadow: what personal data we hold

Example only, from an invented project. Copy the shape and answer for yourself.

| What | Why we have it | Where it goes | How long | How to get rid of it |
| --- | --- | --- | --- | --- |
| Diary entries, including free text about a child's health | The app exists to keep them | The encrypted store on the phone only | Until the parent deletes it | Delete everything in settings, which wipes the store and the export cache |
| Child's first name and date of birth | Shows an age on entries | Same store, never sent | Same | Same |
| Photos attached to entries | The parent chose to attach them | Same store, never sent | Same | Same |
| Purchase receipt | Proves the subscription | The app store, not us | Held by the store | The store's own process, not ours |
| Crash reports | Fixing crashes we cannot see | Sentry, in Ireland | 90 days | Turn crash reporting off in settings |
| Analytics events | Whether people finish onboarding | Our analytics tool, in Europe | 14 months | Decline analytics on first launch, or turn it off later |

## What we do not collect

No account, no email address, no advertising identifier, and no location. The
app has no login, so there is nothing to link a phone to a person.

## Sensitive data

Diary entries are health data about children, which both the GDPR and both app
stores treat as a special case. They never leave the phone, which is why this
document is short.

## Getting rid of everything

Settings, then delete everything. It wipes the encrypted store, the export
cache, the thumbnail cache and the Sentry local queue. A test covers the wipe
and asserts that reopening the app shows the empty state.

Written by an agent on 2026-08-14. Not reviewed by a lawyer, and it needs to be
before launch, because the impact assessment for children's health data cannot
be signed off by an agent.
