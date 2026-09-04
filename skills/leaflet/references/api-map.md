# Leaflet API Reference — Map, Methods & Architecture 🗺️

> Comprehensive technical reference for `L.Map`, constructor options, camera navigation, coordinate mathematics, viewport management, geolocation, and DOM pane architecture.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Map Creation & Initialization

Leaflet maps are instantiated using the factory method `L.map(id, options)` or constructor `new L.Map(id, options)`.

```javascript
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

// Initialize map with bounding constraints and high-DPI raster tiles
const map = L.map('map', {
  center: [50.0755, 14.4378], // [latitude, longitude]
  zoom: 13,
  minZoom: 2,
  maxZoom: 19,
  zoomSnap: 0.5,             // Half-step smooth zooming
  zoomDelta: 0.5,
  wheelPxPerZoomLevel: 120,  // Smooth mousewheel pacing
  preferCanvas: true         // Draw vector layers directly to Canvas 2D
});

// Add 512px retina tiles with zoom offset
L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY', {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  maxZoom: 19,
  crossOrigin: true,
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>'
}).addTo(map);
```

---

## 2. Constructor `MapOptions` Reference

### A. Map State Options

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `center` | `LatLngExpression` | `undefined` | Initial geographic center `[lat, lng]`. |
| `zoom` | `Number` | `undefined` | Initial zoom level integer or float. |
| `minZoom` | `Number` | `0` | Minimum allowed zoom level. |
| `maxZoom` | `Number` | `Infinity` | Maximum allowed zoom level. |
| `maxBounds` | `LatLngBoundsExpression`| `null` | Restricts viewport panning inside a bounding box. |
| `maxBoundsViscosity` | `Number` | `0.0` | Elasticity of map boundaries (`0.0` = hard bounce, `1.0` = completely solid clamp). |
| `crs` | `CRS` | `L.CRS.EPSG3857` | Coordinate reference system (`EPSG3857`, `EPSG4326`, or `Simple`). |
| `preferCanvas` | `Boolean` | `false` | When `true`, vector paths render to HTML5 Canvas instead of SVG. |
| `trackResize` | `Boolean` | `true` | Listens to window resize events and updates the map viewport. |
| `attributionControl` | `Boolean` | `true` | Automatically adds default attribution control. |
| `zoomControl` | `Boolean` | `true` | Automatically adds default zoom `+` / `-` buttons. |
| `closePopupOnClick` | `Boolean` | `true` | Closes active popup when user clicks the map surface. |

### B. Zoom & Navigation Physics Options

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `zoomSnap` | `Number` | `1` | Forces zoom level to snap to multiples of this value (`0` allows continuous fractional zoom). |
| `zoomDelta` | `Number` | `1` | Controls how many zoom levels change per `+` / `-` click or page up/down keypress. |
| `wheelPxPerZoomLevel`| `Number` | `60` | Mouse wheel scroll pixels required to advance one full zoom level. |
| `wheelDebounceTime` | `Number` | `40` | Wheel event debounce interval in milliseconds. |
| `scrollWheelZoom` | `Boolean \| 'center'` | `true` | Allows mouse wheel zoom. If `'center'`, zooms towards center instead of mouse cursor. |
| `doubleClickZoom` | `Boolean \| 'center'` | `true` | Allows double-click to zoom. |
| `boxZoom` | `Boolean` | `true` | Allows Shift + mouse drag to draw a rectangular zoom box. |
| `dragging` | `Boolean` | `true` | Allows panning the map via mouse drag or touch swiping. |
| `touchZoom` | `Boolean \| 'center'` | `true` | Allows two-finger pinch zooming on touchscreens. |
| `keyboard` | `Boolean` | `true` | Allows keyboard navigation with arrow keys and `+` / `-`. |
| `inertia` | `Boolean` | `true` | Simulates momentum panning with deceleration after drag release. |
| `inertiaDeceleration`| `Number` | `3000` | Rate of momentum deceleration in pixels/second². |
| `inertiaMaxSpeed` | `Number` | `Infinity` | Maximum panning speed cap in pixels/second. |

---

## 3. Viewport & Camera Control Methods

### A. Dynamic Camera Navigation

```javascript
// 1. Instant repositioning
map.setView([50.088, 14.420], 15);

// 2. Smooth animated panning
map.panTo([50.075, 14.437], {
  animate: true,
  duration: 1.0,
  easeLinearity: 0.25
});

// 3. Cinematic flight animation
map.flyTo([48.8566, 2.3522], 12, {
  animate: true,
  duration: 2.5 // Seconds
});

// 4. Zoom around a specific screen anchor or coordinate
map.setZoomAround([50.075, 14.437], 16, { animate: true });
```

### B. Fitting Bounding Boxes (`fitBounds`)

Automatically frames a set of coordinates or feature layer with edge padding:

```javascript
// Fit bounds to a GeoJSON feature layer with responsive padding
const group = L.featureGroup([marker1, marker2, polygon1]);
const bounds = group.getBounds();

if (bounds.isValid()) {
  map.fitBounds(bounds, {
    paddingTopLeft: [50, 20],     // Reserve room for top/left navigation header
    paddingBottomRight: [50, 320], // Reserve room for right-side detail drawer
    maxZoom: 16,                  // Prevent extreme zoom on single point
    animate: true,
    duration: 1.2
  });
}
```

---

## 4. Container Lifecycle & Resizing (`invalidateSize`)

When a Leaflet map is placed inside a hidden tab, accordion, Bootstrap modal, or resizable grid container, its DOM pixel dimensions change without window resize events. This causes missing tile columns or incorrect coordinate projections.

### Production Solution: Auto-Resize with `ResizeObserver`

```javascript
const mapElement = document.getElementById('map');
const map = L.map(mapElement).setView([50.0755, 14.4378], 13);

// Observe DOM element resizing and recalculate Leaflet viewport
const resizeObserver = new ResizeObserver(() => {
  map.invalidateSize({
    debounceMoveend: true, // Smooths viewport coordinate recalculation
    pan: false             // Prevents unwanted camera drift during resize
  });
});
resizeObserver.observe(mapElement);

// Cleanup on component teardown (React/Vue/Svelte)
function destroyMap() {
  resizeObserver.disconnect();
  map.remove(); // Removes all event listeners, tiles, and cleans DOM
}
```

---

## 5. Coordinate Mathematics & Transformations

Leaflet provides built-in projection utilities between Geographic Lat/Lng coordinates, Map Container pixel coordinates, and Layer pixel coordinates.

```javascript
// 1. Convert Geographic LatLng to Screen Container Pixel
const point = map.latLngToContainerPoint([50.0755, 14.4378]);
console.log('Pixel position in map container:', point.x, point.y);

// 2. Convert Screen Pixel back to Geographic LatLng
const latlng = map.containerPointToLatLng(L.point(250, 400));
console.log('Geographic coordinates:', latlng.lat, latlng.lng);

// 3. Convert Native MouseEvent to Geographic LatLng
mapElement.addEventListener('contextmenu', (e) => {
  e.preventDefault();
  const clickLatLng = map.mouseEventToLatLng(e);
  console.log('Right-clicked at:', clickLatLng.lat, clickLatLng.lng);
});

// 4. Projected Spherical Mercator meters (EPSG:3857)
const projectedMeters = map.options.crs.project(L.latLng(50.0755, 14.4378));
console.log('Meters X/Y:', projectedMeters.x, projectedMeters.y);
```

---

## 6. Geolocation API (`map.locate`)

Query user device location with built-in watch and fallback handling:

```javascript
// Start watching user location with high accuracy
map.locate({
  watch: true,                // Continually stream GPS updates
  setView: false,             // Do not automatically jump camera on every update
  maxZoom: 16,
  enableHighAccuracy: true,
  timeout: 10000
});

let userMarker = null;
let accuracyCircle = null;

map.on('locationfound', (e) => {
  const radius = e.accuracy / 2;

  if (!userMarker) {
    userMarker = L.circleMarker(e.latlng, {
      radius: 8,
      fillColor: '#0084FF',
      color: '#ffffff',
      weight: 2,
      fillOpacity: 1
    }).addTo(map);

    accuracyCircle = L.circle(e.latlng, {
      radius: radius,
      color: '#0084FF',
      fillColor: '#0084FF',
      fillOpacity: 0.15,
      weight: 1
    }).addTo(map);

    map.flyTo(e.latlng, 15);
  } else {
    userMarker.setLatLng(e.latlng);
    accuracyCircle.setLatLng(e.latlng);
    accuracyCircle.setRadius(radius);
  }
});

map.on('locationerror', (e) => {
  console.warn('Geolocation failed:', e.message);
});

// Stop watching GPS when navigating away
function stopTracking() {
  map.stopLocate();
}
```

---

## 7. Interaction Handler Toggling

All map interaction behaviors are modular handlers accessible directly on the `map` instance:

```javascript
// Temporarily lock map interactions during drawing or modal display
function disableInteractions(map) {
  map.dragging.disable();
  map.touchZoom.disable();
  map.doubleClickZoom.disable();
  map.scrollWheelZoom.disable();
  map.boxZoom.disable();
  map.keyboard.disable();
  if (map.tapHold) map.tapHold.disable();
}

function enableInteractions(map) {
  map.dragging.enable();
  map.touchZoom.enable();
  map.doubleClickZoom.enable();
  map.scrollWheelZoom.enable();
  map.boxZoom.enable();
  map.keyboard.enable();
  if (map.tapHold) map.tapHold.enable();
}
```

---

## 8. Built-in DOM Panes Architecture

Leaflet isolates layer rendering into dedicated DOM container elements (`panes`) to maintain a strict visual z-index hierarchy:

| Pane Name | Default `z-index` | Render Mode | Content Types |
| :--- | :--- | :--- | :--- |
| `mapPane` | `auto` | Container | Root container for all other map panes. |
| `tilePane` | `200` | HTML / Canvas | `L.TileLayer`, `L.GridLayer` basemap tiles. |
| `overlayPane` | `400` | SVG / Canvas | Vector geometries: `L.Polyline`, `L.Polygon`, `L.GeoJSON`. |
| `shadowPane` | `500` | Images | `L.Marker` drop shadows. |
| `markerPane` | `600` | DOM Elements | `L.Marker` icons and `L.DivIcon` elements. |
| `tooltipPane` | `650` | DOM Elements | `L.Tooltip` text badges. |
| `popupPane` | `700` | DOM Elements | `L.Popup` info cards. |

### Methods for Managing Panes (`#map-pane`)
* **`map.createPane(name, container?)`**: Creates a new map pane with the given name (if it doesn't already exist) and returns its `HTMLElement`. The pane is appended to `container` (or `mapPane` by default).
* **`map.getPane(pane)`**: Returns the pane `HTMLElement` by name (`String`) or element reference.
* **`map.getPanes()`**: Returns an object dictionary of all map panes keyed by their names (`{ mapPane, tilePane, overlayPane, shadowPane, markerPane, tooltipPane, popupPane, ... }`).

> For advanced custom pane ordering (e.g. rendering vector labels above polygons or underneath roads), refer to [panes-and-zindex.md](panes-and-zindex.md).
