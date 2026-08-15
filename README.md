# Price Ticker

A live cryptocurrency price ticker banner in a single self-contained HTML file. It shows BTC, ETH, XCH, XMR, SOL, and AAVE prices with 24h change, scrolling in an infinite loop and refreshing every 30 seconds.

## Run it

No build step, no dependencies.

- Open `index.html` directly in a browser, or
- Serve it locally so `fetch` works reliably:

  ```sh
  # any static server works, e.g.:
  npx serve
  # or
  python -m http.server
  ```

> Opening the file over `file://` also works in most browsers, but if you hit CORS or mixed-content issues, use a local server.

## What you get

- **Ticker** — six coins (symbol, price, 24h change) scroll horizontally in a seamless loop. Two identical groups slide via `translateX(-50%)`; a mask gradient fades the edges so tiles don't pop in/out. Hovering the ticker pauses the scroll.
- **Status bar** — green live dot (red on connection problems), "Live · updated HH:MM:SS" timestamp, and a "next refresh in Ns" countdown.
- **Progress bar** — a thin bar that shrinks over each 30s refresh cycle.
- **Data source** — [CoinGecko](https://www.coingecko.com) `simple/price` endpoint, fetched with `cache: "no-store"`. At one request per 30s it stays well under the free-tier rate limit. If a request fails, the ticker keeps showing the last good data and retries on the next cycle.

## Theming

Colors are defined as CSS custom properties on `.viz-root` (see the palette block at the top of `index.html`), with:

- Automatic dark mode via `prefers-color-scheme: dark`
- Manual override by setting `data-theme="dark"` (or `"light"`) on `<html>`
- Semantic roles: `--delta-up` / `--delta-down` for price direction, `--text-secondary` for symbols, `--muted` for missing data

## Accessibility

- `prefers-reduced-motion`: disables the scroll and the progress animation, makes the ticker manually scrollable, and hides the duplicate group
- The banner is a labeled region; each tile has an `aria-label` ("BTC $67,234.56, 24h up 1.23 percent"); decorative elements (progress bar, duplicate group, countdown) are `aria-hidden`
- Tabular numerals (`font-variant-numeric: tabular-nums`) keep digits from jittering as prices update

## Customizing

All config lives in the `<script>` block of `index.html`:

| What | Where |
|---|---|
| Coins | `COINS` array (`id` is the CoinGecko id, `symbol` the display ticker) |
| Refresh interval | `REFRESH_SECONDS` (used by the fetch loop, countdown, and progress bar) |
| Scroll speed | `animation: scroll 30s …` on `.ticker-track` |
| Banner size | `.banner` (1920 × 200 px) |
| Font sizes | `.tile .symbol / .price / .change` (120px) |

When adding a coin, make sure its id is also in the `ids=` list of `API_URL`.
