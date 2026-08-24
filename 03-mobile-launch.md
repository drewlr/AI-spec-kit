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

## Render a store picture at the size the store shows it, not at the size you drew it

A store graphic has one size in the file and another on the screen. Google Play
gives its feature graphic about the width of the phone, which is roughly 390
pixels, so a 1024 pixel file is shown at about a third of the size it was drawn
at. Text under about 6 pixels at that size stops being readable.

Work out which type survives by rendering the file at the display width and
looking at it. Do not work it out on paper. Two attempts at that on Baby What
were wrong, because three strings wrapped onto a second line that were not
expected to, and the result overflowed the canvas by 51 pixels, which the store's
crop would have cut off.

Once the size is known, decide what each piece of the picture is for. A section
title large enough to read names the feature. Everything smaller than that shows
what the feature looks like without being read, which is a real job as long as
nothing important is only said there.

## Give an exported picture its font from a file, not from the network

A design exported through a headless browser will ask the network for a web font
if the source asks for one. When that request does not finish, and in a sandbox
it often does not, the browser falls back to a system font, and it does so
silently. A fallback font is wider, so lines wrap where they did not wrap on
screen, and weights the fallback lacks come out synthesised and heavier.

The picture still exports and still looks plausible, which is why nobody catches
it until somebody compares the export with the design side by side. Point the
export at font files on disk. If the app bundles the same font, use the app's
own copies, which also guarantees the picture matches the product.

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

## Check that a keyword has demand before you celebrate having no competition

Keyword research produces two numbers, how many people search for a phrase and
how hard it is to rank for it, and only the second one is easy to measure. You
can read the competition straight off the store by looking at who ranks and how
big they are. Search volume is not published by either store, so it is tempting
to work from competition alone.

Do not, because the two are not independent. A phrase with no competition
usually has no competition because nobody searches for it. Research for one app
produced a list of phrases with almost no competing apps, and every one of them
turned out to be a phrase the App Store never offers as a completion, which is
the store saying the search is not being made. The list looked like the best
finding in the work and was worth nothing.

There is a free check. Type the phrase into the store and see whether it
appears in the suggestions, because a store only suggests completions people
actually search for. Check the exact wording you plan to use rather than one
that means the same thing, since "breastfeeding tracker" and "breastfeeding
tracker app" are not the same phrase to a store and only one of them was
suggested. Apple Search Ads then gives a real popularity score for free once
you have a developer account, and Google Play Console reports the phrases that
actually brought people to your listing once you are live.

**Why:** targeting a phrase nobody searches for costs the same effort as
targeting one people do, and you find out only after the listing is live.

## The marketing site, and three ways it fails to deploy

Every app needs a website before it can be submitted, because both stores want
a privacy policy address and a support address, and because the deep link files
have to be served from a real domain. It is five static pages and it still goes
wrong in the same three ways.

**Never put the deploy config inside the directory it publishes.** A config file
sitting in the published directory is served to the public at its own address,
and a local preview server that watches the directory sees its own config
change, reloads, and loops without ever answering a request. Put the published
files in a subdirectory and the config beside it, not above it and not in it.

**Point the build at the website, not at the top of the repository.** Hosts
install the dependencies of whatever directory you point them at. If the website
lives in a repository that also holds the app, pointing at the top means
installing the whole mobile app on every deploy in order to publish a few HTML
files. A website directory with no dependency file installs nothing.

**The branch the host watches must actually contain the site.** Hosts build the
default branch. Work done on a feature branch is not on it. This fails on the
first deploy, before anything else has been configured, and the error rarely
says which of the two things is missing.

Two more that cost a day each when missed. A host that serves a missing page
from `404.html` because it noticed the filename is doing you a favour that the
next host will not: check whether yours needs telling. And a host that quietly
skips files in directories beginning with a dot will skip
`.well-known/apple-app-site-association`, which is the file that makes every
invite link open the app rather than the website, and nothing will report it.

## Test the host by running it, not by reading about it

Hosting products get rebuilt and renamed, and their documentation lags the
dashboard. The instructions you wrote a week ago may describe a flow that no
longer exists, and the question you actually need answered is usually the one
the documentation does not cover.

Both are settled the same way. Every serious static host ships a command that
serves your real directory the way the host will. Run it against the real files
and ask for every address: each page, an address that does not exist, the
config file, and anything under a dot directory. Ten minutes of that replaces
an afternoon of reading and is the only thing that can tell you what the
documentation has left out.

Write down what you checked, with the answers. The next person will otherwise
assume the host has behaved this way all along, and will find out that it has
not in the same expensive way you nearly did.
