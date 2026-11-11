# Elevator Lead Radar by Showroom4K

Every Texas elevator on one map, colored by inspection status, with lead scores, a pipeline and outreach drafts.
The app is two files that must sit side by side: `index.html` and `maplibre-gl-csp-worker.js` (the map engine's worker).
Any static host can serve them: GitHub Pages, Vercel or Netlify.

## Satellite imagery

Satellite view turns on by itself when the page runs at its own web address.

- **USGS aerial (default, free):** public domain aerial photos of Texas with roads and place names. No key needed.
- **Mapbox (sharpest, closest to Google's satellite view):** create a free Mapbox account, copy your default public token
  (it starts with `pk.`), and paste it into `mapboxToken` in the `window.ELR_CONFIG` block near the bottom of `index.html`.
  Every visitor then gets Mapbox imagery. In your Mapbox account, restrict the token to your domain.
- **Esri:** same idea with an ArcGIS Location Platform key in `esriKey`.

You can also paste a token in the app under Profile and settings, Satellite imagery. That setting only applies to your browser.

Claude's artifact preview blocks images from other servers, so the preview always shows the drawn Texas map instead.

## Pinning buildings to street addresses

When you load the TDLR elevator file, the app looks up street addresses with the free US Census geocoder,
nearest and highest-scoring buildings first, and moves each pin from its ZIP code center to the street address.
Results are saved in the browser, so the next visit is instant. The weekly data job in the build spec replaces this
with batch geocoding on the server.

## Custom domain

To serve it at radar.showroom4k.com, add the domain in your host's settings, then add a CNAME record named `radar`
in the showroom4k.com DNS that points to the host (for GitHub Pages: `<your-username>.github.io`).

## Data and privacy

The example buildings are made up and labeled as examples. A loaded TDLR file stays in the visitor's browser;
nothing is uploaded to a server. Saved leads, notes and settings are stored in the browser too.
