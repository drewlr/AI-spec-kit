---
name: dashboards
description: >-
  Build and review a dashboard of charts, in any tool, so that somebody can read
  it and decide something. Use this whenever you create, edit or review a
  dashboard, an insight, a tile, a report or a chart that a person will check
  more than once. It covers what belongs on a dashboard and what does not,
  which chart form answers which question, how colour and legends are chosen,
  how one filter row scopes every tile, and the checks to run before calling it
  finished. It also covers the sets of questions a product dashboard is built
  from, so a new dashboard starts from a decision somebody has to take rather
  than from the data that happens to exist. Triggers on: dashboard, tile,
  insight, chart, graph, report, metric, KPI, funnel, retention, cohort,
  breakdown, legend, filter, PostHog, Amplitude, Mixpanel, Looker, Metabase,
  Grafana, Tableau.
---

# Dashboards

A dashboard is a page somebody opens again and again to decide something. A
question asked once is answered in a document or a notebook, and a dashboard
answers it with a wall of tiles and no conclusion.

Read this before you make the first tile, because most of what goes wrong is
decided before any chart is drawn.

## Answer two questions in writing before you build anything

**What is this dashboard for, and what does the reader come away knowing?**
Write both answers down before the first tile, put them in the dashboard's own
description, and repeat them in a text tile at the top of the page. A dashboard
whose goal you cannot state in one sentence is a dashboard that will collect
whatever is easy to count.

The goal names the decision the reader has to take. The takeaway names what they
know after ten seconds of looking, which is usually one number and one shape. If
the two answers do not agree, the tiles are wrong rather than the writing.

**Write every dashboard name, tile title and description in the project's plain
writing style**, and load the `plain-writing` skill before you write them. A
title says what the tile is about in the words the reader uses. A description
says what the number decides and which people it leaves out. Neither is the
place for a clever name, and a tile called something clever is a tile somebody
has to open to understand.

## Start from the decision, not from the data

**Write down the decision each tile drives before you build it.** A tile whose
answer changes nothing is a tile to delete, however interesting the number is.
Put the decision in the tile's own description, where the person reading the
chart can see it.

**A dashboard covers one subject and one reader.** Money, product use and
faults are three dashboards rather than three rows of one. A reader who has to
scroll past what they did not come for reads less of what they did.

**Keep it to about five to nine tiles**, and to one screen where you can. A
dashboard that needs scrolling is two dashboards. Stephen Few's first named
mistake in `Information Dashboard Design` is exceeding the boundary of a single
screen, and it is still the commonest one.

**Order the tiles the way they are read.** The headline numbers go at the top,
the breakdowns that explain them in the middle, and the diagnostics at the
bottom. Somebody checking the dashboard in ten seconds reads the top row only,
so the top row has to be the answer.

**A total with no denominator decides nothing.** Installs decide nothing;
installs against listing views decide whether to rewrite the listing. Put the
rate next to the count, or put the count next to the rate, and never ship one
alone.

**Say what a number does not cover.** A figure worked out from signed in
accounts is not a figure about everybody who uses the app, and a reader will
take it for one unless the tile says otherwise. Write the limit into the tile's
description rather than into a document nobody opens.

**A vanity number is one that only ever goes up.** Totals since the beginning,
cumulative installs and lifetime page views all rise whatever happens, so they
cannot tell you that something got worse. Prefer a number that can fall.

## Which form answers which question

Pick the form from the question, and pick the colours last.

| The question | The form |
| --- | --- |
| One current value | A stat tile. The number is the chart. |
| How it changed over time | A line per series. |
| How categories compare | A bar per category, side by side, sorted longest first. |
| How a whole splits | A bar per part, side by side, with the total as its own tile. |
| How many get from one step to the next | A funnel. |
| Whether people come back | A retention grid or a curve. |
| How two measures relate | A scatter plot. |
| Anything with more than about eight categories | A table, or a bar chart of the top few with the rest folded into one row. |

**Never a pie chart.** People compare lengths accurately and angles badly, so a
bar chart is a more accurate read of the same numbers. Two slices is a stat
tile with a percentage on it. More than about six is unreadable at any size. A
pie also forces a separate legend, because the labels do not fit in the slices,
and the reader's eye then travels between the legend and the chart once per
slice.

**Never a stacked chart**, whether it stacks bars or fills areas. Only the
bottom band of a stack sits on the axis, so only the bottom band can be read.
Every band above it starts wherever the band below it happened to end, and
comparing two of those means comparing two lengths that share no baseline, which
people do badly. A stack also hides the thing a reader most often wants, which
is one band's own shape over time.

Use one line per series for a time series, and bars side by side for a
comparison. Where the total matters as well as the parts, give the total its own
tile rather than stacking the parts to make one.

**Never a one bar bar chart.** Use a stat tile.

**Never two vertical axes on one chart.** The two scales are lined up
arbitrarily, so the chart shows a relationship that is not in the data. Use two
charts, or put both measures on one axis by indexing them to a common starting
point.

**A bar chart's axis starts at zero.** A bar says how big something is by how
long it is, so cutting the bottom off the axis makes a small difference look
large. A line chart may start elsewhere, because a line says how something
changed rather than how big it is.

**Sort a category chart by size, not alphabetically**, unless the categories
have an order of their own such as age bands or funnel steps, in which case use
that order.

## Colour

**Colour carries identity, not size.** One series is one colour. Colouring each
bar darker where it is bigger says the same thing the bar's length already
said, and it uses up the only channel left for saying something new.

**Give different categories different colours, and keep each category's colour
the same on every tile.** If pregnancy is blue on one chart it is blue on all of
them. Colour follows the thing rather than its position in the list, so
filtering one category out must not repaint the rest.

**Show a legend whenever there is more than one series.** One series needs no
legend, because the title names it. Never leave colour as the only way to tell
two things apart: label the bars directly as well, or provide the table
underneath.

**Choose colours somebody who cannot tell red from green can still read.** Do
not judge that by eye. The `dataviz` skill ships a validator and a validated
eight colour order. If you have no validator to hand, the Okabe and Ito eight
colour set is the safe default and is published for this purpose.

**Keep status colours for status.** Red, amber and green mean bad, warning and
good. Using red as the colour of the fourth category makes a reader think
something is wrong. Where a colour does mean good or bad, put an icon or a word
beside it, because about one man in twelve cannot read the colour alone.

**The chart furniture recedes.** Gridlines and axes are one shade off the
background and solid rather than dashed. Text stays in the ordinary text
colour, and the coloured mark beside it carries the identity.

## One filter row, scoping everything

**Put the date range and the property filters at the top of the dashboard, and
let every tile obey them.** A reader who narrows the date and sees half the
tiles change has no way of knowing which half is answering the question they
asked.

**A tile that deliberately ignores the dashboard filter says so in its own
description.** A daily count sent by a server, or a tile whose job is to show
everything that has ever arrived, is a fair exception, and an unexplained one
is a bug.

**Check that the filter actually reaches every tile.** In most tools a tile
written in SQL ignores the dashboard's filters unless the query asks for them.
See the PostHog section below for the exact form.

## Before you call it finished

- Open the dashboard and look at it. The validator checks colour, not whether a
  label is cut off or a chart overflows its tile.
- Change the date filter at the top and watch every tile change.
- Read the dashboard's goal and takeaway at the top, then look at the tiles. If
  a tile answers neither, it belongs on another dashboard.
- Read each tile's title and description on their own, without the chart. If
  either fails to say what the tile answers, rewrite it.
- Check that data is arriving at all. Keep one tile that lists every event
  received, by name, and look at it first when another tile looks wrong. A tile
  built on an event nothing sends is an empty chart rather than an error, and it
  looks exactly like a real zero.
- Check the tiles that are empty on purpose, and say in their description why.

## The sets of questions a product dashboard comes from

A product dashboard is built from the decisions the product owner has to take.
Four published frameworks name those decisions, and they overlap, so pick one
and write your own sets from it rather than using all of them.

- **AARRR**, by Dave McClure. Acquisition, activation, retention, revenue and
  referral. Written for a company that has to find out which part of its funnel
  leaks, and it is the one that fits an app with a subscription.
- **HEART**, by Google. Happiness, engagement, adoption, retention and task
  success. Written for measuring an experience rather than a business, so it
  fits the question of whether a screen works.
- **A north star metric**, popularised by Sean Ellis. One number that states the
  value the product delivers, with three to five input metrics under it. It is
  a way of agreeing what matters rather than a set of charts.
- **Cohorts and funnels** are techniques rather than frameworks, and both belong
  on almost any product dashboard. A cohort groups people by when they arrived
  and follows each group separately, which is the only way to see that a change
  helped, because a total mixes old and new arrivals together.

For a subscription app the three input numbers that move the business are
activation, retention and conversion, in that order. Day 30 retention of a
group of new installs is the single most predictive figure, and it cannot be
read until the group is thirty days old.

## PostHog

The rules above hold in any tool. What follows is what PostHog does
differently.

**A dashboard level filter reaches a trends, funnel or retention tile on its
own. It does not reach a tile written in SQL unless the query asks for it.**
Put the `{filters}` placeholder in the `where` clause, and it expands to the
dashboard's date range, its property filters and its test account setting:

```sql
select toStartOfDay(timestamp) as day, count() as visits
from events
where event = 'session_started' and {filters}
group by day order by day
```

A SQL tile with a hard coded `timestamp > now() - interval 90 day` ignores the
filter row completely, and the dashboard's date picker then appears to do
nothing.

**Colour per series in a SQL tile is a hex value you set yourself**, at
`chartSettings.yAxis[n].settings.display.color`. To give each category its own
colour, return one column per category rather than one column and a breakdown,
and set a colour on each column. A legend comes from `chartSettings.showLegend`.

**Colour per series in a trends or funnel tile is set through
`resultCustomizations`**, which is the colour picker beside each row of the
detailed results table. Only trends and funnels support it. A whole custom
colour theme needs a paid plan; assigning a colour to one result does not.

**Chart types worth knowing by their internal names.** `BoldNumber` is a stat
tile. `ActionsLineGraph` is a line. `ActionsBar` puts several series side by
side, which is the one to use. `ActionsBarValue` is the total for each breakdown
value over the whole period, which is the right form for a category comparison.
`ActionsTable` is a table. `ActionsPie`, `ActionsStackedBar` and
`ActionsAreaGraph` all exist and this skill says not to use any of them, because
a pie asks the reader to compare angles and the other two stack.

**A trends tile stacks its breakdown by default.** Set
`trendsFilter.showPercentStackView` and `trendsFilter.stackBreakdownValues` to
false, and pick `ActionsLineGraph` or `ActionsBarValue` rather than the bar over
time, which stacks whatever you do.

**Unique users means unique senders.** If the app posts events itself with a
per visit identifier rather than through an SDK, then every count PostHog calls
users is a count of visits. Say so on the dashboard, in a text tile at the top,
because every chart in the tool is labelled with the tool's word rather than
yours.

**A text tile at the top of the dashboard is the place for what the reader has
to know before reading any number.** What the identifier means, which group of
people the numbers cover, and which tile to check when something looks wrong.
