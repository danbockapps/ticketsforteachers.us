# WordPress trial — tk.danbock.net

A Dockerized WordPress (theme evaluation site for Matt) running alongside the
static site on the VPS. The static `site/` and the main nginx server block for
`ticketsforteachers.us` are **not** touched by any of this.

- URL: https://tk.danbock.net
- Stack: `wordpress:latest` + `mariadb:11` via `docker compose`
- Port: WordPress publishes to `127.0.0.1:8090` on the VPS; host nginx proxies
  HTTPS traffic for `tk.danbock.net` to that port.

## First-time setup on the VPS

1. **DNS** — add an A record: `tk.danbock.net → 178.156.142.199`
2. **Files** — from the repo root:
   ```sh
   cd wordpress
   cp .env.example .env    # then edit passwords
   docker compose up -d
   ```
3. **Nginx** — copy `nginx-tk.conf` to `/etc/nginx/sites-available/tk.danbock.net`,
   symlink into `sites-enabled/`, **comment out the 443 server block** (the cert
   doesn't exist yet), then `sudo nginx -t && sudo systemctl reload nginx`.
4. **Cert** — `sudo certbot certonly --nginx -d tk.danbock.net`
5. Re-enable the 443 block, then `sudo nginx -t && sudo systemctl reload nginx`.
6. **Install WordPress** — open https://tk.danbock.net and follow the
   5-minute install. Pick any admin username/password; this is a trial box.

## Day to day

- The DB and uploads live in named volumes (`db-data`, `wp-data`) and survive
  restarts and redeploys.
- Update WP: `docker compose pull && docker compose up -d`
- Tear everything down: `docker compose down -v`, remove the nginx vhost
  symlink, delete the DNS record. Nothing else was affected.

## After the trial (someday)

The original site appears to have been built with the **Divi theme / Divi
Visual Builder** from Elegant Themes (the old `site/wp-content/` folders hint
at a WP export). Before migrating for real, ask Matt whether the organization
still has an Elegant Themes license, and whether the original WP install still
exists somewhere — its content may be exportable via WordPress' XML export.
