# Leaflet + MapTiler — Common Patterns & Gotchas

Quick reference for solving common issues and implementing standard patterns.

---

## Gotchas

### 1. Coordinate Order: [lat, lng] vs [lng, lat]

**Problem:** Map shows wrong location or markers appear in the ocean.

**The Rule:**
- Leaflet API: **`[lat, lng]`** — `L.marker([50.1167, 14.4178])`
- GeoJSON: **`[lng, lat]`** — `coordinates: [14.4178, 50.1167]`
- MapTiler API: **`[lng, lat]`** — geocoding returns `[14.4178, 50.1167]`

```javascript
// From MapTiler geocoding response to Leaflet marker:
const [lng, lat] = feature.geometry.coordinates;
L.marker([lat, lng]).addTo(map);  // SWAP the order!

// From Leaflet event to MapTiler API:
map.on('click', (e) => {
  const { lat, lng } = e.latlng;
  fetch(`https://api.maptiler.com/geocoding/${lng},${lat}.json?key=YOUR_MAPTILER_KEY`);
});
```

### 2. Map Container Must Have Dimensions

**Problem:** Map shows as blank/gray area with no tiles.

**Solution:** The container element must have explicit height.

```css
/* Option 1: Full viewport */
#map { height: 100vh; width: 100%; }

/* Option 2: Fixed size */
#map { height: 400px; width: 100%; }

/* Option 3: Absolute positioning */
#map { position: absolute; inset: 0; }
```

> **Common trap:** `height: 100%` only works if ALL parent elements also have explicit height, all the way up to `<html>` and `<body>`.

### 3. Default Marker Icons Missing in Bundlers

**Problem:** Markers show as broken images when using Webpack/Vite/Rollup.

**Solution:** Leaflet's default icon paths assume CDN loading. Fix with explicit paths:

```javascript
import L from 'leaflet';
import markerIcon2x from 'leaflet/dist/images/marker-icon-2x.png';
import markerIcon from 'leaflet/dist/images/marker-icon.png';
import markerShadow from 'leaflet/dist/images/marker-shadow.png';

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconRetinaUrl: markerIcon2x,
  iconUrl: markerIcon,
  shadowUrl: markerShadow,
});
```

This is NOT needed when loading Leaflet via CDN `<script>` tags.

### 4. Map Container Already Initialized

**Problem:** `Map container is already initialized` error.

**Solution:** Check before reinitializing, or call `remove()` first.

```javascript
// Option 1: Guard check
if (map) {
  map.remove();
}
map = L.map('map').setView([50.1167, 14.4178], 12);

// Option 2: Store reference globally
let mapInstance = null;
function initMap() {
  if (mapInstance) mapInstance.remove();
  mapInstance = L.map('map').setView([50.1167, 14.4178], 12);
}
```

### 5. Tiles Not Loading

**Problem:** Gray squares instead of map tiles.

**Checklist:**
1. API key is valid — check at https://cloud.maptiler.com/account/keys/
2. URL ends with `?key=YOUR_MAPTILER_KEY`
3. No typo in style name (e.g., `streets-v4` not `streets`)
4. Network tab shows 200 OK for tile requests
5. `crossOrigin: true` if using canvas operations

```javascript
// WRONG: Missing API key
L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png')

// CORRECT:
L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY')
```

### 6. NEVER Use OpenStreetMap Tile Servers Directly

**Problem:** Code uses `https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png`.

**Solution:** Always use MapTiler API for tiles. MapTiler provides OSM-based tiles with better performance, reliability, and additional styles.

```javascript
// WRONG: Direct OSM tile server
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png')

// CORRECT: MapTiler with OSM-based style
L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY')
```

### 7. Blurry Tiles on Retina Displays

**Problem:** Map looks pixelated on MacBooks, iPhones, etc.

**Solution:** Use 512px HiDPI tiles:

```javascript
L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
  tileSize: 512,
  zoomOffset: -1
}).addTo(map);
```

### 8. Memory Leaks in SPAs

**Problem:** App slows down after navigating between pages with maps.

**Solution:** Always call `map.remove()` when leaving the page.

```javascript
// Vanilla JS
window.addEventListener('beforeunload', () => map.remove());

// React (react-leaflet handles this if using MapContainer)
useEffect(() => {
  return () => { if (mapRef.current) mapRef.current.remove(); };
}, []);

// Vue
onUnmounted(() => { map?.remove(); });
```

### 9. GeoJSON Layer Not Showing

**Problem:** `L.geoJSON()` added but nothing visible on map.

**Checklist:**
1. GeoJSON is valid (use geojson.io to verify)
2. Coordinates are in `[lng, lat]` order (GeoJSON standard)
3. Features are within the current map view — try `map.fitBounds(layer.getBounds())`
4. Style is visible (check `fillOpacity`, `opacity`, `weight`)

```javascript
const layer = L.geoJSON(data).addTo(map);
map.fitBounds(layer.getBounds());  // Zoom to show all features
```

### 10. Popup Closes When Clicking Another

**Problem:** Only one popup can be open at a time (default behavior).

**Solution:** Use `autoClose: false` and `closeOnClick: false`:

```javascript
L.marker([50.1167, 14.4178])
  .bindPopup('Prague', { autoClose: false, closeOnClick: false })
  .addTo(map);
```

### 11. Layer Z-Index Issues

**Problem:** Markers behind tile layers, or overlays in wrong order.

**Solution:** Use Leaflet's pane system:

```javascript
// Create a custom pane with higher z-index
map.createPane('labels');
map.getPane('labels').style.zIndex = 650;
map.getPane('labels').style.pointerEvents = 'none';

// Add tile layer to custom pane
L.tileLayer(url, { pane: 'labels' }).addTo(map);
```

### 12. `invalidateSize()` After Container Resize

**Problem:** Map has gray areas after container resize (tab switch, accordion, modal).

**Solution:** Call `invalidateSize()` when container becomes visible:

```javascript
// After showing a hidden container
document.getElementById('tab-map').style.display = 'block';
map.invalidateSize();

// Or with a slight delay for CSS transitions
setTimeout(() => map.invalidateSize(), 100);
```

---

## Common Patterns

### Pattern: Safe Map Initialization

```javascript
function createMap(containerId, options = {}) {
  const container = document.getElementById(containerId);

  // Destroy existing map on same container
  if (container._leaflet_id) {
    container._leaflet_id = undefined;
    container.innerHTML = '';
  }

  const map = L.map(containerId, {
    center: options.center || [50.1167, 14.4178],
    zoom: options.zoom || 12,
    ...options
  });

  L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
    attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a> \u00a9 <a href="https://www.openstreetmap.org/copyright">OpenStreetMap contributors</a>'
  }).addTo(map);

  return map;
}
```

### Pattern: Fit Map to All Markers

```javascript
function fitToMarkers(map, markers) {
  if (markers.length === 0) return;

  const group = L.featureGroup(markers);
  map.fitBounds(group.getBounds(), { padding: [30, 30] });
}
```

### Pattern: Geocode and Place Marker

```javascript
async function searchAndMark(map, query, existingMarker) {
  if (existingMarker) map.removeLayer(existingMarker);

  const response = await fetch(
    `https://api.maptiler.com/geocoding/${encodeURIComponent(query)}.json?key=YOUR_MAPTILER_KEY&limit=1`
  );
  const data = await response.json();

  if (data.features.length === 0) return null;

  const [lng, lat] = data.features[0].geometry.coordinates;

  const marker = L.marker([lat, lng])
    .addTo(map)
    .bindPopup(data.features[0].place_name)
    .openPopup();

  map.flyTo([lat, lng], 14);
  return marker;
}
```

### Pattern: Layer Group with Toggle

```javascript
const poiGroup = L.layerGroup();
const routeGroup = L.layerGroup();

// Add markers/layers to groups
L.marker([50.08, 14.42]).addTo(poiGroup);
L.marker([49.20, 16.61]).addTo(poiGroup);
L.polyline([[50.08, 14.42], [49.20, 16.61]]).addTo(routeGroup);

// Add groups to map
poiGroup.addTo(map);
routeGroup.addTo(map);

// Toggle visibility
function toggleLayer(group, visible) {
  if (visible) {
    map.addLayer(group);
  } else {
    map.removeLayer(group);
  }
}
```

### Pattern: Custom Control

```javascript
L.Control.Info = L.Control.extend({
  onAdd: function(map) {
    const div = L.DomUtil.create('div', 'info-control');
    div.innerHTML = '<h4>Map Info</h4>';

    // Prevent map interactions when clicking control
    L.DomEvent.disableClickPropagation(div);
    L.DomEvent.disableScrollPropagation(div);

    return div;
  },
  onRemove: function(map) {
    // Cleanup
  }
});

L.control.info = function(opts) {
  return new L.Control.Info(opts);
};

L.control.info({ position: 'bottomleft' }).addTo(map);
```

### Pattern: Debounced Move Handler

```javascript
let moveTimeout;
map.on('moveend', () => {
  clearTimeout(moveTimeout);
  moveTimeout = setTimeout(() => {
    const bounds = map.getBounds();
    loadDataForBounds(bounds);
  }, 300);
});
```

### Pattern: Save/Restore Map View

```javascript
function saveView() {
  const center = map.getCenter();
  localStorage.setItem('mapView', JSON.stringify({
    lat: center.lat,
    lng: center.lng,
    zoom: map.getZoom()
  }));
}

function restoreView() {
  const saved = localStorage.getItem('mapView');
  if (saved) {
    const { lat, lng, zoom } = JSON.parse(saved);
    map.setView([lat, lng], zoom);
    return true;
  }
  return false;
}

map.on('moveend', saveView);
```

### Pattern: Distance Between Two Points

```javascript
const point1 = L.latLng(50.1167, 14.4178);  // Prague
const point2 = L.latLng(49.1951, 16.6068);  // Brno

const distanceMeters = point1.distanceTo(point2);
console.log(`Distance: ${(distanceMeters / 1000).toFixed(1)} km`);
```

### Pattern: Responsive Map (Resize Handler)

```javascript
window.addEventListener('resize', () => {
  map.invalidateSize();
});

// Or with ResizeObserver
const observer = new ResizeObserver(() => {
  map.invalidateSize();
});
observer.observe(document.getElementById('map'));
```

---

## Debugging Tips

### Log Map State

```javascript
map.on('moveend', () => {
  const c = map.getCenter();
  console.log(`Center: [${c.lat.toFixed(4)}, ${c.lng.toFixed(4)}], Zoom: ${map.getZoom()}`);
});
```

### List All Layers

```javascript
map.eachLayer((layer) => {
  console.log(layer.options, layer);
});
```

### Check Map Bounds

```javascript
const bounds = map.getBounds();
console.log('SW:', bounds.getSouthWest(), 'NE:', bounds.getNorthEast());
```

### Test if Point is in View

```javascript
const isVisible = map.getBounds().contains([50.1167, 14.4178]);
```
