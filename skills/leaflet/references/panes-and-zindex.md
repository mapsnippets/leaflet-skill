# Leaflet Custom Panes & Z-Index Management

Leaflet organizes DOM elements into distinct containers called **Panes**. By default, raster tiles sit at the bottom (`z-index: 200`), vector paths in the middle (`z-index: 400`), markers above paths (`z-index: 600`), and popups on top (`z-index: 700`).

Custom panes give developers granular control over the stacking order of layers, resolving critical cartographic problems such as:
1. **Keeping street/place labels visible above dense thematic polygon fills**.
2. **Placing boundary borders above heatmaps without intercepting mouse clicks**.
3. **Isolating separate SVG/Canvas renderers with distinct opacity and blending modes**.

---

## 1. Default Leaflet Panes Architecture

```text
mapPane (root container, auto)
├── tilePane (z-index: 200)       --> Base raster & tile layers
├── overlayPane (z-index: 400)    --> Polylines, Polygons, GeoJSON vector layers
├── shadowPane (z-index: 500)     --> Marker shadow drop images
├── markerPane (z-index: 600)     --> Standard markers (L.marker)
├── tooltipPane (z-index: 650)    --> Hover tooltips
└── popupPane (z-index: 700)      --> Click popups
```

---

## 2. Creating & Configuring Custom Panes

To create a pane, call `map.createPane(name, container?)` and adjust its CSS styles via `map.getPane(name)`:

```javascript
import L from 'leaflet';

// 1. Create a custom pane
map.createPane('highlightPane');

// 2. Set z-index (e.g. 450 sits between overlayPane 400 and markerPane 600)
const paneElement = map.getPane('highlightPane');
paneElement.style.zIndex = 450;

// 3. Pointer events control:
// 'none' allows clicks to pass through transparent areas to layers below
paneElement.style.pointerEvents = 'none';

// 4. Assign any layer to this pane via the 'pane' option:
const highlightLayer = L.geoJSON(districtBoundaries, {
  pane: 'highlightPane',
  style: {
    color: '#00D2FF',
    weight: 3,
    fillOpacity: 0
  }
}).addTo(map);
```

---

## 3. The "Sandwich" Architecture: Labels Above Custom Vector Overlays

When rendering opaque or semi-transparent polygon choropleths, base map labels are often washed out or obscured. The industry standard solution is the **Sandwich Architecture**:
1. Base tiles (no labels) in `tilePane` (200).
2. Data polygons in `overlayPane` (400).
3. Transparent labels overlay in a custom `labelsPane` (650).

```javascript
// Step 1: Create labels pane positioned above vector overlays
map.createPane('topLabelsPane');
map.getPane('topLabelsPane').style.zIndex = 650;
map.getPane('topLabelsPane').style.pointerEvents = 'none'; // Critical: let clicks reach data

// Step 2: Add base map tiles (without labels)
L.tileLayer('https://api.maptiler.com/maps/base-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY', {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '&copy; MapTiler'
}).addTo(map);

// Step 3: Add thematic data polygons (sits in default overlayPane: 400)
L.geoJSON(choroplethData, {
  style: (feature) => ({
    fillColor: feature.properties.color,
    fillOpacity: 0.7,
    weight: 1,
    color: '#ffffff'
  })
}).addTo(map);

// Step 4: Add top labels layer to the custom pane
L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY', {
  pane: 'topLabelsPane',
  tileSize: 512,
  zoomOffset: -1
}).addTo(map);
```

---

## 4. Re-ordering Layers Within the Same Pane

For vector layers sharing the same pane, control stacking order programmatically:

```javascript
// Bring a hovered feature or layer to the front of its pane
vectorLayer.bringToFront();

// Send back behind other sibling layers
vectorLayer.bringToBack();
```

For markers within `markerPane`, use `zIndexOffset`:
```javascript
// Ensure high-priority marker always renders on top of other pins
L.marker([50.0755, 14.4378], {
  zIndexOffset: 1000 // Added to the marker's latitude-based z-index
}).addTo(map);
```

---

## 5. Custom Pane with Dedicated SVG / Canvas Renderer

When drawing thousands of geometries, assign a dedicated Canvas renderer to a custom pane:

```javascript
// Create high-speed Canvas renderer bound to custom pane
const customCanvasRenderer = L.canvas({
  pane: 'highlightPane',
  padding: 0.5
});

L.circleMarker([50.0755, 14.4378], {
  renderer: customCanvasRenderer,
  radius: 12,
  color: '#0084FF'
}).addTo(map);
```
