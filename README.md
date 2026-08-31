# iOS Safari Download Extension — Variant Test

Minimal standalone test page to isolate the iOS Safari download-extension bug.

## Setup

```bash
npm install
npm start        # serves on http://localhost:8080
```

To reach it from a real iOS device, expose it with a tunnel:

```bash
npm run tunnel   # prints a public https:// URL (localtunnel)
```

## Usage

1. Open `ios-download-variant-test.html` on a real iOS Safari device.
2. **Replace `TEST_IMAGE_URL`** in the HTML with a real generated-image CloudFront URL from the app (the dummy image won't reproduce the real `Content-Type`).
3. Open Web Inspector (Safari on Mac → device) → Network tab, and confirm the actual `Content-Type` header for the image request.
4. Click all four variants and record the saved filename for each:

| Variant | What it does |
|---------|--------------|
| **A** | Current code, unmodified — server's real Content-Type |
| **B** | Blob re-tagged to `image/png` before `createObjectURL` |
| **C** | Blob re-tagged to `application/octet-stream` before `createObjectURL` |
| **D** | `a.type` attribute set explicitly in addition to `download` |

5. Note the iOS/WebKit version on the test device — the bug's behavior has changed across versions.

## Acceptance

Only implement the variant that empirically produces the correct filename on the real device. A unit test can only pin down *which variant the code implements* — it cannot prove what Safari does with it.# ios-test
