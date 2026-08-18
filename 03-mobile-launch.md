# Before a mobile app goes live

> What has to be true before an app is submitted to the App Store or Google
> Play, and what each item passes on.

Work through this list before submitting to the App Store or Google Play. The
blocking section is what stops a submission. The rest is what stops the app
being bad once it is accepted.

An agent can do most of these. It cannot do the ones marked **Human**, because
they need an account, a payment method, or a professional signature, and those
take days rather than minutes. Start them first.

Report against this list item by item, and say pass, fail, or not checked for
each one. Do not mark an item as passed because the code looks right.

## Blocking. The stores refuse the app without these

- [ ] **Payments take money.** Digital subscriptions go through the store's own
      payments on both platforms, and card processors are not allowed for them.
      Pass when a test purchase produces a receipt the store confirms, checked
      with the store rather than by trusting the phone.
- [ ] **Restore purchases works.** Any app selling a subscription or a permanent
      unlock has to offer it, reachable from settings, a paywall or a store
      menu. Pass when restoring on a second install returns the entitlement.
      Apple rejects apps that sell and cannot restore.
- [ ] **Account deletion inside the app**, if the app has accounts. Apple
      requires it. Pass when deleting removes the server side too, and signing
      in afterwards fails.
- [ ] **Sign in with Apple**, if the app offers any other social login on iOS.
- [ ] **An Android signing key you own.** Builds signed with the automatic debug
      key install on a phone and are refused by the store. Pass when the release
      build is signed with a keystore that exists in a backup you will still
      have in five years, because losing it means you cannot update your own
      app.
- [ ] **A privacy policy at a public URL**, linked from both store listings.
- [ ] **Human: professional sign off on advice.** When the app tells somebody
      what to do about their health, their money or their safety, a qualified
      person reads every outcome before it ships. An agent writes this content
      well and cannot take responsibility for it.

## The store listings

- [ ] Apple's privacy questions, which ask what you collect and whether it is
      tied to the person.
- [ ] Google Play's data safety form, which asks the same things differently.
      Pass for both forms when they agree with each other, with the privacy
      policy, and with what the code actually sends.
- [ ] Age rating.
- [ ] Support URL, and an email address somebody reads.
- [ ] Screenshots, description and keywords.
- [ ] Terms of service, separate from the privacy policy.
- [ ] Extra time booked when children's data is involved, because both stores
      ask more questions.

## Knowing what happens after release

- [ ] **Crash reporting.** The most useful item on this page, because a crash on
      a phone you do not own is otherwise invisible and the only signal is a one
      star review a fortnight later. Sentry and Firebase Crashlytics both have
      free tiers. Pass when a crash you cause on purpose in a release build
      appears in the dashboard.
- [ ] **The questions written down before the analytics tool is chosen.** A tool
      chosen first produces a dashboard of numbers nobody acts on. Five
      questions is enough to start, e.g. whether people finish onboarding, which
      step they abandon, and whether they come back on day two. Pass when every
      question can be answered by counting an event, which means none of them
      needs to know who did it.
- [ ] **Every event tested before it is sent.** If the event arrived on a
      stranger's desk with no other context, could they work out who the person
      is, or anything sensitive about them? If yes, it does not leave the phone.
      Two fields that are each harmless can fail together, so test the whole
      event rather than each field.
- [ ] **Analytics declared everywhere.** Pass when every event you send appears
      in the privacy policy and both store forms, and non essential analytics
      ask for consent in Europe.
- [ ] **A way for users to reach you** that is not a store review.
- [ ] **Human: watch the reviews for the first fortnight.** The first bad one
      usually describes a real bug before anybody reports it properly.

## Email, if the app sends any

- [ ] **A sending domain with SPF, DKIM and DMARC, set up early.** Any app that
      emails a sign in code needs one, because a default sender is rate limited
      and lands in spam, and a sign in code in the spam folder is an app that
      does not work. It takes an afternoon and it only gets harder to do later.
- [ ] **Transactional email kept apart from campaigns.** Somebody who asked for
      a sign in code has not asked to hear from you again. Adding their address
      to a mailing list because they signed in is not consent, and one
      unsubscribe complaint costs the sending reputation the codes depend on.

## The user's data

- [ ] **Say out loud whether losing the phone loses the data.** For a local
      first app the answer is often yes, and often nobody has said so. Pass when
      either a backup exists or the app tells the user plainly that the export
      is the backup.
- [ ] **Export tested on a full account**, not an empty one. Hundreds of photos
      and a year of records behave differently from three test entries.
- [ ] **Storage limits checked.** Phones limit how large a single stored value
      can be, and hitting the limit can fail silently while the app appears to
      work. Pass when you know where the ceiling is and how close the app gets
      to it.
- [ ] **Delete everything, then reopen.** Pass when nothing comes back,
      including caches, backups and any copy added later. Deletion that leaves a
      copy is worse than no deletion button, because you claimed it in the
      privacy policy.

## Quality on real devices

- [ ] Tested on a cheap Android phone, not only a fast one. Most people do not
      have new phones.
- [ ] System font size turned to maximum, on every screen.
- [ ] Screen reader used on the parts that matter most, meaning signing up,
      paying, and anything urgent.
- [ ] Aeroplane mode, with nothing hanging while it waits for a network that is
      not there.
- [ ] An error boundary, so one broken screen does not blank the whole app.
- [ ] First launch on an empty phone, and a launch after an update on a phone
      with plenty of data. They are different, and the second one is the one
      that breaks.

## Human: start these before you think you need to

- [ ] Apple's developer account, which costs money every year and can take days
      to verify.
- [ ] Google Play's developer account, which now often requires testers before a
      new developer can publish.
- [ ] Subscription products set up in both stores, which is fiddly and has to
      match exactly what the app asks for.
- [ ] The first store review, which is slower than every review after it.

## Choose the name against the store, the registries and the register

The name is not only a branding choice. It decides the domain, and the domain
decides the invite links, the deep links and the address the app sends email
from, so leaving the name open leaves those unfinished. Settle it before you
build the things that depend on it, and settle it with evidence.

Four checks, all free, all doable in an afternoon.

- **Search the name in the store, in the country you are selling in.** An app
  with the same name is allowed and is still a problem, because somebody who
  hears your name and types it finds the other one.
- **Type the name into the store's own search suggestions.** The suggestions
  are ordered by how often people search for them, so they tell you what a
  person who half remembers your name is actually offered.
- **Check the domain against the registries rather than a reseller.** RDAP
  answers 404 for a free domain and 200 for a taken one, at
  `rdap.verisign.com` for `.com` and `rdap.nominet.uk` for `.uk`. A reseller's
  search page will tell you a free domain is premium and expensive.
- **Search the national trademark register** for the same word in the software
  class and in the class covering whatever your app is about. A word that means
  something friendly can still be a registered mark for something else.

Do this before the first build rather than before submission. The application
identifier cannot be changed once a version is published, so a name settled
late is a name you keep.

**Why:** a name chosen without these checks is usually found out at submission,
which is the point where changing it costs the most.
