# ctsmith-blog

Basalt **instance** for writing. Engine is [adobebulk/basalt](https://github.com/adobebulk/basalt) v0.4.0. This repo holds instance config, content, and a template overlay — not a fork of Functions or admin.

- Public: `https://blog.ctsmith.org/` (once DNS / Pages domain is attached)
- Admin: `/admin` (Cloudflare Access)
- Config: `site/data/basalt.yaml` — posts only, `homepage: posts`
- Overlay: `site/themes/site/` in front of `basalt` (`theme = ["site", "basalt"]`)
- Color: Rams blue `#003594`. Light mode black text on mist; dark mode white text on blue.

Do not reuse photos or ctsmith.org R2 buckets, tokens, or deploy hooks.

## Local

```bash
npm install
npm run dev
```

`npm run dev` copies `.dev.vars.example` → `.dev.vars` if that file is missing. Add a real `GITHUB_TOKEN` there to save from admin.

[http://localhost:1313](http://localhost:1313) and [http://localhost:1313/admin](http://localhost:1313/admin). That `dev` script serves Hugo output **and** `/api` (Wrangler) on 1313. Stop the old Hugo-only process first so the port is free.

`npm run hugo:dev` is Hugo only (no admin API).

## Cloudflare (you)

1. Pages project **ctsmith-blog**, connect this repo. Build `npm run build`, output `site/public`, env `HUGO_VERSION=0.161.1`.
2. R2 buckets `ctsmith-blog-assets` and `ctsmith-blog-originals`. Custom domain on the assets bucket → `r2.blog.ctsmith.org` (must match `ASSETS_R2_PUBLIC_URL`).
3. Bindings: `ASSETS_BUCKET`, `ORIGINALS_BUCKET`. Vars from `wrangler.toml`. Secrets: `GITHUB_TOKEN`, `DEPLOY_HOOK_URL`. Optional: `CF_ACCOUNT_ID`, `CF_API_TOKEN` (Pages Read).
4. Custom domain `blog.ctsmith.org`.
5. Access app on `blog.ctsmith.org` for `/admin*` and `/api*` only.

## Updating the engine

Merge on `basalt`, then PR a copy/submodule bump here. Do not edit `functions/` or `admin/` in this repo unless you intend to stop following the engine.
