# flick-landing

The marketing site for [**flick**](https://github.com/one-more-refactor/flick) — the hosted speed-reading service at **[myflick.app](https://myflick.app)**.

It is intentionally **separate** from the app (Rust + Svelte, in [flick-backend](https://github.com/one-more-refactor/flick-backend) / [flick-web](https://github.com/one-more-refactor/flick-web)): different job, different stack, deployed independently. This site is **hosted-only** — the self-hosted edition ships its own in-app guest door, so self-hosters never need this.

![flick.landing](docs/screenshots/landing.png)

## Stack

- **[Astro](https://astro.build)** (static output) — content + SEO, zero JS by default.
- Interactive islands: the auto-running RSVP hero, a pinned scroll scene that aligns to the ORP pivot, use-case vignettes.
- Motion: **[GSAP](https://gsap.com)** (scroll reveals) · **[Lenis](https://lenis.dev)** (smooth scroll) · **[anime.js](https://animejs.com)** (micro-interactions) · **[Vanta](https://www.vantajs.com)** + three.js (ambient dot-field — lazy-loaded, theme-synced, and disabled under `prefers-reduced-motion`).

> These libraries are why this site is a separate repo: some are **not** GPL-compatible, so keeping them out of the AGPL app avoids any licensing conflict. The app's own motion uses only the Web Animations API.

## Brand

Design tokens are ported from the app (`src/styles/tokens.css`) so the two share one look: monospace, square corners, one accent (`--accent`), light/dark. On the same origin the theme is read from `flick.mode` / `flick.theme` in `localStorage` before first paint, so there's no flash.

Where the CTAs point is in `src/config.ts` (`APP_URL`, `SITE`, `REPO`).

## Develop

```sh
bun install
bun run dev      # http://localhost:4321
bun run build    # -> dist/
bun run check    # astro + type check
```

## Deploy

Static `dist/`. Baked into `nginx:alpine` (`deploy/Containerfile`) and run rootless behind a Cloudflare Tunnel — see [`deploy/`](deploy). No exposed ports.

```sh
bun run build
podman build -t flick-landing -f deploy/Containerfile .
```

## License

[MIT](LICENSE) — this marketing site only. The flick app is [AGPL-3.0](https://github.com/one-more-refactor/flick/blob/master/LICENSE). Bundled third-party libraries (GSAP, three.js, …) carry their own licenses.
