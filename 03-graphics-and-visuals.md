# Graphics and visuals

> Where to find design worth learning from, and how to point an agent at an
> example instead of asking it to invent one.

If you cannot afford a designer, start from work that already sells in your
market rather than from a blank screen. Pick an example from one of these sites,
give it to your agent, and ask for the same structure with your own content.

## Where to look

| What you need | Where |
| --- | --- |
| Mobile apps | https://mobbin.com |
| Icons | https://hugeicons.com |
| Landing pages | https://landing.love |
| SaaS websites | https://saaspo.com |
| Navigation bars | https://navbar.gallery |
| Calls to action | https://cta.gallery |
| Web design | https://curated.design |
| Brands and rebrands | https://rebrand.gallery |
| Anything, loosely | https://dribbble.com |

## Point the agent at one example, not at the idea

Give the agent the URL or a screenshot, and say which parts to take. Ask for the
structure, e.g. the order of the sections, where the price sits, how much text
comes before the first fold, and what the first screen asks the person to do.

Then say what is yours: your product, your words, your colours, your fonts.

**Why:** an agent asked for a landing page with no reference produces the
average of every landing page it has seen, which is a page nobody remembers and
nobody can say what is wrong with. Given one example and told which parts to
take, it produces something specific enough to argue about.

## Take the structure, leave the identity

- Do not take a name, a logo, a typeface or a colour palette. Copying the layout
  of a page is ordinary practice. Copying the identity is not, and both app
  stores reject apps that look like somebody else's.
- Do not take the words. They describe their product, not yours.
- Do not take screenshots that have real people or real customer names in them.

## Check it on a real screen before you believe it

A design that looks right in a browser on a laptop can be unreadable on a cheap
phone in daylight. The checks in [before a mobile app goes
live](./02-mobile-launch.md) apply here as well, and the two that catch most of
it are turning the system font size to maximum and opening every screen, and
looking at the thing on a five year old phone.

## Render a store picture at the size the store shows it

A store graphic has one size in the file and another on the screen. Google Play
gives its feature graphic about the width of a phone, which is roughly 390
pixels, so a 1024 pixel file is shown at about a third of the size it was drawn
at, and text under about 6 pixels at that size stops being readable.

Work out which type survives by rendering the file at the width the store shows
it and looking at it. Do not work it out on paper. Two attempts at that on one
project were wrong, because three strings wrapped onto a second line that were
not expected to, and the result overflowed the canvas by 51 pixels, which the
store's crop would have cut off.

Once the size is known, decide what each part of the picture is for. A section
title large enough to read names the feature. Anything smaller than that shows
what the feature looks like without being read, which is a real job as long as
nothing important is said only there.

## Give an exported picture its font from a file, not from the network

A design exported through a headless browser will ask the network for a web font
if the source asks for one. When that request does not finish, and in a sandbox
it often does not, the browser falls back to a system font and does so silently.
A fallback font is wider, so lines wrap where they did not wrap on screen, and
weights the fallback lacks come out synthesised and heavier.

The picture still exports and still looks plausible, which is why nobody catches
it until somebody puts the export beside the design. Point the export at font
files on disk. If the app bundles the same font, use the app's own copies, which
also guarantees the picture matches the product.
