# The selection rule may be selecting for illiquidity

**Status:** open question, ranked ABOVE the rank-cut grid in the research backlog.
**Raised:** 2026-08-10, from the first session in which we captured quotes.
**Claim type:** about the SELECTION RULE, not about execution.

---

## The claim

The rule picks yesterday's top gainers, filtered to under 20M shares and $2–10.
The same properties that put a name at the top of that board — small float, a
violent move — are what leave almost nothing resting on the offer. If that
holds generally, **the edge exists in names we cannot trade at size, and no
execution improvement fixes it.**

This is not a claim that execution is hard. It is a claim that the selection
criterion and tradeability may be *negatively correlated by construction*.

## The evidence

First session with quote capture, 36,149 quotes across the candidate universe,
04:44–06:40 ET. Order sizing is 25% of the thinner side of the round trip,
$500 minimum, 2% equity cap:

| symbol | tradeable on | median order | prior-day rank |
|---|---|---|---|
| AXTL | **93.1%** of quotes | $1,996 | 21 |
| AXTU | **85.3%** | $1,994 | 19 |
| SPAX | 62.4% | $1,773 | 5 |
| RCAX | 50.5% | $600 | 35 |
| XHLD | 19.0% | $803 | 37 |
| **YJ** | **3.6%** | $1,178 | **1** |
| CSAI | **0.0%** | — | 11 |
| VSA | **0.0%** | — | 16 |

**4 of 8 tradeable on a majority of quotes. Two never tradeable.**

The headline: **YJ was the top-ranked gainer at +146%, and its median displayed
offer is $244.** It is tradeable on 3.6% of its quotes. CSAI shows a median
spread of 2,099 bps — a 21% spread is not a market, it is two quotes that
happen to exist.

The suggestive part is the ordering. The two most tradeable names, AXTL and
AXTU, ranked **21st and 19th**. The least tradeable, YJ, ranked **1st**.
n = 8, so this is an observation, not a result — but it points the right way to
be worrying.

## A second data point, from real-time discovery

CHMI was found by Warrior's Momo scanner at 06:30 ET — not from yesterday's
rank — and added to our quote subscription live. Median thinner side **$828**,
so 25% is **$207**: below the $500 floor, **tradeable on 0.0% of its quotes**.

That matters because it is a different selection path reaching the same place.
It weakens "our rank rule is the problem" and strengthens "names that move
violently pre-market are thin, however you find them".

## What would settle it

**Across the 484-day research set, for the candidates the strategy actually
TRADED, what was the displayed book at trigger time?**

We **cannot answer this from our own capture.** We hold no historical quotes —
quote capture began 2026-08-10 04:44 ET. Every prior session is trades only.

That makes it the **Databento MBP-10 pull**: $39.45, 7.5 GB on disk, 6,802
candidate ticker-days. This is now the strongest reason to spend it — stronger
than the original "would a limit at the breakout level have filled", because
this asks whether the strategy has ever been tradeable at all.

Specifically, per traded candidate at trigger time:

1. displayed size at the offer and at the bid
2. what 25% of the thinner side would have permitted
3. what fraction of historical signals clear a $500 floor
4. whether tradeability correlates with prior-day rank — the AXTL-21st /
   YJ-1st ordering above, tested at n = thousands rather than n = 8

## Why this outranks the rank-cut grid

The grid asks *which* rank cut yields the best signals. This asks whether the
signals it finds can be traded. If a large fraction of historical candidates
were never tradeable at size, then:

- every backtest expectancy is computed over trades that could not have
  happened, and the +0.57% headline is overstated by an unknown amount;
- the grid would be optimising a rule over a population that is partly
  fictional.

**Answer this first.** Optimising the selection rule before knowing whether its
output is tradeable is optimising the wrong thing.

## The compounding problem

The book cap disqualifies roughly half the universe. The rank cut already
yields only 1.6–2.0 signals per session. Sharpe scales with the square root of
trade count, so halving an already-thin universe is expensive.

That makes the top-5-versus-top-20 question **urgent rather than academic**: we
may need a wider net simply to have anything left after the book test. But
widening it is only worth doing if the wider names are more tradeable — which
is the same measurement as above.

## Caveats

- **n = 8 names, one session, ~2 hours.** Directional, not conclusive.
- **Displayed size is a lower bound on real depth.** Hidden and reserve orders
  mean true depth is usually greater, so these figures are conservative. The
  ranking between names should survive, the absolute levels may not.
- **Pre-market only.** Regular-hours books on the same names are far deeper;
  none of this describes 09:30–16:00.
- CSAI's figures rest on 14 quotes and are excluded from pooled statistics.
