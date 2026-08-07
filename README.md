# Hearsay — prototype

A clickable sketch of an idea: an app that surfaces a handful of concerts near
you that you'd actually want to attend — including bands you've never heard of —
and explains in plain English why it picked each one.

**→ [Open the prototype](https://amackey.github.io/hearsay-demo/)**

Everything in it is fake. The venues are real rooms; the bands are invented.
Nothing is booked, nothing is charged, no account required.

Best viewed on a phone, where it fills the screen like a real app. On a desktop
it's shown inside a phone frame with the pitch alongside.

Three states to switch between: day one, a few weeks in, and a year later.

Feedback very welcome — there's a short form at the bottom.

---

## Pick your city

**22 markets** — 11 metros and 11 college towns and small scenes. Choosing one
swaps every venue in the prototype for a real room in that city.

The split is deliberate. This app is not for the Taylor Swift show; it's for the
250-person room and the band third on the bill. That's why Athens, Asheville,
Charlottesville and Burlington sit alongside Chicago and Seattle — small college
towns punch far above their population on exactly the thing this is built for.

Three things change with the city, and each is making a point:

- **The venues**, obviously.
- **Where the listings come from.** Each market names the real editorial
  calendar its shows would be pulled from — WXPN in Philadelphia, WWOZ in New
  Orleans, Flagpole in Athens, Isthmus in Madison. See
  `../03-MARKET-SOURCE-INVENTORY.xlsx` for the full research behind that.
- **How much it admits it doesn't cover.** Every source has a genre lean, so the
  week-three screen says so out loud — WWOZ is deep on jazz, funk and brass and
  thin on indie rock, and the app tells you that rather than quietly showing you
  less. Small markets also show **four** picks instead of five, with a line
  explaining why. Padding a short list would break the one promise the product
  makes.

### Adding or editing a market

Everything lives in the `MK` object at the top of the `<script>` block in
`index.html`, plus one `<option>` in the `#mk` select. Keys:

| Key | What it is |
|---|---|
| `city` `rad` `src` | Display name, radius in miles, the calendar listings come from |
| `lean` `thin` | The source's genre strengths and its blind spot |
| `sm` `sm2` `cap` | Small room with / without neighbourhood, and its capacity |
| `v1`–`v3` | Week-three venues |
| `y1`–`y5` | Year-two venues (`y5` only used when `n` is 5) |
| `loved` | The room the app has learned you rate up |
| `vol` `n` | Shows in the area that month, and how many get shown |

The skipped-show tally derives from `vol` and `n`, so the arithmetic can't drift.

> **Venue lists need a freshness check before you show a city to someone who
> lives there.** Small rooms close, and nothing is more damaging to a
> local-music pitch than naming a venue that shut two years ago. Every venue
> here was chosen for confidence, but none was verified as open on the day this
> was built.

## What the "a year later" screen is arguing

The three stages were always about *time*. The year-two screen now also carries the
ideas from revision 6 of the specs — each one chosen because it's a **perceived user
benefit**, not because it's interesting to build.

| On screen | Spec | The point |
|---|---|---|
| **"They opened for The Wilder Sons at AthFest in July"** | `PROD-48e` shared-stage recommendations | The most legible reason the product can give. It's **checkable**, it's **self-explaining** — you can judge the leap yourself instead of trusting us — and it's interesting whether or not you go. Compare "listeners who like X also like Y," which is true but opaque. |
| **"Heads up — well-known act, small room"** | `PROD-51`, `PROD-51a` | A sell-out warning that **doesn't lie**. We have no ticket-inventory data, so it says "shows like this usually go — that's a pattern, not a countdown." Never "selling fast," never "12 left." Fabricated scarcity is the thing this app is an alternative to. |
| **"$45 — above the ~$40 you set"** | `PROD-27` | The budget is a **soft ceiling**, not a filter. An exceptional match may exceed it, and must say so. An unexplained over-budget pick is a bug. |
| **"Price not listed"** (week 3) | `PROD-27a` | Unknown price is a **first-class state**. WXPN publishes no prices at all, so in Philadelphia this is the honest answer — never $0, never silently passing or failing the budget check. |
| **"It's a Tuesday, and they're on last"** | `PROD-76`–`78` | The gap this found in the old model: a 9pm Tuesday show is **not a calendar conflict** — the calendar at 9pm Tuesday is empty. It's a problem at 8am Wednesday. And it **warns, never filters** — you decide. |
| **"Do you remember who opened?"** (week 3) | `PROD-68`, `KG-60` | Community curation, **only asked of people who marked attendance**. Local openers are the hardest edge to source and you were in the room. "Don't remember" is as easy to tap as an answer, on purpose. |
| **Ticket-budget bar** in "what it's learned" | `PROD-79` | Tolerance is **learned, not configured** — "stretched twice this year; both times you went." |

Two things deliberately **not** in the demo, because they're invisible to a user:
the collaborative-filtering staging (`RECSYS-SPEC` §7) and the knowledge-graph
provenance model (`KG-SPEC` §3). They shape what the app can honestly say; they are
not features.

> **Festival names are real; the bands and the co-bills are invented.** Same caveat as
> the venues — verify before showing a city to someone who lives there.

### What the feedback form now captures

Three additions: which market they were viewing, a free-text "is your city on the
list — if not, which one?", and a **multi-select on which revision-6 features
actually matter** (`feats`). Market and city ride inside the existing `screen`
field so they arrive **without any change to the Apps Script**; all three are also
sent as their own `market`, `city` and `feats` keys — add columns and fields to the
script when you want them broken out properly.

The two highest-value questions on the form right now: **demand for a market you
haven't considered** is the cheapest signal you can buy, and **which of the six
features people actually pick** tells you what to build after M0 — several cost real
work, and at least one (the sell-out heads-up) can never be backtested, so stated
preference is the only evidence you will get.
