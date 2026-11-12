# Lighthouse reports

The baseline report in this folder is real, not fabricated.

| File | What it is |
|---|---|
| `mobile-baseline.report.html` | Lighthouse mobile report (open in a browser) |
| `mobile-baseline.report.json` | Same run, machine-readable data |

Captured on **12 Nov 2026** against `https://10-x-shop.netlify.app/` with Chrome
157.0.8103.39, Lighthouse 12, default **mobile** preset:

| Category | Score |
|---|---|
| Performance | 86 |
| Accessibility | 100 |
| Best Practices | 100 |
| SEO | 100 |

Lab metrics: FCP 1.4 s, LCP 3.0 s, TBT 70 ms, SI 2.9 s, **CLS 0.171**. The CORE target
(Performance ≥ 85) is met. The FULL target CLS < 0.1 is not met; no optimization deploy was made,
so there is no honest "after" report to compare.

## How to reproduce

```powershell
npx -y lighthouse@12 "https://10-x-shop.netlify.app/" `
  --chrome-path="C:\Program Files\Google\Chrome\Application\chrome.exe" `
  --output=html --output=json --output-path="docs/lighthouse/mobile-after" `
  --only-categories=performance,accessibility,best-practices,seo
```

## Notes

- Re-run after the Netlify redeploy so the report matches the final build.
- Do not compare scores from different presets (desktop vs mobile) or different network
  conditions.
- DummyJSON latency affects the score; repeat the audit once to check stability.
- If you make a real optimization (for example lazy-load below-the-fold images), save the second
  report as `mobile-after.report.html` and record both runs in `REPORT.md`.
