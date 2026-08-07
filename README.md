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

### What the feedback form now captures

Two additions: which market they were viewing, and a free-text "is your city on
the list — if not, which one?". Both ride along inside the existing `screen`
field, so they arrive **without any change to the Apps Script**. They are also
sent as their own `market` and `city` keys — add two columns and two fields to
the script when you want them broken out into their own columns.

That second question is the most useful thing on the form right now. Demand for
a market you haven't considered is the cheapest signal you can buy.
