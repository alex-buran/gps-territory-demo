# Territory Demo

A single-file browser prototype of GPS-based territory capture. Start an activity, move, and every grid tile your route crosses is claimed and filled in behind you.

Built as a technical proof for a gamified fitness app concept: run, walk or cycle to take control of real-world map zones.

## Demo

Open `index.html`, or deploy the folder anywhere static.

Two ways to run it:

- **Demo mode** simulates a ~3.2 km loop and completes in about 20 seconds. No permissions needed, works indoors.
- **Live GPS** tracks your real position via the browser Geolocation API. Requires HTTPS and location permission.

## What it shows

- Live position tracking with a drawn route polyline
- A lat/lng grid at roughly 46 m per cell
- Tile capture as the route enters a new cell, with an animated fill
- Running stats: elapsed time, distance, zones captured
- An end-of-activity summary with pace per kilometre

## Notes on the implementation

**Distance** uses the haversine formula between consecutive fixes rather than straight-line start-to-end, so it reflects the actual path walked.

**Grid cells** are indexed by `floor(lat / CELL)` and `floor(lng / CELL)`, which makes capture an O(1) set insert. Cell size is a constant in degrees, so cells narrow slightly toward the poles. For a production build this would move to H3 or S2 for equal-area cells.

**Accuracy filtering** discards fixes reporting worse than 60 m accuracy. Without it, a single bad urban-canyon fix teleports the route across several blocks and falsely claims every tile in between. This is the simplest form of the anti-spoofing problem a real version has to solve properly, alongside speed plausibility checks and vehicle detection.

**Simulated time** in demo mode advances faster than wall-clock so the loop completes quickly, but the displayed duration is scaled to a realistic running pace. Showing true elapsed time would report a pace of several hundred km/h.

## Stack

Leaflet 1.9 with CARTO dark basemap tiles. No API key, no build step, no dependencies beyond the CDN. One HTML file.

## Deploy

Drag the folder onto [Netlify Drop](https://app.netlify.com/drop), or push to a repo and enable GitHub Pages. Geolocation requires a secure context, so it will not work from a `file://` URL.

## Limitations

This is a prototype, not a product. There is no backend, no persistence, no accounts, and no multi-user contest for zones. Reloading the page clears everything. A real version needs a server-authoritative zone ledger, since anything computed client-side can be forged.

## Licence

MIT
