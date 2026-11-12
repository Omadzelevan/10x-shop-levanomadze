# COMPATIBILITY — 10X Shop

Tested on **12 Nov 2026** on Windows 10 Enterprise LTSC 21H2 (build 19044) with:

- **Chrome 157.0.8103.39** (headless, system Chrome)
- **Firefox 159.0** (headless, Playwright Firefox build)

Two builds were checked:

1. **Live URL** — `https://10-x-shop.netlify.app/` (currently the earlier deploy).
2. **Local production preview** — `http://localhost:4173` built from the current source, which
   includes the SPA fallback and the corrected checkout email message.

Legend: `✓` works · `issue: … → fix: …` describes a real problem and its fix.

## Desktop

| Page / flow | Chrome (desktop) | Firefox (desktop) |
|---|---|---|
| `/` Catalog: load, 12 cards, add to cart | ✓ 12 cards, badge updates, no console errors | ✓ 12 cards, badge updates |
| `/products/:id`: gallery, stepper, add to cart | ✓ title, gallery, quantity, add to cart | ✓ title, add to cart, badge updates |
| `/cart`: quantity, remove, clear, totals | ✓ totals change and empty state works | ✓ totals change and empty state works |
| `/login`: validation errors, successful login | ✓ errors shown; `emilys` / `emilyspass` logs in | ✓ errors shown; login succeeds |
| `/checkout`: guard, validation, place order, confirmation | ✓ guest → login → checkout → confirmation | ✓ same flow; confirmation shown |
| `/products/abc`, `/products/999999`, `/whatever` → 404 | ✓ 404 page (client-side navigation) | ✓ 404 page (client-side navigation) |
| Direct refresh of `/products/5` on the deployed URL | issue: live URL returns **HTTP 404** for `/products/5`, `/cart`, `/login` → fix: `public/_redirects` with `/* /index.html 200` added; local preview now returns 200 and the page renders. **Netlify must be redeployed** for the live URL to be fixed. | issue: same HTTP-level 404 on the deployed URL (server configuration, not browser-specific) → same fix. Client-side `/products/5` renders correctly in Firefox. |
| Cart survives refresh; login does not | ✓ cart kept (`shop_cart`); auth is in-memory only, so refresh logs out | ✓ cart kept; refresh logs out |

## Mobile emulation (375 × 812)

| Page / flow | Chrome (mobile emulation) | Firefox (mobile emulation) |
|---|---|---|
| No horizontal scroll on any page | ✓ catalog, cart, login, checkout | ✓ catalog, cart, login, checkout |
| Burger menu opens/closes, `aria-expanded` changes | ✓ `false → true`, navigation visible | ✓ `false → true` |
| Tap targets ≥ 44px | ✓ no visible target under 44px high | ✓ no visible target under 44px high |
| Product grid is 1 column at 375px, 2 from 481px, 4 above 1024px | ✓ 1 / 2 / 4 columns measured | ✓ 1 column at 375px |
| Checkout form and summary stack vertically | ✓ summary below form | ✓ summary below form |

## Notes

- Automated runs: Chrome **25/25** functional checks and **15/15** responsive/screenshot checks;
  Firefox **23/23** functional checks. All passed against the local production preview.
- Against the **live URL**, the earlier deploy passed every flow except the checkout email text:
  the old build still shows `Email is required`, while the current source shows the required
  `Please enter a valid email address`. This is a stale-deploy difference, not a browser bug.
- The only live URL defect found is the missing SPA fallback (all direct routes return 404). The
  fix is in `public/_redirects`; it reaches production only after a new Netlify deploy.
- Screenshots: `docs/compat/chrome-catalog.png`, `chrome-cart.png`,
  `chrome-mobile-catalog.png`, `chrome-mobile-cart.png`, `chrome-mobile-login.png`,
  `chrome-mobile-checkout.png`, `firefox-mobile-catalog.png`, `firefox-mobile-login.png`,
  `firefox-mobile-checkout.png`.
- A physical-device check (real phone, real touch) was **not** performed; mobile results above are
  browser viewport emulation.
