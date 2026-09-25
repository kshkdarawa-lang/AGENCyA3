# AGENC A3 — Cloudflare setup

This package is prepared to deploy the existing AGENC A3 Node/Express backend as a Cloudflare Container. Cloudflare Containers can run existing Docker applications and can forward WebSocket requests to the container.

## What you need

1. A Cloudflare account on a Workers Paid plan (Containers are available on Workers Paid).
2. A persistent PostgreSQL database reachable from the internet (Neon, Supabase, AWS, your VPS PostgreSQL, etc.).
3. Docker running on the machine from which you deploy.
4. Node.js/npm.

## 1. Install dependencies

```bash
npm install
```

## 2. Login to Cloudflare

```bash
npx wrangler login
```

A browser window will open. Authorize the Cloudflare account you want to use.

## 3. Add the database secret

Use the complete PostgreSQL connection string, for example:

```bash
npx wrangler secret put DATABASE_URL
```

When prompted, paste your PostgreSQL connection string.

Then create the JWT signing secret:

```bash
npx wrangler secret put JWT_SECRET
```

Use a long random value. Do not put either value into `wrangler.jsonc` or GitHub.

## 4. Deploy

Make sure Docker is running, then:

```bash
npx wrangler deploy
```

The first container deployment can take several minutes.

## 5. Test the API

After deployment, Wrangler prints the Worker URL. Test:

```text
https://YOUR-WORKER.workers.dev/api/health
```

It should return JSON similar to:

```json
{"ok":true,"service":"AGENC A3"}
```

## 6. Connect the GitHub Pages site

Open the AGENC A3 site:

`https://kshkdarawa-lang.github.io/AGENCyA3/`

In **إعدادات ربط السيرفر**, enter the Worker URL, for example:

`https://agenc-a3-api.YOUR-SUBDOMAIN.workers.dev`

Save it, then create an account and log in.

## 7. WebSocket / rooms

The existing backend uses WebSocket on the same origin. The frontend automatically changes `https://` to `wss://` when connecting to the configured API URL. Cloudflare Containers support forwarding WebSocket requests to the container.

## Important music note

The current AGENC A3 backend stores uploaded music under `public/uploads/music` inside the container. Container disk is not a suitable permanent music library because container instances can be replaced. Before production, move `/api/music/upload` to Cloudflare R2 and store only the R2 object key/URL in the database. The room WebSocket music controls can remain as they are.

## Optional custom domain

After the Worker works on `workers.dev`, add your API custom domain/route in Cloudflare, then use that HTTPS URL in the GitHub Pages site's server setting.

## Useful commands

```bash
npx wrangler deploy
npx wrangler tail
npx wrangler secret put DATABASE_URL
npx wrangler secret put JWT_SECRET
```
