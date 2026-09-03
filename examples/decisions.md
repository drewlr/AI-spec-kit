# Meadow: decision log

> The decisions that took real thought, what else was considered, and why. Read
> it before proposing a change of direction.

Example only, from an invented project. One entry per decision that took real
thought. Newest first.

## 2026-07-02. Store payments rather than a card processor

We use the App Store and Play Billing for the subscription. We considered
Stripe, which charges less and gives us the customer record, and a web checkout
outside the app, which some apps use to avoid store fees.

Both stores require their own payments for digital subscriptions, so the choice
was not really open. Stripe stays for any future physical goods. Revisit if
either store changes its rules, which both have been asked to do by regulators.

## 2026-06-18. Everything on the phone, no server

The diary stays on the device and we run no backend. We considered a hosted
database with sync, which would give backup and multi device support.

Holding health notes about children on our server means a formal impact
assessment, a data processing agreement, and a breach to worry about for as long
as the company exists. The cost of the alternative is that a lost phone loses
the diary, which we say plainly in onboarding. If parents ask for sync often
enough, the entry to revisit is this one.

## 2026-05-30. Articles in a spreadsheet, generated into code

Content lives in `content/articles.csv` and a script generates
`src/data/articles.ts`. We considered a headless CMS and considered writing the
articles directly in TypeScript.

A CMS means a network call, a monthly bill, and content that can change under a
released app. Writing them in code means only somebody who reads code can fix a
typo. The spreadsheet lets the midwife who writes the content edit it, and the
diff in a pull request is readable.

Written by an agent on 2026-08-14, from decisions taken by the team. The team
has read it.
