# Before a mobile app goes live

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
- [ ] **Analytics, scoped on purpose.** Decide the question before choosing the
      tool, e.g. whether people finish onboarding, which step they abandon, and
      whether they come back on day two. Collect nothing sensitive. Pass when
      every event you send appears in the privacy policy and both store forms,
      and non essential analytics ask for consent in Europe.
- [ ] **A way for users to reach you** that is not a store review.
- [ ] **Human: watch the reviews for the first fortnight.** The first bad one
      usually describes a real bug before anybody reports it properly.

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
