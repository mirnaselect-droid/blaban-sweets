# BLABAN-SWEETS Android / Google Play preparation

## Confirmed app identity
- Public display name: `BLABAN-SWEETS | حلو ب لبن` (Arabic + English).
- Proposed Android application ID selected by the owner: `com.blabansweets.app`.
- Treat this application ID as permanent for release planning; verify it is available/unused in the intended Google Play account before publishing. Do not change it casually after release.
- Production customer storefront: https://blaban-sweets.pages.dev.
- Customer app launch must stay on the public storefront. Keep admin access separate at `/admin.html`; do not add admin shortcuts or management controls to the customer app.

## Current state
- The PWA manifest and service-worker foundation exist on branch `pwa-safe-main`.
- Android icon assets include 192px and 512px PNGs plus an SVG fallback.
- GitHub Actions PWA asset validation passed for the current branch commit before this identity update.
- The owner reports testing checkout/orders and the admin interface on the preview successfully.
- This is not yet a signed Android App Bundle (AAB), and it is not ready to publish to Google Play.

## Monetization option (not implemented yet)
- Google AdMob supports app-open ads that can appear during an app's loading/splash screen, subject to eligibility, ad availability, frequency controls, privacy disclosures, and Google Play / AdMob policies.
- A PWA alone does not automatically earn money on every launch. For this Android TWA, native AdMob integration and testing would be a separate development step; no ad SDK, ad unit IDs, or live ads are currently configured.
- Do not promise income per launch: an ad may not load or be served, and revenue varies. Never click your own live ads or encourage artificial/repeated opens; invalid traffic can lead to withheld earnings or account restrictions.
- Because BLABAN-SWEETS is a shopping app, ads should not block checkout or mislead customers. Before implementation, get the owner's explicit choice on whether to add app-open ads and how often they may appear. Keep monetization accounts and payout ownership clearly documented if the store/app is sold.

## Safe release sequence
1. Keep changes isolated on `pwa-safe-main` and verify the preview after each update; do not merge to `main` without explicit owner approval.
2. Confirm the application ID is available/unused in the intended Play Console account before creating the release package.
3. Build a Trusted Web Activity (TWA) wrapper for the production HTTPS origin if the storefront passes quality checks. Avoid a bare WebView wrapper that adds no meaningful value.
4. Generate and securely store the Android signing key on a trusted device/build environment; never commit the private key or passwords to this repository.
5. Add `/.well-known/assetlinks.json` only after the final application ID and signing certificate SHA-256 fingerprint are known. Do not guess the fingerprint.
6. Test the signed AAB on physical Android devices and verify links, back navigation, checkout, connectivity failures, and that admin routes are not cached as public app content.
7. Complete Play Console listing, privacy/data-safety disclosures, screenshots, content rating, and any required testing track before production rollout.

## Important safeguards
- No merge to `main`, production deployment, or Supabase schema/permission change is authorized by this preparation.
- Do not put Supabase service-role credentials, signing keys, or passwords in frontend code or Git.
- Do not claim that offline checkout works: the service worker currently provides only a conservative storefront shell fallback.
- The database policies and order-creation RPC must be re-checked and tested with anonymous and non-admin sessions before release.
- The current production domain is a Cloudflare Pages domain. Keep it as the intended TWA origin unless the owner explicitly chooses another permanent domain; changing the origin later affects Digital Asset Links verification.
