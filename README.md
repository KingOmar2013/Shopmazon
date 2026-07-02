# Shopmazon

An Amazon.com **UI/UX replica** built for learning — a single-file web app that reproduces Amazon's storefront look and full shopping flow, informed by a UX audit of the real site. No backend, no build step, no framework.

> Study project only. Not affiliated with Amazon. Catalog data and product photos come from the free [DummyJSON](https://dummyjson.com) demo API. No real products, payments, or personal data.

## Features

| Area | What it does |
|---|---|
| Storefront | Amazon-style header, dual nav bar, department links, hero, product grid |
| Search & filter | Keyword search, category dropdown, sort (price / rating / featured) |
| Product page | Multi-image gallery, price + discount, ratings, description, sticky buy box |
| Cart | Add / Buy now, quantity stepper, remove, live subtotal, localStorage persistence |
| Checkout | Shipping + payment form (UI only), order summary, validation |
| Confirmation | Generated order number, delivery estimate |
| Catalog | Live fetch of 194 real products with white-background photos |

## UX audit fixes applied

Choices that improve on the real Amazon rather than copy its dark patterns:

| Real Amazon issue | Fix in Shopmazon |
|---|---|
| Sponsored vs. organic look identical | Sponsored items get a border + explicit tag |
| Overloaded product page | Single decision column, isolated sticky buy box |
| Guilt-framed shipping declines | Honest free-shipping threshold, no coercion |
| Buried critical info | Stock, delivery, and price surfaced above the fold |

## Tech

- Vanilla HTML + CSS + JavaScript, one file (`shopmazon.html`)
- No dependencies, no bundler
- State in memory; cart persisted via `localStorage`
- Data: `https://dummyjson.com/products` (fetched at runtime)
- Prices: API values are USD, converted to EGP at a fixed demo rate (`FX` constant, default `48`)

## Run

**Recommended — local server** (avoids the browser blocking `fetch()` on `file://`):

```bash
# from the folder containing shopmazon.html
python -m http.server 8080
# then open http://localhost:8080/shopmazon.html
```

Or with Node:

```bash
npx serve .
```

Then open the printed URL.

## Configuration

| Constant | Location | Purpose |
|---|---|---|
| `FX` | top of `<script>` | USD → EGP conversion rate |
| fetch `limit` | `loadProducts()` | number of products to load (max 194) |

## Troubleshooting

**"Couldn't load products"** — the browser blocked the catalog `fetch()`. This happens when the file is opened directly (`file://…`). Serve it over http instead (see **Run** above). On a real device / APK the app is served over http, so this does not occur.

**Broken image tiles** — a product photo failed to load; the app falls back to a 📦 placeholder automatically. Requires internet, since photos are hosted on the DummyJSON CDN.

## Android (Capacitor)

Same path as a standard Capacitor web build:

```bash
npm i -g @capacitor/cli
npx cap init shopmazon com.you.shopmazon --web-dir=www
# place shopmazon.html in www/ as index.html
npx cap add android
npx cap sync
npx cap open android   # build APK in Android Studio
```

The default Capacitor manifest already includes `INTERNET` permission, which the catalog and photos require.

## Project structure

```
shopmazon.html      # the entire app
README.md           # this file
```

## License / attribution

For personal learning use. Product data and images © their respective owners, served via DummyJSON for demo purposes. Amazon trademarks and design are the property of Amazon; this replica exists only as a UI/UX study.
