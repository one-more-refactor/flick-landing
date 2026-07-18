# flick-landing

The marketing landing page for **flick** — the hosted speed-reading service at
[flick.cr3do.net](https://flick.cr3do.net). It is intentionally **separate** from
the flick app (Rust + Svelte, in the `flick` repo): different job, different
stack, deployed independently. This site is hosted-only; the self-hosted edition
ships its own in-app guest door.

## Stack

- **Astro** (static output) — content + SEO, zero JS by default.
- Interactive islands: the auto-running RSVP hero, scroll choreography, and an
  ambient animated background.
- Motion: **GSAP** (scroll reveals), **Lenis** (smooth scroll), **anime.js**
  (micro-interactions), **Vanta**/three.js (ambient background, lazy-loaded and
  disabled under `prefers-reduced-motion`).

## Brand

Design tokens are ported from the app (`src/styles/tokens.css`) so the two
share one look: monospace, square corners, one accent (`--accent`), light/dark
× six themes. The theme is read from `flick.mode` / `flick.theme` in
localStorage (shared with the app on the same origin) before first paint.

## Develop

```sh
bun install
bun run dev      # http://localhost:4321
bun run build    # -> dist/
bun run check    # astro/type check
```

## Deploy

Static `dist/` — serve behind Caddy at the root of flick.cr3do.net, with the app
under a path or subdomain (see `src/config.ts` `APP_URL`).
