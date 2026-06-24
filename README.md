# ticketsforteachers.us — static mirror

A static, no-runtime copy of https://ticketsforteachers.us/ (originally built
with WordPress + the Divi theme). We serve it as flat files with nginx — no
WordPress, no PHP, no Node.

## Structure

```
site/                 # web root — deploy this directory
  index.html          # home  (/)
  about/index.html    # about (/about/  — was WordPress slug /91-2/)
  wp-content/ ...      # theme JS, fonts, images, logos (paths kept from WP)
  wp-includes/ ...     # jQuery
nginx.conf            # server block (static serving + /91-2/ -> /about/ redirect)
```

The pages are fully static: all CSS is inlined in the HTML (SiteGround critical
CSS), and the only interactivity is `mailto:` / external links. **All JavaScript
has been removed** — jQuery, jquery-migrate, the 274 KB Divi `scripts.min.js`,
lazysizes, and et-core-common (~384 KB total). Nothing on these two pages needs
it. The only `<script>` kept is the JSON-LD SEO schema (not executable code).

## What was changed from the original

- `/91-2/` (about) is now served at `/about/`; canonical/og URLs and nav links
  updated accordingly. nginx 301-redirects the old `/91-2/` path.
- Local asset URLs rewritten from absolute `https://ticketsforteachers.us/...`
  to root-relative `/wp-content/...` and `/wp-includes/...` so they load from disk.
- Dead WordPress head tags removed (wp-json/REST, RSD/EditURI, shortlink, RSS &
  oEmbed links, generator meta) — none affect rendering.
- Fixed a broken `mailto::matt@...` link (double colon) on the home page.
- Removed all JavaScript and the corresponding `.js` files. Since the mobile
  hamburger menu was JS-driven, a small `<style id="static-overrides">` keeps the
  2-item nav visible on mobile and hides the (backend-less) search UI. The `js`
  class the inline script used to set is now baked into the `<html>` tag.
- External references left as-is (Google Fonts; the rootedindps.org partnership
  link; Gravatar/schema metadata).

## Run locally

```sh
cd site
python3 -m http.server 8000
# open http://localhost:8000/  and  http://localhost:8000/about/
```

## Deploy to the VPS

See the header comment in `nginx.conf`.

## Refresh from the live WordPress site

Re-download the two pages and assets, then re-run the path rewrite. The original
mirror was done with `curl` (the `wp-content`/`wp-includes` asset list is small;
see git history for the exact asset paths).
