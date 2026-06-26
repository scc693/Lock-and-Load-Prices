# Lock-and-Load-Prices

A single-file, mobile-first **POS calculator** for *Boris' Questionably Legal Elixirs* — a
prop for a tabletop RPG puzzle. No real transactions, just the over-engineered math the DM
needs while players try to solve the puzzle for some health potions.

## Open it

Just open **`index.html`** in any browser. No build step, no server, no dependencies.
On a phone, "Add to Home Screen" to run it full-screen like an app (PWA manifest included).

## What it does

- **Full menu** parsed from Boris' price sheet (Battle Brews, Snacks & Potions, Mystic Reagents) — tap `+`/`–` to ring up items.
- **Regular vs Bulk** pricing toggle, with **per-item minimum order quantities** in bulk mode (e.g. Life Shard Kit bulk is $175/unit with a `min 10` — the first tap jumps to 10, and you can't sit below the minimum). Set via an optional 6th value on a catalog row.
- **20% markup** applied to every item (base price shown struck-through next to the marked-up price).
- **$15 flat-rate shipping** added once per order.
- **Processing fee** selector: **$5**, **$10**, or **CashApp est.** — the last estimates the on-chain (gas) cost CashApp passes through on a BTC withdrawal, from live mempool fees.
- **Bitcoin conversion** at CashApp-style rates: pulls live BTC spot (Coinbase, CoinGecko fallback) and applies an adjustable **spread %** (default 1.75%) to approximate CashApp's buy rate.
- **Breakdown** panel showing base → markup → shipping → fee, the spot rate, the effective rate, and a refresh button.

### Order math

```
items            = Σ (base_price × qty)        # base = Regular or Bulk
+ 20% markup     = items × 0.20
+ shipping       = $15 flat (per order)
+ processing fee = $5 | $10 | CashApp on-chain estimate
─────────────────────────────────────────────
total (USD)
≈ Bitcoin        = total ÷ (spot × (1 + spread%))
```

## Notes

- Live rates fetch directly from the device when online; offline it falls back to manual rate
  entry and the fixed $5/$10 fee options so the calculator still works at the table.
- Everything is fictional flavor for a game — the "products" are RPG items (Heart Containers,
  Phoenix Pops, etc.), not anything real.
