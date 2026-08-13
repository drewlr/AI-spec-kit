# Before a mobile app goes live

For an app going to the App Store and Google Play. The order matters: the
blocking section is what stops a submission, and the rest is what stops the app
being bad once it is accepted.

An agent can do most of the work on this list. It cannot do the parts that need
an account, a payment method, or a professional signature, and those are the
parts that take days rather than minutes. Start them early.

## Blocking. The stores will refuse the app without these

- **Payments that take money.** A paywall that sets a flag and charges nobody
  is easy to build and easy to forget. Digital subscriptions must go through
  the store's own payments on both platforms. Card processors are not allowed
  for this. Check the receipt with the store rather than trusting the phone.
- **Restore purchases.** Any app selling a subscription or a permanent unlock
  must offer a way to restore it. It has to be reachable from settings, a
  paywall, or a store menu, and it must work. Apple rejects apps that sell and
  cannot restore.
- **Account deletion inside the app**, if the app has accounts. Apple requires
  it. Deleting has to remove the server side too, not only the phone.
- **Sign in with Apple**, if the app offers any other social login on iOS.
- **A signing key you own**, for Android. Builds signed with the automatic
  debug key install fine on a phone and are refused by the store. Make the
  keystore, back it up somewhere you will still have in five years, and
  understand that losing it means you cannot update your own app.
- **A privacy policy at a public URL**, linked from both listings.
- **Professional sign off on anything that gives advice.** If the app tells
  somebody what to do about their health, their money, or their safety, a
  qualified person reads every outcome before it ships. An agent writes this
  content well and cannot take responsibility for it.

## The store listings

- Apple's privacy questions, which ask what you collect and whether it is tied
  to the person.
- Google Play's data safety form, which asks the same things differently. Both
  answers must match what the app actually does, and both must match the
  privacy policy.
- Age rating.
- Support URL and an email somebody reads.
- Screenshots, description, keywords.
- Terms of service, separate from the privacy policy.
- If children's data is involved, expect extra questions and extra time.

## Knowing what happens after release

- **Crash reporting.** The most useful thing on this page. Without it, a crash
  on a phone you do not own is invisible, and the only signal is a one star
  review a fortnight later. Sentry and Firebase Crashlytics both have free
  tiers.
- **Analytics, scoped deliberately.** Decide what question you are answering
  before choosing a tool. Whether people finish onboarding, which step they
  abandon, and whether they come back on day two are worth knowing. Whatever
  you collect has to appear in the privacy policy and both store forms, and in
  Europe non essential analytics need consent. Collect nothing sensitive.
- **A way for users to reach you** that is not a store review.
- **Watch the reviews for the first fortnight.** The first bad one usually
  describes a real bug before anyone reports it properly.

## The user's data

- **Decide whether losing the phone loses their data.** For a local first app
  the answer is often yes, and often nobody has said so out loud. Either build
  a backup or tell the user plainly that the export is the backup.
- **Test the export on a full account**, not an empty one. Hundreds of photos
  and a year of records behave differently from three test entries.
- **Check the storage limits you are near.** Phones limit how large a single
  stored value can be, and hitting the limit can fail silently while the app
  appears to work. Find out where your ceiling is before a real user does.
- **Test deleting everything, then reopening.** Deletion that leaves a copy is
  worse than no deletion button, because you claimed it in the privacy policy.

## Quality on real devices

- Test on a cheap Android phone, not only a fast one. Most people do not have
  new phones.
- Turn the system font size to maximum and look at every screen.
- Use the screen reader on the parts that matter most: signing up, paying, and
  anything urgent.
- Aeroplane mode. Nothing should hang waiting for a network that is not there.
- Add an error boundary so one broken screen does not blank the whole app.
- First launch on an empty phone, and a launch after an update on a phone with
  plenty of data. They are different, and the second one is the one that
  breaks.

## Things that take longer than you expect

Start these before you think you need to.

- Apple's developer account, which costs money every year and can take days to
  verify.
- Google Play's developer account, which now often requires testers before a
  new developer can publish.
- Setting up the subscription products in both stores, which is fiddly and has
  to match what the app asks for exactly.
- The first review, which is slower than every review after it.
