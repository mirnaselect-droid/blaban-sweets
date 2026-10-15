# BLABAN-SWEETS Android / Google Play preparation

## Product decision: customer app + separate admin access
- The public app opens the customer storefront at https://blaban-sweets.pages.dev.
- The customer journey stays focused on browsing products, cart, checkout, and order confirmation.
- Store management stays on the separate responsive admin route: /admin.html. Do not add management controls or customer-visible order data to the storefront UI.
- The admin route requires Supabase sign-in and checks membership in public.admin_users before showing the dashboard. Database row-level security (RLS) must remain the authority for reading/updating orders and managing products/settings; the UI check alone is not a security boundary.
- Do not add a public “admin shortcut” to the customer app. The owner can open and bookmark the separate admin URL on her phone; a separately installable admin PWA can be considered later if needed, without exposing management tools in the customer app.

## Current state
- The storefront has a web app manifest and service-worker foundation.
- The storefront is hosted at https://blaban-sweets.pages.dev.
- This branch adds standard 192px and 512px PNG app icons for browser installation and Android packaging.
- The admin interface is responsive and uses a separate login screen, but the full sign-in, permissions, and order workflow still need end-to-end testing on a preview.
- The PWA asset-validation GitHub Actions workflow passed for the current branch commit.
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
- The database policies currently queried restrict order reads/updates to admins and restrict product/settings mutations to admins. Re-check policies and test with both an anonymous session and a non-admin authenticated session before release.
- The current production domain is a Cloudflare Pages domain. Confirm the intended permanent app origin before creating Digital Asset Links, because changing the origin later affects verification.
