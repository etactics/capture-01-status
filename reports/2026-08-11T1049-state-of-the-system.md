# State of the system — 2026-08-11

**Trading is halted.** The paper bot is stopped and disabled. Capture, the
tunnel, the dashboard and the dead-man's switch are still running. The account
is flat: zero positions, zero working orders, equity $100,039.66, confirmed by
reading the broker directly rather than trusting anything local.

This document is for someone deciding whether to continue. It is written in
plain language and it tries hard not to flatter the work.

---

## The one thing to read if you read nothing else

**No result produced so far is evidence about the trading strategy.**

Six trades were taken. Every one of them was taken by a system that was being
modified the same day, and the two trades that dominate the profit and loss
were both caused by our own defects rather than by anything the market did.
The strategy has never run unchanged for a single complete session.

The reason is not that the strategy is bad. It is that we have not yet
measured it. Those are different statements and only the second one is
supported.

---

## What works, proven by watching it, not by asserting it

**Market data capture.** Two days, continuously, and it is the only component
that has behaved. It has recorded every trade on the US market plus the order
book for a selected list of symbols. Today alone it took 73.8 million
messages and 10.4 GB. When we broke it, the raw journal let us rebuild what
was lost. This is the asset. Everything else is replaceable.

**The order path end to end.** Signal, order, acknowledgement, fill, exit,
flat. Six complete round trips, both exit types exercised, no rejected orders
except ones we caused. Every fill matched the account to the penny. Orders
acknowledged in about 120 milliseconds and filled at or better than the price
we asked for. The plumbing works.

**Recovery from our own mistakes.** When a restart destroyed 72 seconds of
recorded data, it was rebuilt from the raw journal and verified by counting
the restored rows, not by assuming the tool worked.

**Restart safety.** The bot now survives being killed: it re-adopts positions
already held, cancels orders left behind, and recovers the levels it was
managing them with. Verified by killing it and watching it come back.

---

## What is built but has never been proven

- **The trading rule itself.** It has never run for a full session in one
  unchanged form. Its main threshold was changed today, from a value that was
  impossible to satisfy to one derived from a single day of data.
- **The alert reaching a human.** The watchdog correctly went quiet when the
  bot died. Whether that quiet ever became a phone alert is unknown, and an
  investigation step accidentally reset the evidence. Treat alerting as
  unproven.
- **That our fills could exist in a real market.** Paper trading fills orders
  that a real market would not. One order today was more than twice the size
  showing on the offer. We now record this on every order, but we have not yet
  collected enough to say which of our fills are realistic.
- **Whether the names the strategy picks can be traded at size.** An earlier
  finding suggested the selection rule may favour names too thin to trade.
  Still open.

---

## Every defect found in the last two days

Nine in total. Six were introduced by this work; three were already there.

| # | What broke | Consequence |
|---|---|---|
| 1 | Entry rule compared a move to a number that is always bigger than it | Rule could never fire. Two sessions, zero trades |
| 2 | "Is the price data fresh?" measured the wrong clock | Blocked nearly every order for a reason unrelated to freshness |
| 3 | Minimum order size fought the maximum order size | Ordinary trades refused |
| 4 | A quiet stretch made the rule's yardstick almost zero | Bought with a target and stop a third of a cent apart. Lost $149 |
| 5 | Every reconnection silently dropped the order book feed | Two blackouts, 16 and 28 minutes, invisible at the time |
| 6 | Writing data files blocked the network connection | Dropped market data, connection dropping every two minutes |
| 7 | A restart overwrote the previous run's data files | 72 seconds destroyed. Recovered from the journal |
| 8 | Restarting forgot orders already at the broker | Nearly sold the same shares twice. The broker refused it |
| 9 | Bot was a terminal session, and its watchdog watched the wrong hours | Bot dead five hours, nothing noticed |

### The shape they share

Seven of the nine are the same mistake: **a number compared against the wrong
thing, failing safe, looking careful.**

The entry rule demanded a move be 2.5 times bigger than a quantity that is
mathematically always larger than the move. The freshness check demanded data
under 5 seconds old measured against a clock that runs about 15 seconds
behind. The size floor demanded a minimum that the size ceiling forbade.

In every case the code refused to act and the refusal read as prudence. In
every case the system reported healthy. Nothing looked broken, because from
the inside, "correctly declining to trade" and "unable to trade" produce the
same logs.

**This is the thing to carry forward.** The failures were not crashes. They
were confident, quiet, and wrong, and they survived because a safe-looking
refusal is nobody's first suspect. A guard that has never once let something
through has not been shown to be safe. It has not been shown to work at all.

---

## What each trade actually measured

| Trade | Result | Where the result came from |
|---|---|---|
| PCLA | **+$227.25** | **A defect.** Bought 15 seconds after a trading halt lifted, halted again 5 seconds later, frozen 5 minutes. The 10-minute window behind the decision contained a 5-minute hole with no trading, so the "move" was mostly the hole. The profit came from the reopening auction |
| LITZ | −$21.00 | The market. A normal losing trade, cleanly stopped |
| MPAA | −$11.27 | The market, but the trade should not have been taken twice more |
| MPAA | −$14.58 | **A defect.** Re-bought 16 seconds after being stopped out of the same name |
| MPAA | +$9.66 | The market |
| OPAL | **−$148.96** | **A defect.** A quiet hour collapsed the yardstick, so target and stop sat a third of a cent apart, inside the trading spread. Losing was the only possible outcome |

Net: **+$41.10**. Of that, **+$227.25 and −$148.96 — the two largest by far —
were produced by bugs.** Remove them and the remainder is four small trades
over two days, which is not a sample.

Two exits were also priced during a 28-minute blackout in the order book
feed, so nothing about the quality of those exits can be concluded either.

---

## What would have to be true before trading again

1. **One full session, start to finish, with no code changes during it.** Not
   one fix, not one threshold. If something is wrong, note it and let the
   session finish. This has never happened.
2. **Someone confirms an alert reaching the phone.** Stop the bot deliberately
   and check the phone. Until a human sees the alert arrive, assume it does
   not.
3. **A day where the recorded data has no holes.** Capture must run a session
   with no gaps in either trades or the order book.
4. **The rule's threshold checked against more than one day.** It was set from
   a single session. It might be wrong by a lot.
5. **A decision about the strategy premise, before any more work on
   executing it.** See below.

Conditions 1 to 3 are about trusting the machine. Condition 5 is about whether
the machine is worth pointing at this idea at all.

---

## What to tell a fresh pair of eyes, in order

1. **The data capture is good and the trading is not.** Do not let the quality
   of one imply the other.
2. **Ignore the profit and loss.** It measures our bugs.
3. **The premise itself is in question, and that is the most interesting
   finding of the two days.** The strategy assumes stocks that jump before the
   open keep going afterwards. We tracked nine such stocks through a full day.
   Three continued, four faded, two went up and gave it all back. The single
   cleanest "continuation" opened at $3.86, ran to $7.10 by 10:18, and closed
   at $4.07 — it finished higher than it opened, having given back nearly the
   whole move. One day and nine stocks is not proof. But it is the first
   evidence that touches the idea rather than the plumbing, and it points the
   wrong way.
4. **Be suspicious of anything that has never fired.** Most of what went wrong
   was a check that had never once let anything through, which nobody
   questioned because not trading feels safe.
5. **The system was changed too often.** Roughly a dozen deployments in two
   days, several while positions were open. Even where each change was right,
   the sequence destroyed the ability to attribute any outcome to anything.
   Slower would have produced less code and more knowledge.

---

## Exactly what is running now

| Component | State |
|---|---|
| Market data capture | **running**, starts at boot |
| Tunnel | **running**, starts at boot |
| Dashboard | **running**, starts at boot |
| Dead-man's switch | **running**, watching capture only |
| Paper trading bot | **stopped and disabled** |
| Order-placing scheduled job | **removed** |
| Automatic report publishing | **removed** |
| Kill-switch file | **present** |

Alerts are reduced to one condition: **capture stops working.** Everything
else is written to the log for whoever chooses to look. Restoring the other
alerts is a single flag in the watchdog, noted in the code.

No result so far is evidence about the strategy. The data is real, the
plumbing works, and the question of whether the idea is any good is still
entirely open.
