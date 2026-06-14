# Itemize your Costco receipts into YNAB

Turn a photo of a Costco receipt into one fully itemized, categorized split transaction in [YNAB](https://www.ynab.com/) — in a few seconds, right in your browser.

## The problem

A single Costco run is one of the worst things to enter in YNAB: one big transaction you have to split across a dozen categories, with sales tax that applies to some items and not others. By hand, it's a slog.

## What it does

Take a photo of the receipt and the tool builds the whole entry for you:

- **One YNAB transaction, split line by line** — one split per item.
- Each item **categorized** into your real YNAB categories.
- **Sales tax distributed** exactly as the receipt charged it. (Costco taxes groceries and general merchandise at different rates; the tool reads the receipt's own printed tax totals, so it's correct for whichever store you shopped at — no hardcoded rates.)
- The **real product name** in each item's memo, on demand. Costco's cryptic abbreviations like `NTG BEACH` become `Neutrogena Beach Defense SPF 60+ (#1696849)` with one tap.
- The total **reconciled to the penny** before anything posts.

You review everything, fix anything, then post.

## Use it

**→ [Live version](https://tweder.github.io/costco-ynab-itemizer/)**

You'll need two things:

1. **A YNAB Personal Access Token** — in YNAB: Account Settings → Developer Settings → New Token. Free.
2. **An Anthropic API key** — from [console.anthropic.com](https://console.anthropic.com) → API Keys. This is *paid API usage* (see [Costs](#costs)) and is separate from a Claude.ai subscription.

Then: paste both keys → pick your budget and account → take or upload a receipt photo → **Parse** → review the lines → **Post**.

## Privacy

This is the part that matters, since you're handling a token with full access to your budget:

- It's a **single static HTML file** — no server, no backend, no database, no accounts, no analytics.
- Your keys and the receipt are sent **only** to `api.anthropic.com` (to read the receipt) and `api.ynab.com` (to post the transaction). Nowhere else.
- Keys live in your browser for the session, and are saved to your browser's local storage **only if you tick "Remember on this device."** Untick it, or use "Forget saved keys," and nothing persists.
- The entire tool is one readable file in this repo. What you see is what runs — confirm the two points above with your own eyes before pasting anything.
- Your YNAB token has full read/write access to your budget. Treat it like a password. You can revoke it anytime from YNAB's Developer Settings.

## Costs

Reading a receipt is one Anthropic vision request — a few cents, depending on the model you choose (Haiku cheapest, Opus most accurate). Each optional product-name lookup is one web search, roughly a cent. Set a spend limit in your Anthropic console to stay in control. You pay Anthropic directly for your own usage; no one else is in the loop.

## Run it yourself

Prefer not to use the hosted version? Download `index.html` and run it locally. One catch: **YNAB rejects posts from a `file://` page**, so you can't just double-click the file — you have to serve it from a real web address:

```
cd folder-with-the-file
python3 -m http.server
```

Then open the `http://localhost:8000/` address it prints. (Reading and categorizing work from a plain file; only the final post to YNAB needs the local server.)

## Notes & limitations

- Vision occasionally misreads a line (odd by-weight deli items are the usual culprit). The **Pre-tax amount is editable** — correct it and the tax and total re-balance automatically. The tool won't let you post until the split reconciles to the receipt total.
- Categories are inferred per item; review them. Low-confidence lines are flagged.
- Line items auto-sort by category by default; uncheck the toggle to keep cashier scan order.
- Built and tested for US Costco warehouse receipts.

## License

MIT — see [LICENSE](LICENSE).

## Disclaimer

Not affiliated with Costco, YNAB / You Need A Budget LLC, or Anthropic. Provided as-is, without warranty. You are responsible for reviewing every transaction before posting it to your budget.
