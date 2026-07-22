# Fishing Spots

Interactive map app for cataloguing fishing spots across the BC East Kootenay (Fairmont / Columbia Valley, Cranbrook / Elk Valley) and Calgary area.

## Architecture

Single-file app — everything lives in `index.html`: HTML, CSS, and vanilla JS. No build step, no framework, no bundler.

- **Map**: Leaflet 1.9.4 (loaded from unpkg CDN) with OSM / OpenTopoMap / Esri Satellite tile layers
- **Data**: All spots stored in `localStorage` under key `fishing-spots-v1`. Seed spots (researched, baked into the JS) are merged on first load and tracked separately so deleted seeds don't resurrect
- **Geometry types**: marker (pin), line (river stretch), polygon (zone) — each spot has exactly one
- **Ratings**: 4 criteria (Fish quality, Action, Ease of access, Scenery), each 0–5 stars. Overall score = mean of rated criteria
- **Regions**: `fairmont`, `south` (Cranbrook/Elk Valley), `calgary`, `other`

`spots-import-columbia-valley.json` is a standalone export file with researched seed data — not loaded at runtime (the same data is in the `SEED_SPOTS` array in the JS).

## How to run

Open `index.html` in a browser. No server needed (though a local server avoids CORS issues with some tile providers):

```sh
npx serve .
# or
python3 -m http.server
```

## Conventions

- No dependencies to install — keep it that way. The app is intentionally a single portable HTML file.
- Dark/light theme via `prefers-color-scheme` using CSS custom properties.
- All user-facing strings are in the HTML/JS, not externalized.
- Score colors: green (4.5+) → yellow-green → amber → orange → red (< 1.5), grey for unrated.
- Escape all user content with `escapeHtml()` before inserting into the DOM.

## Testing

No test suite. To verify changes: open the app, add/edit/delete a spot, check the map renders, confirm import/export round-trips, and test both light and dark mode.
