# Research backlog

Ranked. Items 1 and 2 sit at the same rank: both are prerequisites for
believing any expectancy number, and neither should wait on the other.

Everything below item 2 is subject to the overfitting guardrails recorded at
the end of this file. **The holdout is untouched and is spent once.**

---

## RANK 1 — Is the strategy tradeable at all?

**Full write-up: `selection_selects_for_illiquidity.md`**

The selection rule may be selecting for illiquidity. First session with quote
capture: 4 of 8 candidates tradeable on a majority of quotes, two never, and
YJ — the top-ranked gainer at +146% — tradeable on 3.6% of its quotes with a
median displayed offer of $244. CHMI, found independently by a real-time
scanner rather than by rank, was tradeable on 0.0%.

**Needs:** Databento MBP-10, $39.45, 7.5 GB, 6,802 candidate ticker-days.
**Cannot be answered from our own capture** — quote history begins
2026-08-10 04:44 ET. Every prior session is trades only.

**Question:** across 484 days, for candidates the strategy actually traded,
what was displayed size at trigger, what would 25% of the thinner side have
permitted, what fraction clear a $500 floor, and does tradeability correlate
with prior-day rank?

---

## RANK 1 — Is the exit band inside the instrument's noise?

**No new data required. Runnable today against the existing 484-day tape.**
This is why it ranks alongside item 1 rather than below it: the mechanism is
plausible, the test is free, and the effect may be larger.

### The observation

Both reconstructed trades resolved in under two seconds, by noise rather than
by thesis:

| | ZJYL | CHMI |
|---|---|---|
| stop touched | **1.54s** after entry | **1.35s** after entry |
| target | never printed in 30 min | printed **52s AFTER the stop** |
| MAE | −21.28% | −4.83% |
| MFE | 0.00% | **+5.19%** |

CHMI traversed −4.83% to +5.19% within 53 seconds — a realised one-minute
range of **10.0% of entry, against an exit band 8% wide** (−3% stop to +5%
target). When both barriers are touched inside a minute, which one "wins" is
an ordering accident, not information.

The two cases differ in exactly the way that matters: ZJYL kept falling
(stop did its job), CHMI recovered past the target (stop threw away a winner).
Which case is typical is the whole question.

### Relevant contrast

From `claude/execution-and-hosting.md`: Cameron uses **mental** stops of 10–20
cents, not automatic ones. On a $2.80 name that is 3.5–7%, wider than our 3%,
and discretionary **specifically so noise does not pick him off**. We run a
tighter *hard* stop on the same class of instrument.

### The test

For every historical signal, from the existing tape:

1. Time from entry to first touch of −3%, and to first touch of +5%.
2. What fraction touch **both** inside the 30-minute window, and in what order.
3. Distribution of realised range over the first 60 seconds after entry,
   compared with the 8% band width.
4. Expectancy under the current rule versus: a wider stop; a **time-delayed
   stop that does not arm for the first N seconds**; and no stop with only the
   30-minute timeout.
5. How much outcome variance is explained by which barrier was touched first,
   versus by where the name ended up at timeout.

### How to report it

**As a distribution, not an average. State n.** A rule whose mean outcome is
fine but which stops out 60% of eventual winners in the first two seconds is a
different animal from one that stops out uniformly, and the mean hides it.

**Do not tune parameters off this run.** The output is the shape of the
problem. Any tuning happens afterwards, inside the guardrails, with the
holdout untouched.

### Caveat that must appear in the write-up

**n = 2 reconstructed trades is not evidence.** The reason to run this is that
the mechanism is plausible and the test is free — not that two trades
established anything. If the distribution shows most stops are followed by
continued decline, the stop is doing its job and CHMI is simply the exception.

---

## RANK 2 — fillna(1.0) correction

The current expectancy baseline may itself be wrong. **No result from any rank-3
item changes what we trade until this lands** — comparing new rules against a
broken incumbent is worse than not testing.

## RANK 3 — The rank-cut grid

Rank is a quota, not a filter: top-5 supplies ~5 candidates a session
regardless of conditions, so count is held constant and quality floats. Frozen
2-D grid over 484 days — rank thresholds {3,5,10,15,20,30} × absolute floors
{none,+10,+15,+20,+30,+50%} and relative floors {1.0,1.5,2.0,3.0}× the day's
top-20 median. Plus the three-way decomposition: rank alone, gain alone, both.

**Blocked on rank 2.** Also now entangled with rank 1: the book cap disqualifies
roughly half the universe, and Sharpe scales with the square root of trade
count, so a wider net may be needed simply to have anything left.

## RANK 3 — Condition outcome on day breadth

Bucket sessions by how many names cleared the bar (1, 2–3, 4–6, 7+); hit rate
and expectancy net of cost per bucket. Two opposite hypotheses, both plausible:
more catalysts vs more crowding. Scanner overlap is already a monotone exit
signal across 35,000 events; if that is crowding rather than name-specific,
market-wide breadth should show the same shape.

## RANK 4 — Day-of-week split

Monday's gap from the prior session is three days, not one. Never checked.

## RANK 4 — Venue / edge rerun on the filtered subset

Rerun on the strategy subset, not the gated population.

---

## Overfitting guardrails — apply to every item above rank 2

1. **Grids are frozen before running.** No threshold, bucket or variant added
   after seeing a result. A better axis becomes a separate pre-registered test
   with its own holdout.
2. **The holdout is untouched.** Most recent 20% of sessions by date. Nothing
   computed on it, nothing looked at. Spent once, on the final candidate rule.
3. **Minimum sample.** Any cell with fewer than 100 signals is "insufficient"
   and carries no weight whatever its hit rate.
4. **Prefer a plateau over a peak.** Judge by whether good cells form a smooth
   contiguous region. A high cell surrounded by bad ones is noise — the same
   reasoning that made the 42-cell target/stop result credible at 27 of 42
   positive.
5. **Report the comparison count** and state how much of the best cell's
   advantage could be explained by having tested that many. If the best cell
   beats the incumbent by less than the spread among its neighbours, say so.
6. **A new rule replaces the incumbent only on the untouched holdout, by a
   margin** — not nominally.
7. **Nothing changes what we trade until fillna(1.0) lands.**

~120 cuts have already been run against this data. Items in rank 3 add ~70
cells. Without the above this becomes a fishing expedition with a
pre-registration fig leaf.
