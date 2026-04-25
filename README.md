# @openric/capture

A pure-browser data-entry tool for any server that implements the **[OpenRiC](https://openric.org)** Viewing + Write API (`/api/ric/v1/*`). You supply a server URL and an API key; the app POSTs captured entities straight to that server. No backend. No Heratio assumptions.

Live demo: **[capture.openric.org](https://capture.openric.org)**
Sibling project: **[@openric/viewer](https://github.com/openric/viewer)**

## What it does

- Create **Records**, **Agents**, **Repositories**, **Functions**, **Places**, **Rules**, **Activities**, **Instantiations**, and **Relations** against any OpenRiC-conformant server (8 entity types per spec v0.37 §6).
- Pull the taxonomy (type pickers) live from `/api/ric/v1/vocabulary/{taxonomy}` — so form options always match whatever taxonomy the server actually publishes.
- Store the server URL + API key in `localStorage` — never sent anywhere except the server you configured.
- Log the last 20 captures in `localStorage` so you can verify and navigate.

## Spec v0.37 dual-read

Capture is **namespace-agnostic by design**: forms POST native DB-column field names (`name`, `title`, `start_date`, etc.), not JSON-LD CURIEs. On *load-for-editing* the `loadForEditing` helper strips every `@*` and `xxx:yyy` key from server responses, reading only the native columns. This means Capture works unchanged against:

- **Pre-spec-v0.37 servers** that emit `rico:heldBy` / `rico:hasInstantiation` / `rico:Production`
- **Spec-v0.37+ servers** that emit `rico:hasOrHadHolder` / `rico:hasOrHadInstantiation` / `rico:Activity` + `rico:hasActivityType`
- **Servers using the `openricx:` extension namespace** for description / contact / DateRange / etc.

The tile help text reflects spec v0.37 framing (Mechanism is a canonical Agent subclass; Repository / Function are API surfaces with documented canonical mappings; Activity uses `hasActivityType` IRIs).

## Why standalone

OpenRiC is implementation-neutral. The viewer was extracted from Heratio to prove that any OpenRiC-conformant server can drive it; this tool is the mirror of that story on the capture side. The Heratio reference implementation has its own capture UI at `heratio.theahg.co.za/ric-capture` — this is *not* that.

## Run locally

Zero build step. Open `docs/index.html` in a browser, or:

```bash
python3 -m http.server -d docs 8000
# http://localhost:8000
```

## Deploy

GitHub Pages serves `docs/` on the `main` branch. Custom domain via `docs/CNAME`.

## Licence

AGPL-3.0-or-later.
