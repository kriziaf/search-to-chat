# Search-to-Chat

A reusable search → chat-style answer → source-article UI pattern. Static client-side app (`index.html` + `research.css` + `research.js`) over a swappable JSON knowledge base.

Currently deployed as **"Ask MD Live"** — a virtual-healthcare help-chat demo with 6 mock Q&A articles. See [`SPEC.md`](./SPEC.md) for the component's architecture, screens, and how to swap in new content for a different deployment.

Spun out of the `Research-Wiki` repo's `feat/search-ui-generic` branch on 2026-11-18 — this component belongs to a different, healthcare-focused project and that source branch is intentionally never merged to `Research-Wiki`'s `main`.

## Running locally

Static files, no build step. Serve the directory with any static file server, e.g.:

```
python3 -m http.server 8743
```

then open `http://localhost:8743/index.html`.
