# ticketsforteachers.us — static site

A static, no-runtime site for https://ticketsforteachers.us/. We serve it as
flat files with nginx — no WordPress, no PHP, no Node.

## Structure

```
site/                 # web root — deploy this directory
  index.html          # home  (/)
  about/index.html    # about (/about/)
  wp-content/ ...      # theme fonts, images, logos
  wp-includes/ ...     # jQuery
nginx.conf            # server block (static serving + /91-2/ -> /about/ redirect)
```

The pages are fully static: all CSS is inlined in the HTML, and the only
interactivity is `mailto:` / external links. There is no JavaScript except a
JSON-LD SEO schema `<script>` (not executable code).

## Run locally

```sh
cd site
python3 -m http.server 8000
# open http://localhost:8000/  and  http://localhost:8000/about/
```

## Deploy to the VPS

See the header comment in `nginx.conf`.
