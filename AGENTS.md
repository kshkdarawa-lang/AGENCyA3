# AGENC A3 Cloudflare deployment notes

- This project uses Cloudflare Workers + Containers to run the existing Node/Express backend.
- The container listens on port 3000 and supports the existing WebSocket endpoint.
- Never commit DATABASE_URL or JWT_SECRET. They are Worker secrets.
- Use Wrangler for local development, deployment, migrations, and logs.
- Cloudflare Containers require a Workers Paid plan.
- The current backend uses PostgreSQL and local container disk for uploaded music. PostgreSQL should be an external persistent database. Uploaded music is not durable across container replacement yet; migrate music uploads to R2 before production.
- Keep the existing GitHub Pages frontend and set its API base to the deployed Worker URL.
