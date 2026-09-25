# Cloudflare-ready AGENC A3

The original Node/Express backend is deployed as a Cloudflare Container. PostgreSQL remains external and persistent; set `DATABASE_URL` as a Worker secret. WebSocket room traffic is forwarded to the container.

See `CLOUDFLARE-SETUP.md` for the exact deployment steps.
