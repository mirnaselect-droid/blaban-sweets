# BLABAN-SWEETS Android / Google Play preparation

## Current state
- The storefront has a web app manifest and service-worker foundation.
- The storefront is hosted at https://blaban-sweets.pages.dev.
- This branch adds standard 192px and 512px PNG app icons for browser installation and Android packaging.
- This is not yet a signed Android App Bundle (AAB), and it is not ready to publish to Google Play.

## Safe release sequence
1. Test the storefront and checkout against a preview deployment, including real order creation, payment-detail display, and the admin path.
2. Confirm the permanent Android application ID before generating a signing key. It is a long-lived identity and should not be guessed at release time.
3. Build a Trusted Web Activity (TWA) wrapper for the production HTTPS origin if the storefront passes quality checks. Avoid a bare WebView wrapper that adds no meaningful value.
4. Generate and securely store the Android signing key; never commit the private key or passwords to this repository.
5. Add `/.well-known/assetlinks.json` using the exact application ID and SHA-256 fingerprint of the signing certificate used for the release.
6. Test the signed AAB on physical Android devices and verify links, back navigation, checkout, connectivity failures, and that admin routes are not cached as public app content.
7. Complete Play Console listing, privacy/data-safety disclosures, screenshots, content rating, and any required testing track before production rollout.

## Important safeguards
- Do not merge this branch into `main` until preview and order-flow tests pass.
- Do not put Supabase service-role credentials, signing keys, or passwords in frontend code or Git.
- Do not claim that offline checkout works: the service worker currently provides only a conservative storefront shell fallback.
- The current production domain is a Cloudflare Pages domain. Confirm the intended permanent app origin before creating Digital Asset Links, because changing the origin later affects verification.
