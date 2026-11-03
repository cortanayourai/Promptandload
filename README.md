# Promptandload

Source of the promptandload.com site (Hugo content + Caddy deploy compose).

Deploy truth is docker-compose.yml in this repo: caddy:2-alpine, loopback-only bind,
250m memory cap, read-only rootfs. Ingress is the Cloudflare tunnel, never a public port.
