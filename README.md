# zao.pt

Personal site. Hand-written HTML and CSS — no build step, no dependencies.

```
index.html    all content
style.css     one stylesheet, light + dark
```

## Develop

Open `index.html` in a browser. That's it.

## Deploy

Cloudflare Workers (static assets), connected to `joaobzao/zao` via the dashboard's Git
integration — pushes to `main` deploy automatically. No build command; assets are served
from the repo root, including `assets/`.

- `zao.capas.workers.dev` — the deployment
- `zao.pt` — custom domain
