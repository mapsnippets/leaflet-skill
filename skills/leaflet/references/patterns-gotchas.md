# Leaflet Patterns & Production Gotchas ⚠️🛠️

> Comprehensive failure-mode directory and architectural solutions for the top 12 most frequent bugs, memory leaks, projection traps, and performance bottlenecks in Leaflet production applications.

---

## 1. Inverted Coordinates (`[lat, lng]` vs `[lng, lat]`)

* **Problem:** GeoJSON, MapLibre, Turf.js, WKT, and REST APIs return `[longitude, latitude]`. Leaflet methods (`L.latLng`, `setView`, `L.marker`, `L.polyline`) require `[latitude, longitude]`. Passing raw GeoJSON points directly to Leaflet places markers in Antarctica or the ocean.
* **Fix:** Explicitly swap coordinates before feeding into Leaflet:
```javascript
// GeoJSON coordinate: [14.4378, 50.0755] (lng, lat)
const geojsonPoint = [14.4378, 50.0755];

// Correct Leaflet position: [50.0755, 14.4378] (lat, lng)
const marker = L.marker([geojsonPoint[1], geojsonPoint[0]]).addTo(map);

// When using L.GeoJSON, Leaflet handles inversion automatically via coordsToLatLng:
L.geoJSON(geojsonFeatureCollection).addTo(map);
```

---

## 2. Grey / Broken Tiles on Load (`invalidateSize`)

* **Problem:** If a map container initializes while hidden (e.g. inside an inactive Bootstrap/Tailwind tab, modal dialog, accordion, or CSS flexbox/grid before layout computation finishes), Leaflet cannot compute pixel dimensions and renders partial grey tiles or incorrect zoom levels.
* **Fix:** Trigger `map.invalidateSize()` after the container becomes visible, or observe with `ResizeObserver`:
```javascript
// Solution A: Modern ResizeObserver (Recommended for SPAs)
const mapContainer = document.getElementById('map');
const resizeObserver = new ResizeObserver(() => {
  map.invalidateSize({ debounceMoveend: true });
});
resizeObserver.observe(mapContainer);

// Solution B: Modal / Tab event callback
bootstrapModal.addEventListener('shown.bs.modal', () => {
  map.invalidateSize();
});
```

---

## 3. Missing Leaflet CSS

* **Problem:** Map tiles display stacked vertically in a single column or disjointed in the DOM; zoom controls appear distorted or floating in random positions.
* **Fix:** Ensure `leaflet.css` is imported in your root JS/TS entry point or HTML `<head>`:
```javascript
// In ES module bundlers (Vite, Webpack):
import 'leaflet/dist/leaflet.css';
```
```html
<!-- In plain HTML: -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
```

---

## 4. Default Marker Icon 404 in Webpack / Vite / Rollup

* **Problem:** Modern bundlers rewrite image asset URLs, causing default Leaflet `marker-icon.png` and `marker-shadow.png` to fail with HTTP 404 errors.
* **Fix:** Re-assign icon URLs or use SVG / `L.divIcon`:
```javascript
import L from 'leaflet';
import iconUrl from 'leaflet/dist/images/marker-icon.png';
import iconRetinaUrl from 'leaflet/dist/images/marker-icon-2x.png';
import shadowUrl from 'leaflet/dist/images/marker-shadow.png';

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconUrl,
  iconRetinaUrl,
  shadowUrl
});
```

---

## 5. "Map container is already initialized" (React / Hot Reload)

* **Problem:** In React (especially React 18 Strict Mode) or Vue with hot module reloading, components mount twice during development, causing `Error: Map container is already initialized`.
* **Fix:** Properly clean up the map instance during component unmount:
```javascript
// React:
useEffect(() => {
  const map = L.map(containerRef.current).setView([50.0755, 14.4378], 13);
  return () => {
    map.remove(); // Removes DOM elements, clears event listeners
  };
}, []);

// Vue 3:
// Use shallowRef to avoid Vue's deep reactivity wrapping Leaflet's internal DOM references
const map = shallowRef(null);
onMounted(() => {
  map.value = L.map(containerRef.value).setView([50.0755, 14.4378], 13);
});
onUnmounted(() => {
  if (map.value) map.value.remove();
});
```

---

## 6. Invalid `/512/` Path in Tile URLs & Blurry 512px High-DPI Tiles

* **Problem A (Invalid `/512/` Path):** Adding `/512/` into the URL (e.g. `https://api.maptiler.com/maps/streets-v4/512/{z}/{x}/{y}@2x.png`) fails with HTTP error! 512px is the **native default** on MapTiler Cloud, so there is **no `/512/` path prefix**.
  * ❌ **INVALID:** `https://api.maptiler.com/maps/streets-v4/512/{z}/{x}/{y}@2x.png?key=KEY`
  * ✅ **VALID (512px Default):** `https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=KEY`
  * ✅ **VALID (512px Retina @2x):** `https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}@2x.png?key=KEY`
  * ✅ **VALID (256px Legacy):** `https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=KEY` (only 256px requires explicit `/256/` prefix)
* **Problem B (Blurry 512px Tiles in Leaflet):** MapTiler raster tiles are 512×512 high-resolution tiles. Using default Leaflet `tileSize: 256` stretches and blurs the tiles, doubling perceived pixel size.
* **Fix:** In Leaflet's `L.tileLayer` options, always specify `tileSize: 512` and `zoomOffset: -1`:
```javascript
// High-resolution 512px standard tiles
L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=KEY', {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '&copy; MapTiler &copy; OpenStreetMap contributors'
}).addTo(map);

// Crisp High-DPI Retina 512px tiles (@2x)
L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}@2x.png?key=KEY', {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '&copy; MapTiler &copy; OpenStreetMap contributors'
}).addTo(map);
```

---

## 7. Memory Leaks on Page Navigation

* **Problem:** Navigating away from a page or route without calling `map.remove()` leaves active event listeners on `window`, interval timers, and heavy tile canvas buffers in memory.
* **Fix:** Call `map.remove()` in component teardown lifecycle. Ensure any custom intervals or plugin timers (e.g. `L.realtime`, GPS `watchPosition`) are explicitly cancelled.

---

## 8. Event Propagation / Click-Through Trap

* **Problem:** Clicking a button or custom HTML control inside the map also triggers the map's `click` event or starts a drag/zoom gesture.
* **Fix:** Use `L.DomEvent.stopPropagation` and `L.DomEvent.disableClickPropagation`:
```javascript
const customControlDiv = L.DomUtil.create('div', 'my-control');

// Prevent map pan/zoom when interacting with control
L.DomEvent.disableClickPropagation(customControlDiv);
L.DomEvent.disableScrollPropagation(customControlDiv);

// Stop custom button clicks from firing map.on('click')
button.addEventListener('click', (e) => {
  L.DomEvent.stopPropagation(e);
  // Custom logic...
});
```

---

## 9. Custom Panes & Pointer-Events Trapping

* **Problem:** Creating a custom pane with a high `z-index` (e.g. to place vector boundaries over basemap labels) blocks mouse clicks on underlying markers or tiles, even if the custom pane is transparent.
* **Fix:** Set `pointer-events: none` on the pane container, and `pointer-events: auto` on the interactive vector paths:
```javascript
const labelPane = map.createPane('customLabels');
labelPane.style.zIndex = '650';
labelPane.style.pointerEvents = 'none'; // Clicks pass through to markers underneath

// If specific paths inside the pane need to be clicked:
const path = L.polyline(coords, {
  pane: 'customLabels',
  interactive: true // Leaflet enables pointer-events on this SVG element
}).addTo(map);
```

---

## 10. Vector Performance Degradation (Canvas vs SVG)

* **Problem:** Leaflet renders vector paths (`L.Polyline`, `L.Polygon`, `L.CircleMarker`) as individual inline SVG DOM elements by default. When displaying more than 1,000 features, the DOM tree becomes bloated and frame rates plummet to <15 FPS during pan/zoom.
* **Fix:** Switch vector rendering to HTML5 Canvas:
```javascript
// Option A: Enable globally on map
const map = L.map('map', {
  preferCanvas: true // Renders all vector paths onto a shared Canvas
});

// Option B: Dedicated canvas renderer with custom padding
const canvasRenderer = L.canvas({ padding: 0.5 });
L.geoJSON(largeDataset, {
  renderer: canvasRenderer
}).addTo(map);
```

---

## 11. Non-Geographic Coordinate Systems (`L.CRS.Simple`)

* **Problem:** Rendering indoor floorplans, video game maps, or ultra-high-resolution microscopy images using default `L.CRS.EPSG3857` distorts distances and clips coordinates outside `[-85.05, 85.05]` latitude.
* **Fix:** Use `L.CRS.Simple`, where `[y, x]` directly maps to pixels:
```javascript
const map = L.map('map', {
  crs: L.CRS.Simple,
  minZoom: -2,
  maxZoom: 2
});

const bounds = [[0, 0], [1000, 1000]]; // [height, width] in pixels
L.imageOverlay('floorplan.png', bounds).addTo(map);
map.fitBounds(bounds);
```

---

## 12. Popups with Dynamic Async Content

* **Problem:** Updating a popup's HTML content asynchronously (e.g. after a `fetch` request completes) causes the popup bubble to overflow or clip off-screen because Leaflet does not automatically recalculate dimensions.
* **Fix:** Call `popup.update()` immediately after modifying content:
```javascript
marker.on('click', async () => {
  const popup = L.popup().setContent('Loading sensor data...');
  marker.bindPopup(popup).openPopup();

  const response = await fetch(`/api/sensor/${id}`);
  const data = await response.json();

```

---

## 13. "Map has no maxZoom specified" with `L.maplibreGL` & Plugins (e.g. MarkerCluster)

* **Problem:** Adding `L.markerClusterGroup` or third-party plugins to a map throws:
  `Uncaught Map has no maxZoom specified (MarkerClusterGroup.js:593)`
  Unlike raster `L.tileLayer`, `L.maplibreGL` is an overlay layer hosting a WebGL canvas and does not populate `map.options.maxZoom`. Therefore, `map.getMaxZoom()` returns `Infinity`, triggering plugin validation errors.
* **Fix:** Explicitly define `{ maxZoom: 19 }` on `L.map` and/or pass `{ maxZoom: 19 }` to `L.markerClusterGroup`:
```javascript
// Always pass maxZoom on L.map when using vector basemaps
const map = L.map('map', { maxZoom: 19 }).setView([51.5072, -0.1276], 12);

L.maplibreGL({
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY'
}).addTo(map);

// MarkerClusterGroup safely reads maxZoom from map
const markers = L.markerClusterGroup({
  maxZoom: 19,
  maxClusterRadius: 50,
  spiderfyOnMaxZoom: true
});
map.addLayer(markers);
```

