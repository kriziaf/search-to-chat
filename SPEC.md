# Search-to-Chat — Component Spec

Status: **built, deployed as "Ask MD Live"** (v0.10.1 of the underlying UI).

## What it is

A reusable search → recommended-questions → chat-style answer → source-article UI pattern: a user types or picks a question, gets a conversational answer with cited sources, and can click through to a full article with its own "related articles" and a link back to the real source material.

It was originally built as `Research-Wiki`'s civic-tech research wiki, then reskinned on that repo's `feat/search-ui-generic` branch into **"Ask MD Live,"** a virtual-healthcare help-chat demo.

## Origin

Spun out of the `Research-Wiki` repo (a civic-tech research portal) on 2026-11-18, as a single fresh commit with no shared git history — the component belongs to a different, healthcare-focused project. `Research-Wiki`'s `feat/search-ui-generic` branch holds the pre-spin-out commits for reference but is intentionally never merged to that repo's `main`.

## Architecture

Static client-side app, no build step, no dependencies beyond Google Fonts (loaded via `<link>` in `index.html`):

- **`index.html`** — markup for every screen (search, ask, article, related, topics, graph, ingest log), all present in the DOM at once and toggled via `data-screen` on `#research-app`.
- **`research.css`** — all styling.
- **`research.js`** — all logic: search/ranking, the chat-style Q&A flow, article rendering, the `[[wikilink]]`-based relationship graph, topics/tag browsing, Graph View (hand-rolled Canvas2D, no D3), and the ingest log screen.
- **`data/wiki-index.js`** — the content: `window.WIKI_INDEX`, an array of documents.
- **`data/ingest-log.js`** — `window.INGEST_LOG`, a changelog-style list shown on the Ingest Log screen.

## Current dataset

6 curated mock Q&A articles, one per healthcare category, sourced from the real `github.com/kriziaf/help-center-chat` knowledgebase (see that repo's `KNOWLEDGEBASE_SPEC.md`):

| Category | Doc id | Real source file |
|---|---|---|
| General Info | `concepts/mdlive-service-basics` | `public/general-info/mdlive-service-basics.md` |
| Dermatology | `concepts/dermatology-acne-treatment` | `public/dermatology/dermatology-acne-treatment.md` |
| Insurance | `concepts/insurance-coverage-basics` | `public/insurance/insurance-coverage-basics.md` |
| Men's Health | `concepts/mens-health-prescriptions` | `public/mens-health/mens-health-prescriptions.md` |
| Mental Health | `concepts/mental-health-appointments` | `public/mental-health/mental-health-appointments.md` |
| Pediatric Care | `concepts/pediatric-care-vaccines-wellness` | `public/pediatric-care/pediatric-care-vaccines-wellness.md` |

Each doc carries a real `sourceUrl` (GitHub blob link) and `sourceAuthor`, which the article screen renders as a "Read full article →" link.

## Screens

- **Search** (home) — hero, search input, "Recommended" question chips (`RECOMMENDED_TOPICS` in `research.js`), "Recent" chips (from `localStorage`).
- **Ask** — chat-style question/answer with a sources chip and inline "Continue reading" link into the full article.
- **Article** — full doc body, tags, source link, and a "View related articles" CTA.
- **Related Articles** — ranked by shared tags and `[[wikilink]]` adjacency (`findRelated()` in `research.js`).
- **Topics** — browse all docs grouped by tag.
- **Graph View** — Canvas2D node graph of doc relationships, built from `[[Title]]` references inside each doc's `body` (`wikilinkGraph` in `research.js`).
- **Ingest Log** — changelog view driven by `data/ingest-log.js`.

## Swapping content for a new deployment

1. Replace `data/wiki-index.js`'s `WIKI_INDEX` array with new docs matching the existing schema: `id`, `title`, `type`, `tags` (array), `created`, `updated` (dates), `body` (markdown string, supports `[[Title]]` wikilinks and `## ` headings), and optionally `sourceUrl`/`sourceAuthor`.
2. Update `RECOMMENDED_TOPICS` near the top of `research.js` to `{ id, question }` pairs pointing at the new doc ids — this drives the home screen's chips.
3. Update branding copy in `index.html` (`<title>`, wordmark, hero `<h1>`/subtitle, search placeholder) and the footer label in `research.js` (`wire()`, `'Ask MD Live v' + APP_VERSION`).
4. Optionally replace `data/ingest-log.js`'s `INGEST_LOG` array (`{date, articles: [title, ...]}` entries) to reflect the new content.

No other files need to change — `research.css` and the rest of `research.js` are generic and count-driven.
