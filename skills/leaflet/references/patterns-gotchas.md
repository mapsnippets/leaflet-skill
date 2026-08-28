# Leaflet Patterns & Common Gotchas

This document details the top 10 most frequent bugs and traps encountered when building with Leaflet, along with verified fixes.

---

### Gotcha 1: Inverted Coordinates (`[lat, lng]` vs `[lng, lat]`)
* **Problem:** GeoJSON, MapLibre, Turf.js, and REST APIs return `[longitude, latitude]`. Leaflet methods (`L.latLng`, `setView`, `L.marker`) require `[latitude, longitude]`. Passing raw GeoJSON points to Leaflet places markers in Antarctica or the ocean.
* **Fix:** Explicitly swap: `const leafletCoords = [geoJsonCoord[1], geoJsonCoord[0]];`

---

### Gotcha 2: Grey / Broken Tiles on Load (`invalidateSize`)
* **Problem:** If a map container initializes while hidden (e.g., inside an inactive tab, Bootstrap modal, or before CSS flexbox finishes rendering), Leaflet cannot calculate dimensions and renders partial grey tiles.
* **Fix:** Trigger `invalidateSize()` after the container becomes visible:
```javascript
// On modal or tab shown:
setTimeout(() => {
  map.invalidateSize();
}, 200);
```

---

### Gotcha 3: Missing Leaflet CSS
* **Problem:** Map tiles display stacked vertically or disjointed in the DOM; zoom controls appear distorted.
* **Fix:** Ensure `leaflet.css` is imported in your root JS/TS file:
```javascript
import "leaflet/dist/leaflet.css";
```

---

### Gotcha 4: Default Marker Icon 404 in Webpack / Vite / Rollup
* **Problem:** Bundlers rewrite image assets, causing default `marker-icon.png` and `marker-shadow.png` to 404.
* **Fix:** Re-assign icon URLs or use SVG / `L.divIcon`:
```javascript
import L from "leaflet";
import iconUrl from "leaflet/dist/images/marker-icon.png";
import iconRetinaUrl from "leaflet/dist/images/marker-icon-2x.png";
import shadowUrl from "leaflet/dist/images/marker-shadow.png";

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconUrl,
  iconRetinaUrl,
  shadowUrl
});
```

---

### Gotcha 5: "Map container is already initialized" (React / Hot Reload)
* **Problem:** In React (especially React 18 Strict Mode), components mount twice in development, causing `Error: Map container is already initialized`.
* **Fix:** Clean up in `useEffect` return function:
```javascript
useEffect(() => {
  const map = L.map(containerRef.current).setView([lat, lng], zoom);
  return () => {
    map.remove();
  };
}, []);
```

---

### Gotcha 6: Blurry 512px Tiles
* **Problem:** MapTiler raster tiles are 512×512 high-resolution tiles. Using default Leaflet `tileSize: 256` stretches and blurs the tiles.
* **Fix:** Always specify `tileSize: 512` and `zoomOffset: -1`:
```javascript
L.tileLayer(url, {
  tileSize: 512,
  zoomOffset: -1
}).addTo(map);
```

---

### Gotcha 7: Memory Leaks on Page Navigation
* **Problem:** Navigating away from a page without calling `map.remove()` leaves event listeners on `window` and DOM references active.
* **Fix:** Call `map.remove()` in component teardown lifecycle.
