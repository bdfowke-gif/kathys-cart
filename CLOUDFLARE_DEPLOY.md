# Cloudflare deployment for Kathy's Cart

The app is ready to run as a Cloudflare Worker. It uses one optional KV binding:

`FLYER_VAULT`

That binding stores the twice-weekly North Vancouver flyer snapshot for the six-day search window. Do not put the Vynn API key in this file or in the browser; add it as a Worker secret.

## Dashboard path (recommended)

1. In Cloudflare, choose **Workers & Pages → Create application → Worker**.
2. Name it `kathys-cart-north-vancouver`.
3. Deploy the Worker source from `worker/index.js`.
4. Create a KV namespace named `kathys-cart-flyer-vault`.
5. Open the Worker → **Settings → Bindings → Add → KV Namespace**.
6. Use variable name `FLYER_VAULT` and select `kathys-cart-flyer-vault`.
7. Add the Vynn key as a Worker secret named `VYNN_API_KEY`.
8. Deploy again.

The generated address will be similar to:

`https://kathys-cart-north-vancouver.<your-account-subdomain>.workers.dev`

## Verification

After deployment, check:

- `/health` — Worker is responding.
- `/api/coverage` — North Vancouver store scope is present.
- `/api/flyers/status` — storage should say `Cloudflare KV`, not `Worker memory until KV is connected`.

The included `wrangler.jsonc` documents the binding. Replace its placeholder namespace ID only if deploying with Wrangler; the dashboard binding is safer for a first deployment.
