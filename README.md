# Elevator Lead Radar by Showroom4K

Every Texas elevator on one map, colored by inspection status, with lead scores, a pipeline, outreach drafts
and a 3D view of each building.
The app is two files that must sit side by side: `index.html` and `maplibre-gl-csp-worker.js` (the map engine's worker).
Any static host can serve them: GitHub Pages, Vercel or Netlify.

## 3D view of a building

Open a building and press **View in 3D** (or the `3` key). The view flies to the building, highlights it in its
status color and slowly circles it. **Next lead** and **Prev** (or the arrow keys) walk the ranked list, the way you
would on a sales call. If the wrong building is highlighted, click the right one; the app remembers it in that browser.

There are two levels:

- **Free, no key:** 3D building shapes from OpenStreetMap (served by OpenFreeMap) over aerial imagery. The lead's
  building is drawn at its real outline, at least as tall as its TDLR floor count.
- **Photo-real (Google key):** Google Photorealistic 3D Tiles, the real 3D mesh of the city built from aerial photos,
  drawn with CesiumJS. This is the look in Google Earth. Coverage is metro-first: downtowns render in full, and some
  small-town buildings have no 3D mesh.

### Turning on photo-real 3D

1. In the Google Cloud console, create a project and turn on billing (a card is required even for the free allowance).
2. Under APIs & Services, enable **Map Tiles API**.
3. Create an API key. Under Website restrictions add your site's addresses, for example
   `https://radar.showroom4k.com/*` and `https://jmojr1010-ship-it.github.io/*`. Under API restrictions, pick Map Tiles API.
4. Paste the key into `googleKey` in the `window.ELR_CONFIG` block near the bottom of `index.html`, so every visitor gets
   it. To try it first, paste it in the app under Profile and settings, Satellite and 3D, and press **Save and check**;
   that only applies to your browser and tells you exactly what's wrong if Google refuses the key.

The same key also switches the 2D map to Google's satellite imagery with street names.

**Cost** (Google's price list, November 2026): Google bills photo-real 3D per root tileset request, which the app
makes once when a visitor first opens the 3D view and then reuses for every lead in that visit. Each month the first
1,000 of those and the first 100,000 map tiles are free. After that, 3D is $6.00 per 1,000 and 2D tiles $0.60 per
1,000. Set a budget alert in Google Cloud on day one.

**Rules that come with Google's tiles:** display only. Don't screenshot the 3D view for postcards or mailers; for
print, use a 2D static map instead. The app keeps Google's logo and data credits on screen as Google requires.
The 3D view has no place search of its own, so no other company's geocoder is used with Google's tiles.

## Satellite imagery

Satellite view turns on by itself when the page runs at its own web address.

- **USGS (default, free):** public domain aerial photos with roads and place names. When you zoom to building level,
  the app switches to the USGS 1-foot city imagery where it exists (NAIP Plus), so close-ups stay sharp.
- **Google:** with a Google key (see above).
- **Mapbox:** paste a Mapbox public token (starts with `pk.`) into `mapboxToken`. Restrict the token to your domain.
- **Esri:** an ArcGIS Location Platform key in `esriKey`.

Claude's artifact preview blocks images and data from other servers, so the preview shows the drawn Texas map and a
simple 3D marker instead.

## Pinning buildings to street addresses

When you load the TDLR elevator file, the app looks up street addresses with the free US Census geocoder,
nearest and highest-scoring buildings first, and moves each pin from its ZIP code center to the street address.
Results are saved in the browser, so the next visit is instant. The weekly data job in the build spec replaces this
with batch geocoding on the server.

## Custom domain

To serve it at radar.showroom4k.com, add the domain in your host's settings, then add a CNAME record named `radar`
in the showroom4k.com DNS that points to the host (for GitHub Pages: `jmojr1010-ship-it.github.io`).
Add the new address to the Google key's website restrictions too.

## Data and privacy

The example buildings are made up and labeled as examples. A loaded TDLR file stays in the visitor's browser;
nothing is uploaded to a server. Saved leads, notes, settings and a Google key typed into settings are stored in
the browser too.

## Credits

Building shapes © OpenStreetMap contributors, served by OpenFreeMap (OpenMapTiles schema). Aerial imagery: USGS,
USDA, The National Map. 3D engine: CesiumJS (Apache 2.0). Basemap: Natural Earth, US Census Bureau.
