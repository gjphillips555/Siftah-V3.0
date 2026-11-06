# Siftah V3.0 — integration roadmap

## Safety / deployment boundary
- This repository and its Vercel project are separate from the existing LootersComputas production project.
- Do not attach, remove, transfer, or overwrite `looterscomputas.online` during development.
- Use Vercel preview deployments and a separate preview hostname until explicit production approval.
- Do not copy production secrets into source control. Configure new project environment variables separately and only after confirming each provider's callback URLs and scopes.
- Existing GitHub production repository and its `main` branch are not modified by this project.

## Existing LootersComputas features located for reuse
The original repository contains the following source implementations; these should be ported selectively after dependency and security review, rather than pasted wholesale into the static preview:

### Siftah client
- `public/siftah/index.html`
- `public/siftah/app.js`
- `public/siftah/auth.js`
- `public/siftah/coded_dreams.js`
- `public/siftah/siftah.css`
- `public/siftah/credits.html`

The current chat client posts to `/api/siftah/chat`. It expects the server route and account/session behavior to be migrated with it.

### AI API
- `src/routes/api/siftah/chat.ts`
- `src/routes/api/siftah-message.ts`

These are server routes from the original TanStack Start app. They cannot be served by the current static Vite-only preview without migrating the server framework and its dependencies.

### Authentication
- `src/lib/auth/client.ts`
- `src/lib/auth/provider.tsx`
- `src/routes/api/auth/$.ts`

The original uses Better Auth. OAuth credentials and callback URLs must be configured on the new Vercel project; never expose secrets in browser code.

### PayPal / credits
- `src/routes/api/paypal/create-order.ts`
- `src/routes/api/paypal/capture-order.ts`
- `src/routes/api/paypal/webhook.ts`
- `src/routes/api/paypal/validate-discount.ts`

Port together with the server-side billing and database dependencies. Do not enable checkout until webhook signature validation, idempotency, product pricing and account crediting are tested.

### Marketplace / community
- Existing route implementations are in `src/routes/`, including account, checkout, listing, hidden forum, arcade, and wallet-related pages.
- Database access is in `src/lib/db.ts` with migrations in `migrations/`. Review schema ownership and isolate the new project's database before migration.

### Arcade
- Existing game catalogue: `public/arcade-games.json`
- Existing arcade page: `public/arcade.html`

The catalogue is copied into this repository as a starting asset. Game launch URLs and provider terms still need verification.

## Current V3.0 work
- Responsive dark/red dashboard scaffold based on the supplied reference.
- Four original vector mascot designs in a consistent 2D cartoon direction: Siftah, Dexter, Da Vinci, Super Saiyan.
- Explicit development-preview indicator; no claim that chat, authentication or payment are live.
- Main LootersComputas navigation opens in a separate tab.
- Fixed a JavaScript error in the first scaffold that could stop the rest of the page scripts from running.

## Next engineering milestones
1. Migrate the app to a server-capable Vercel framework (recommended: TanStack Start matching the original) and pin dependencies.
2. Port Better Auth and session enforcement with fresh OAuth callback configuration.
3. Port Siftah chat endpoint and model-provider routing; add Dexter and Da Vinci service adapters.
4. Port database schema into a separate database; migrate wallet/credits with user-scoped authorization.
5. Port PayPal checkout and webhooks; test with sandbox transactions.
6. Port arcade catalogue, forum and marketplace routes.
7. Add automated build, typecheck, auth invariants and smoke tests.
8. Verify the preview deployment on its own hostname.
9. Only after explicit approval, plan the production domain cutover. Until then, the custom domain remains untouched.
