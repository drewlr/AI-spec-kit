# Meadow: what is tested

> What is tested, what is not, and what a passing test run does and does not
> prove.

Example only, from an invented project. One paragraph is enough, and this is
two.

The storage layer is tested properly, because the phone holds the only copy of a
parent's diary and a bug there loses data for good. `src/storage/` has tests for
saving, reading, migrating between schema versions, and the delete everything
function, including a test that asserts nothing survives a wipe. The content
generator is tested, so a malformed row in the spreadsheet fails the build
rather than shipping.

Nothing else has tests. The screens, the navigation and the paywall are checked
by hand, so a passing suite says the data is safe and says nothing about whether
the app works. Purchases have no tests at all, because they need a real store
account, and they are checked by buying a subscription in the sandbox before
each release. Treat a green run as evidence about storage only.

Written by an agent on 2026-08-14, and current as of the last release. A
developer has read it.
