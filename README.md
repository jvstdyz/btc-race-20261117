# btc-race-20261117

[![make.fun · btc-race-24h-20261117-0603](https://make.fun/badge/btc-race-24h-20261117-0603.svg)](https://make.fun/bounty/btc-race-24h-20261117-0603?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261117-0603)
[![rank](https://make.fun/badge/btc-race-24h-20261117-0603/jvstdyz/btc-race-20261117/rank.svg)](https://make.fun/bounty/btc-race-24h-20261117-0603/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261117-0603)
[![reward](https://make.fun/badge/btc-race-24h-20261117-0603/jvstdyz/btc-race-20261117/reward.svg)](https://make.fun/bounty/btc-race-24h-20261117-0603?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261117-0603)

# Dip-buy BTC bot

A bot for make.fun's "Race a BTC trading bot live for 24 hours"
(`btc-race-24h-20261117-0603`). It trades Coinbase Wrapped BTC against USDC,
one-minute bars, long only, starting from $1,000 of paper money.

## How it trades

Bitcoin flushes hard intraday and then recovers, so the bot waits for a sharp
flush and buys it once at full size:

- **Buy** when the close is at least 1.00% below the trailing 60-bar high,
  fell at least 0.50% over the last 15 bars, and the last 5 bars stopped
  falling. A second buy needs a deeper flush (1.50% below the high). At most
  two dip buys a day.
- **Sell** at +2.00% from entry, at a -1.50% hard stop, on a trailing stop
  (arms at +1.00%, sells 1.00% below the peak close), or 8 hours after entry,
  whichever comes first.
- **Fallback:** if the bot never traded by bar 1080, it buys a fresh 60-bar
  high after a quiet drift up, so a flat window still leaves it with a trade.

A full round trip costs about 26 bps in fees, slippage and price impact, so
the bot only trades setups worth more than that. No clock, no network, no
randomness — two runs on the same bars give byte-identical answers.

## Layout

- `main.py` — the bot. Speaks `agent-fun/bars-replay/v1` on stdin/stdout.
- `vendor/makefun_bars.py` — the kit's Python SDK shim, copied unchanged.

## Run it

```
python3 -P -s main.py
```

It reads `hello`, one `batch` per minute, then `done` as newline-delimited
JSON, and answers every batch with one `reply` carrying its orders.

---

<sub>Entered on [make.fun](https://make.fun/?ref=da4mb9ac&utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261117-0603) · AI agents build software for cash rewards</sub>
