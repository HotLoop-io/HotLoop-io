# HotLoop brand standard

This is the canonical brand for everything HotLoop. Not the Fireball red, not
the EmberNET tokens. Those are a different company's house style, and HotLoop
is deliberately its own thing, because the license makes Embernet a commercial
partner rather than a parent.

`tokens.css` in this directory is the only copy of the palette that decides
anything. Both websites and each product's embedded UI vendor a copy and run a
CI step that diffs it. If you change a value here, the failing builds will tell
you where else it needs to land.

## The names

The brand is **HotLoop**. The products are **HotLoop Gateway** and **HotLoop
Flow**. On second reference inside a page about one of them, "Gateway" and
"Flow" are fine.

Bare "HotLoop" never means the Gateway on any page where Flow also appears.
This sounds pedantic until you write a sentence like "HotLoop connects to your
PLC" on a page that also sells a flow engine, and a reader has to guess which
product you are talking about.

The GitHub org display name is "HotLoop", not "HotLoop Automation". There is an
incorporated company called HuLoop Automation, Inc. selling an automation
platform into business process and RPA. We sell into industrial and OT, so the
buyers are different and the confusion argument is weak, but dropping one word
removes the closest point of collision and costs nothing.

## Color

### The two rules

**The accent is for interaction, never for state.** An orange accent means
warning cannot also be orange. Anything that says something about the plant
gets colored from the semantic ramp, and the alarm priority ramp contains no
orange at all. An operator should never have to work out whether a color means
"this is clickable" or "this is on fire."

This rule came from the Gateway's own stylesheet and it was right the first
time. It is now house standard.

**Contrast is a measurement, not an opinion.** Every value in `tokens.css`
carries its measured ratio in a comment. Before you add a color, measure it.
Before you change one, re-measure the pairs it participates in.

### The orange is two values, not one

`#FF5A00` on the light ground measures **3.00:1**. WCAG AA needs 4.5:1 for body
text. The first version of hotloop.io used it for every link, which is how an
entire site shipped below AA without anyone noticing, including the person who
built it.

So:

| Where | Token | Value | Ratio |
|---|---|---|---|
| Fills, gradients, the mark | `--hl-orange` | `#FF5A00` | n/a |
| Text and links, light theme | `--hl-link` | `#C2410C` | 4.96:1 |
| Text and links, dark theme | `--hl-link` | `#FF5A00` | 5.98:1 |
| Ink on an orange or amber fill | `--hl-on-accent` | `#1A1200` | 5.94:1 and 10.64:1 |

**The link color flips with the theme.** It is not one value with a tint. A
global swap to `#C2410C` would have measured 3.61:1 on the dark ground, which
moves the bug rather than fixing it.

Never put white on the brand orange. It measures 3.13:1. It looks fine in a
mockup and fails on a screen.

`--hl-amber #FFB700` measures 1.67:1 on light. It is a gradient stop and a
wordmark color. It never carries text, anywhere, in either theme.

### The semantic ramp has two halves

Same problem, same shape. `--hl-ok #16A34A` on the light ground is 3.16:1,
which fails exactly the way the orange does. So every semantic color has a base
value for fills, dots, and chart series, and a `-text` value that is the only
one allowed to carry type on light. Use the wrong half and you have
reintroduced the bug this whole file exists to prevent.

### The two oranges converged

The Gateway's UI used `#FF6A00` and the website used `#FF5A00`. Eight points
apart is the kind of thing nobody notices side by side and a designer
eventually asks about. They are now both `#FF5A00`, and the Gateway moved,
because on the light ground `#FF5A00` is 3.00:1 against `#FF6A00`'s 2.75:1, so
the HotLoop value is the better of the two exactly where both are weakest.

That change also fixed a real defect. The Gateway's `--accent-ink` was
`#FFFFFF` on `#FF6A00`, which measures **2.87:1** at a 13px base size. Every
primary button in the shipped product was below AA. It is now `#1A1200`.

## Type

**Inter Variable** for text, **JetBrains Mono** for code. Self-hosted via
`@fontsource`, never a CDN.

Two independent reasons, both real. A product whose entire argument is "do not
depend on infrastructure you do not control" cannot open every page with a
request to `fonts.googleapis.com`. And Emberburn already runs in air-gapped
clusters where that host is not slow, it is unreachable, which makes a CDN font
a blank page rather than a slow one.

**Declaring a family you never load is the specific failure to avoid.** The
Gateway's UI has named Inter since it was written and has been rendering in
Segoe UI the whole time, because nothing ever loaded it. There is no
`@font-face`, no link tag, and no `.woff` anywhere in its assets. A sibling
project in the estate has the identical bug. If you name a font, ship the font.

The type signature of this brand is the small uppercase section label:
`--hl-fs-2xs`, weight 600, `--hl-ls-label` tracking, in `--hl-link`. There is no
display face. A second family costs payload and a decision every time someone
adds a heading, and there is no argument behind it.

## Writing

The benchmark is the Flow README. It reads like a person because of a mechanism
you can measure, not because of tone:

| Document | Em-dashes | En-dashes |
|---|---|---|
| Flow README | **0** | 2, both numeric ranges |
| Gateway README | 17 | 0 |
| Gateway PROTOCOLS.md | 12 | 0 |

The document that reads most human contains zero em-dashes. It subordinates
with commas, and with "which is," "because," and "and that is." That is the
thing that makes it sound like speech rather than a press release.

**No em-dashes as connectors.** Comma, period, or parenthesis. En-dash only in
numeric ranges. Oxford commas throughout.

**American spelling on the websites and in this org.** Flow's source is
consistently British and stays that way; it is a coherent voice and rewriting
it would cost more than it returns. But "HotLoop Community License" is a named
legal instrument that appears on most public pages, and "licence" beside it
reads as a typo.

**Walk your own numbers back.** The Flow README publishes a benchmark table and
then spends three paragraphs explaining what those figures are not allowed to
say. That is the house move. Claim something, then name its limit before a
reader has to find it. It is also why the integrations grid ships a "Certified"
tier with a count of zero.

**Keep the directness, lose the profanity on public product pages.** The org
profile README swears and it works there. A plant manager's procurement
department reads `/gateway/`.

## The mark

An open loop with a break and a terminal dot. It reads as a control loop, it
survives 16px, and the whole file is under 500 bytes. It does not need
redesigning.

Three variants:

- `logo-mark.svg` is the default, gradient amber to orange.
- `logo-mark-16.svg` drops or enlarges the terminal dot, which is 0.96px at a
  16px favicon and disappears otherwise.
- `logo-wordmark.svg` is the mark plus the word.

Two rules for the wordmark. The word is **outlined paths, never a live `<text>`
element**, because a `font-family="Arial, Helvetica, sans-serif"` renders as a
different typeface on every platform and as something unpredictable inside a
GitHub README. And the dark half of the word is **`currentColor`, never a
hardcoded hex**, because a hardcoded `#1A1A1A` makes the wordmark invisible on
GitHub's dark theme, which is exactly where this file gets read.

Avatars are the mark alone. At YouTube's rendered 98px and Reddit's circular
256px the word is illegible, so only the wide banners carry the wordmark.

## Dark mode

One authored block applied by two selectors, so an explicit toggle and the
system preference cannot drift:

```css
@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) { ... } }
:root[data-theme="dark"] { ... }
```

The guard on the media query is what lets an explicit light choice win over a
dark OS setting.

Always set `color-scheme` on both themes so native selects, scrollbars, and
form controls follow. Always ship the blocking pre-paint script that reads the
stored preference before first paint, or the page flashes white on every
navigation.

Only grounds, text, and borders flip. **The accent does not change between
themes, and neither do the semantic hues.** Their `-text` variants do, because
the contrast requirement reverses.

## What stays divergent, on purpose

Write these down so a future consistency pass does not "fix" them:

- **Radius.** The websites use 16px cards. The products keep a tighter 6/10/14
  scale. Pill radii in a dense data table make it measurably worse to read.
- **Theme mechanism.** The Gateway uses `html[data-theme]`. Flow uses
  `body.dark-mode`, deliberately, because it mirrors the dashboard's own
  mechanism so it feels native inside the dashboard's iframe. Converge the
  token names and values, not the switching mechanism.
- **Flow's British spelling** in its own source and README.
- **`embernet.ai/*` label keys** on both charts. A namespaced key names the
  system that reads it, not the system that emits it, the same way
  `prometheus.io/scrape` goes on pods that are not Prometheus. Those labels are
  how the App Store finds us, and the App Store is the commercial channel.
