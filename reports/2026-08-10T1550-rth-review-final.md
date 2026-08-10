# Regular-hours review — 2026-08-10

Nine low-float pre-market gainers tracked through the session, plus anything `rth_continuation` traded. Written to answer one question: **what, if any of this, is worth trading tomorrow.**

## Read this before the numbers

**Today's +$41.10 is not validation of anything.** Six round trips, and the two that dominate the total were both artefacts:

- **PCLA +$227.25 was a halt reopening.** The rule bought at 10:48:49, fifteen seconds after a volatility pause lifted, and the name halted again five seconds later and stayed halted for five minutes -- the stop was unreachable throughout. The ten-minute window that produced its "31.97% move" contained a five-minute hole with no prints, so the move was largely the gap. The rule was structurally ATTRACTED to reopenings; it did not select a big move, it selected a broken range measurement.
- **OPAL -$148.96 was a collapsed denominator.** An hour of near-motionless tape drove the baseline to 0.0065%, the move measured 730x it, and the derived target and stop were 0.01% apart -- $0.0003 on a $2.70 stock, inside a 0.37% spread. It stopped out six seconds after entry, which was the only available outcome.

Both are now impossible: the entry baseline is measured over a window that ENDS where the signal window begins, halts inside the feature window are flagged, and target and stop must each clear one tick or the signal is refused. Neither fix was in force when these trades were taken, so **the P&L below measures yesterday's bugs, not the strategy.**

## What the strategy actually traded

| symbol | entry | in | out | P&L | participation | fill could exist? | exit |
|---|---|---|---|---|---|---|---|
| PCLA | 10:48:49 | 14.9000 | 17.9300 | +227.25 | - | ? | target 17.5948 (18.09% = 1.5 |
| LITZ | 10:56:15 | 11.4500 | 11.3300 | -21.00 | - | ? | stop 11.3542 (0.84% = 0.93x  |
| MPAA | 11:23:25 | 12.4100 | 12.3400 | -11.27 | - | ? | stop 12.3505 (0.48% = 0.93x  |
| MPAA | 11:29:55 | 12.4706 | 12.3800 | -14.58 | - | ? | stop 12.4107 (0.48% = 0.93x  |
| MPAA | 11:30:26 | 12.4100 | 12.4700 | +9.66 | - | ? | target 12.5092 (0.80% = 1.55 |
| OPAL | 14:02:08 | 2.6900 | 2.6500 | -148.96 | 8.7% | yes | stop 2.6898 (0.01% = 0.93x r |

- **Realisable fills: 6 trade(s), +41.10** -- orders a real book could plausibly have absorbed.
- **NOT realisable: 0 trade(s), +0.00** -- above 10% of trailing 15-minute dollar volume. Paper filled them at the touch; the book would not have. These are simulator artefacts and must not be read as results.
- Combined +41.10 is shown only for reconciliation against the broker, never as performance.
- `?` on 5 trade(s): participation was not yet being recorded when those orders were placed. It is computed on every order from now on, so this column is blank only for today's earlier trades -- absence here means unmeasured, NOT within the limit.

## What to take from today

- Of the nine tracked pre-market movers, **3 continued** after 09:30, **4 faded**, **2 round-tripped**.
  - continued: STKH, NXTC, INHD
  - faded: SCKT, JWEL, DKI, AIFA
  - round-tripped: HUDI, INLF

## Price path

Pre-market is 04:00–09:30 ET; the open is the first regular-hours print on the consolidated tape, not the official opening auction price, which we do not receive.

The gap is measured against the prior session's close (`2026-08-07.csv.gz`), not against the first pre-market print.

| symbol | prev close | PM low (t) | PM high (t) | open | gap | RTH low (t) | RTH high (t) | close | open→close | verdict |
|---|---|---|---|---|---|---|---|---|---|---|
| SCKT | 0.3861 | 0.36 (05:25:04) | 3.97 (09:07:09) | 2.65 | +586.4% | 1.38 (11:36:13) | 2.79 (09:35:30) | 2.175 | -17.9% | faded |
| JWEL | 1.55 | 1.72 (04:24:58) | 6.25 (07:39:00) | 4.01 | +158.7% | 2.94 (12:07:45) | 5.17 (09:43:14) | 3.3775 | -15.8% | faded |
| STKH | 1.88 | 1.99 (04:17:05) | 6.11 (05:17:37) | 3.86 | +105.3% | 3.4701 (09:32:02) | 7.1 (10:18:03) | 4.18 | +8.3% | continued |
| DKI | 3.6701 | 5.12 (09:14:44) | 10.16 (04:17:06) | 5.19 | +41.4% | 3.81 (15:38:20) | 5.32 (09:32:06) | 4.11 | -20.8% | faded |
| HUDI | 0.7225 | 0.87 (05:01:50) | 1.14 (07:12:44) | 1.05 | +45.3% | 0.82 (09:40:54) | 1.12 (15:44:19) | 1.04 | -1.0% | round-tripped |
| INLF | 4.51 | 5.01 (04:52:35) | 5.81 (05:49:39) | 5.46 | +21.1% | 5.11 (09:58:17) | 5.6925 (11:04:25) | 5.4 | -1.1% | round-tripped |
| AIFA | 2.01 | 2.15 (07:56:22) | 2.56 (07:47:10) | 2.43 | +20.9% | 2.04 (10:29:55) | 2.45 (09:30:01) | 2.18 | -10.3% | faded |
| NXTC | 5.84 | 6.1212 (06:11:17) | 7.3033 (07:15:21) | 6.16 | +5.5% | 5.91 (09:33:58) | 7.1564 (10:12:55) | 6.73 | +9.3% | continued |
| INHD | 5.145 | 5.4 (07:45:27) | 7.4897 (04:22:59) | 6.28 | +22.1% | 6 (09:31:59) | 7.3 (13:08:06) | 6.4937 | +3.4% | continued |
| PCLA | 8.11 | 8.13 (05:34:39) | 8.79 (04:16:12) | 8.9 | +9.7% | 7.53 (12:38:16) | 19.66 (11:02:50) | 9.33 | +4.8% | continued |
| LITZ | 10.07 | 9.05 (07:27:45) | 9.5286 (08:59:47) | 9.07 | -9.9% | 8.97 (09:30:01) | 11.63 (15:48:27) | 11.63 | +28.2% | faded |
| MPAA | 13.63 | 10.29 (08:58:23) | 13.76 (07:58:55) | 10.368 | -23.9% | 10.01 (09:30:00) | 12.66 (11:59:50) | 12.335 | +19.0% | faded |
| OPAL | 2.33 | 2.07 (09:28:00) | 2.33 (06:30:00) | 2.09 | -10.3% | 2.07 (09:30:00) | 2.69 (14:01:59) | 2.6 | +24.4% | faded |

## Volume: pre-market vs regular hours

| symbol | PM shares | PM $ | RTH shares | RTH $ | RTH/PM $ |
|---|---|---|---|---|---|
| SCKT | 59,073,319 | $115,284,387 | 123,232,580 | $240,098,731 | 2.1x |
| JWEL | 52,016,457 | $231,347,949 | 15,502,410 | $57,717,873 | 0.2x |
| STKH | 21,101,483 | $98,983,465 | 36,491,263 | $181,700,246 | 1.8x |
| DKI | 7,468,204 | $54,685,385 | 869,402 | $3,777,674 | 0.1x |
| HUDI | 16,176,820 | $16,532,622 | 10,310,693 | $9,854,511 | 0.6x |
| INLF | 369,291 | $1,968,085 | 377,941 | $2,059,215 | 1.0x |
| AIFA | 283,166 | $676,103 | 490,430 | $1,094,181 | 1.6x |
| NXTC | 275,157 | $1,809,977 | 749,541 | $4,925,559 | 2.7x |
| INHD | 980,276 | $6,248,156 | 548,996 | $3,540,555 | 0.6x |
| PCLA | 30,123 | $253,940 | 3,445,265 | $47,367,070 | 186.5x |
| LITZ | 146,311 | $1,354,886 | 1,954,469 | $21,284,151 | 15.7x |
| MPAA | 20,004 | $216,787 | 229,046 | $2,777,326 | 12.8x |
| OPAL | 7,670 | $16,402 | 367,015 | $923,798 | 56.3x |

## Spread and displayed depth, pre-market vs regular hours

Depth is the **thinner side** of the round trip — we enter through the offer and must exit through the bid, so the binding number is the smaller of the two. Displayed size is a lower bound on real depth.

> **This comparison cannot be made for the tracked nine.** Their quote subscriptions began mid-morning, after the open, so no pre-market book exists for them. The names that do have pre-market book coverage were subscribed the previous evening; they are shown separately below where available. Subscribing a symbol only captures the book from that moment — it cannot be backfilled.

| symbol | window | quotes | spread p50 | spread p95 | thin p50 | thin p95 |
|---|---|---|---|---|---|---|
| SCKT | pm | — | no book captured ||||
| SCKT | rth | 143,524 | 0.522% | 1.058% | $916 | $7,068 |
| JWEL | pm | — | no book captured ||||
| JWEL | rth | 18,866 | 0.637% | 1.807% | $628 | $2,709 |
| STKH | pm | — | no book captured ||||
| STKH | rth | 91,551 | 0.590% | 1.395% | $564 | $3,760 |
| DKI | pm | — | no book captured ||||
| DKI | rth | 3,657 | 1.449% | 3.294% | $430 | $1,704 |
| HUDI | pm | — | no book captured ||||
| HUDI | rth | 8,171 | 0.957% | 2.113% | $208 | $1,728 |
| INLF | pm | — | no book captured ||||
| INLF | rth | 2,112 | 1.468% | 3.442% | $548 | $2,617 |
| AIFA | pm | — | no book captured ||||
| AIFA | rth | 2,575 | 2.179% | 6.250% | $223 | $460 |
| NXTC | pm | — | no book captured ||||
| NXTC | rth | 3,902 | 1.787% | 5.046% | $654 | $1,332 |
| INHD | pm | — | no book captured ||||
| INHD | rth | 3,578 | 2.080% | 5.625% | $655 | $1,980 |
| PCLA | pm | — | no book captured ||||
| PCLA | rth | 12,315 | 2.080% | 4.648% | $1,389 | $4,030 |
| LITZ | pm | — | no book captured ||||
| LITZ | rth | 20,431 | 0.359% | 0.626% | $21,812 | $72,135 |
| MPAA | pm | — | no book captured ||||
| MPAA | rth | 3,816 | 0.638% | 1.469% | $1,233 | $3,652 |
| OPAL | pm | — | no book captured ||||
| OPAL | rth | 7,299 | 0.787% | 1.626% | $508 | $1,506 |

### Gaps in book capture

Missing quotes are not wide quotes. These stretches have no book at all and are excluded from the percentiles above:

- **SCKT**: 10:08:42-10:13:42 (5.0 min); 10:13:52-10:19:56 (6.1 min); 10:20:22-10:25:39 (5.3 min); 10:26:31-10:32:11 (5.7 min); 10:32:45-10:39:06 (6.4 min); 10:40:45-10:46:54 (6.2 min) …
- **JWEL**: 09:53:03-10:08:25 (15.4 min); 10:59:51-11:18:57 (19.1 min); 12:32:03-12:34:35 (2.5 min)
- **STKH**: 09:53:04-10:08:24 (15.3 min); 10:59:51-11:18:56 (19.1 min); 12:32:03-12:34:34 (2.5 min)
- **DKI**: 09:53:02-10:08:27 (15.4 min); 10:30:18-10:32:26 (2.1 min); 10:59:48-11:18:57 (19.1 min); 12:05:36-12:07:46 (2.2 min); 12:32:00-12:34:49 (2.8 min); 14:26:33-14:28:57 (2.4 min) …
- **HUDI**: 09:53:03-10:08:29 (15.4 min); 10:59:51-11:19:05 (19.2 min); 12:31:54-12:34:43 (2.8 min); 15:40:41-15:42:42 (2.0 min)
- **INLF**: 09:52:34-10:08:40 (16.1 min); 10:15:45-10:17:51 (2.1 min); 10:24:00-10:26:27 (2.4 min); 10:30:07-10:32:30 (2.4 min); 10:32:32-10:34:42 (2.2 min); 10:59:52-11:20:16 (20.4 min) …
- **AIFA**: 09:53:00-10:08:30 (15.5 min); 10:15:44-10:18:24 (2.7 min); 10:18:24-10:22:02 (3.6 min); 10:22:02-10:24:06 (2.1 min); 10:26:23-10:28:35 (2.2 min); 10:30:30-10:37:19 (6.8 min) …
- **NXTC**: 09:52:54-10:08:37 (15.7 min); 10:40:01-10:42:25 (2.4 min); 10:45:46-10:50:41 (4.9 min); 10:55:03-10:57:29 (2.4 min); 10:59:37-11:18:57 (19.3 min); 11:19:45-11:21:53 (2.1 min) …
- **INHD**: 09:52:50-10:08:37 (15.8 min); 10:21:59-10:24:07 (2.1 min); 10:32:16-10:34:18 (2.0 min); 10:54:23-10:57:38 (3.2 min); 10:59:47-11:19:00 (19.2 min); 12:11:10-12:14:16 (3.1 min) …
- **PCLA**: 10:30:26-10:32:37 (2.2 min); 10:43:34-10:48:34 (5.0 min); 10:48:54-10:53:54 (5.0 min); 10:59:52-11:20:03 (20.2 min); 11:32:23-11:37:23 (5.0 min)
- **LITZ**: 10:59:44-11:19:00 (19.3 min); 12:32:03-12:34:35 (2.5 min)
- **MPAA**: 10:11:40-10:15:18 (3.6 min); 10:17:32-10:20:04 (2.5 min); 10:22:24-10:26:10 (3.8 min); 10:30:25-10:32:37 (2.2 min); 10:32:37-10:36:53 (4.3 min); 10:48:26-10:50:36 (2.2 min) …
- **OPAL**: 10:22:28-10:26:09 (3.7 min); 10:37:52-10:42:14 (4.4 min); 10:45:38-10:48:23 (2.7 min); 10:55:24-10:57:54 (2.5 min); 10:59:47-11:19:19 (19.5 min); 11:32:01-11:34:58 (3.0 min) …

## What the rule would have done

Replayed at the live loop interval with the live decision functions. **Condition 2 (target ≥ 3× spread) is not applied here**, because the book was not captured for most of the day on most of these names — so these are the tape conditions only, and the live rule would have taken fewer. Fills are priced at the last print, not at the touch, wherever no quote exists: those are not achievable fills and are marked as such.

| symbol | triggers | trades | sum | mean | priced at |
|---|---|---|---|---|---|
| SCKT | 0 | 0 | - | - | - |
| JWEL | 0 | 0 | - | - | - |
| STKH | 0 | 0 | - | - | - |
| DKI | 0 | 0 | - | - | - |
| HUDI | 0 | 0 | - | - | - |
| INLF | 0 | 0 | - | - | - |
| AIFA | 0 | 0 | - | - | - |
| NXTC | 0 | 0 | - | - | - |
| INHD | 0 | 0 | - | - | - |
| PCLA | 8 | 8 | +3.69% | +0.46% | last_print |
| LITZ | 13 | 13 | +4.13% | +0.32% | last_print |
| MPAA | 2 | 2 | +0.13% | +0.07% | last_print |
| OPAL | 4 | 4 | +1.23% | +0.31% | last_print |

**PCLA**

| entry | exit | in | out | return | why | mult | unit |
|---|---|---|---|---|---|---|---|
| 09:58:00 | 09:59:15 | 9.7200 | 10.2100 | +5.04% | target | 10.08 | 1.49% |
| 09:59:15 | 10:00:30 | 10.2100 | 9.8304 | -3.72% | stop | 13.09 | 1.49% |
| 10:00:30 | 10:01:00 | 9.8304 | 10.1000 | +2.74% | target | 10.94 | 1.49% |
| 10:01:15 | 10:02:30 | 10.5000 | 10.9000 | +3.81% | target | 10.59 | 2.24% |
| 10:02:30 | 10:03:15 | 10.9000 | 10.5711 | -3.02% | stop | 11.71 | 2.43% |
| 10:03:15 | 10:04:30 | 10.5711 | 11.1200 | +5.19% | target | 11.15 | 2.21% |
| 10:04:30 | 10:05:15 | 11.1200 | 10.8200 | -2.70% | stop | 19.14 | 1.48% |
| 10:05:15 | 10:06:15 | 10.8200 | 10.4234 | -3.67% | stop | 11.63 | 2.22% |

**LITZ**

| entry | exit | in | out | return | why | mult | unit |
|---|---|---|---|---|---|---|---|
| 09:37:00 | 09:37:15 | 10.4000 | 10.5200 | +1.15% | target | 69.26 | 0.17% |
| 09:37:15 | 09:37:30 | 10.5200 | 10.5500 | +0.29% | target | 76.95 | 0.17% |
| 09:37:30 | 09:38:15 | 10.5500 | 10.7100 | +1.52% | target | 81.79 | 0.17% |
| 09:38:15 | 09:38:30 | 10.7100 | 10.8100 | +0.93% | target | 74.43 | 0.23% |
| 09:38:30 | 09:39:00 | 10.8100 | 10.7600 | -0.46% | stop | 79.11 | 0.23% |
| 09:39:00 | 09:40:15 | 10.7600 | 10.9750 | +2.00% | target | 59.72 | 0.30% |
| 09:40:15 | 09:42:15 | 10.9750 | 11.0900 | +1.05% | target | 34.51 | 0.32% |
| 09:42:15 | 09:44:15 | 11.0900 | 10.9400 | -1.35% | stop | 34.91 | 0.34% |
| 09:44:15 | 09:44:30 | 10.9400 | 10.9000 | -0.37% | stop | 16.73 | 0.34% |
| 09:44:30 | 09:45:15 | 10.9000 | 10.8200 | -0.73% | stop | 15.27 | 0.34% |
| 09:45:15 | 09:46:15 | 10.8200 | 10.9027 | +0.76% | target | 11.03 | 0.34% |
| 09:46:15 | 09:47:00 | 10.9027 | 10.9600 | +0.53% | target | 13.52 | 0.34% |
| 09:47:00 | 09:47:15 | 10.9600 | 10.8307 | -1.18% | stop | 14.55 | 0.34% |

**MPAA**

| entry | exit | in | out | return | why | mult | unit |
|---|---|---|---|---|---|---|---|
| 09:37:15 | 09:38:15 | 11.8850 | 11.6500 | -1.98% | stop | 10.75 | 1.02% |
| 09:38:45 | 09:40:45 | 11.8500 | 12.1000 | +2.11% | target | 10.84 | 1.32% |

**OPAL**

| entry | exit | in | out | return | why | mult | unit |
|---|---|---|---|---|---|---|---|
| 10:13:00 | 10:14:00 | 2.3999 | 2.4600 | +2.50% | target | 12.02 | 0.46% |
| 10:14:00 | 10:15:00 | 2.4600 | 2.4201 | -1.62% | stop | 11.54 | 0.72% |
| 10:16:00 | 10:18:30 | 2.4500 | 2.5000 | +2.04% | target | 15.29 | 0.46% |
| 10:20:00 | 10:22:00 | 2.5125 | 2.4700 | -1.69% | stop | 12.86 | 0.47% |

## Halts

**MPAA**

- 09:36:45 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume

**PCLA**

- 10:43:34 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:48:34 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 10:48:54 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:53:54 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 11:20:03 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 11:32:23 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 11:37:23 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume

**SCKT**

- 09:35:30 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 09:35:45 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 09:56:36 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:07:10 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 10:08:42 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:13:42 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 10:20:22 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:31:59 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 10:34:06 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:39:06 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 10:41:54 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:46:54 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 10:49:15 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 10:54:15 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 10:57:49 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 11:18:50 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 11:23:50 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 11:28:23 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 11:33:23 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 13:06:51 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 13:11:51 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 13:21:14 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 13:26:14 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 13:29:59 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 13:34:59 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 13:43:07 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 13:48:07 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 13:50:50 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 14:42:57 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 14:47:57 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 14:57:45 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 15:02:45 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 15:31:10 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause

**STKH**

- 09:37:01 — `P` Volatility Trading Pause — reason `LUDP`: Volatility Trading Pause
- 09:52:48 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume
- 09:58:24 — `T` Trading Resumption — reason `C11`: Trade Halt Concluded By Other Regulatory Auth,; Quotes/Trades Resume


---

Generated 2026-08-10 15:50:52 ET from captured tape. Rule parameters: N=10.0 × the name's own expected 10-minute excursion, target 1.55×, stop 0.93×, absolute floor 1.0%, time stop 30m.
