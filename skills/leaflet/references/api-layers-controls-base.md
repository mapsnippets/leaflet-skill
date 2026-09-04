# Leaflet API Reference — Layer Groups, Controls & Base Classes 🎛️

> Comprehensive technical reference for `L.LayerGroup`, `L.FeatureGroup`, `L.Control`, custom control subclassing, click propagation prevention, custom `L.GridLayer` canvas rendering, and Leaflet's OOP base classes (`L.Class`, `L.Evented`, `L.Handler`).

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Layer Containers: `LayerGroup` vs `FeatureGroup`

### A. `L.LayerGroup`

A simple container that groups multiple layers so they can be added or removed from the map as a single entity:

```javascript
import L from 'leaflet';

// Create layer group with initial members
const weatherRadarGroup = L.layerGroup([cloudLayer, rainLayer]);

// Add group to map
weatherRadarGroup.addTo(map);

// Add individual layer to group
weatherRadarGroup.addLayer(windBarbsLayer);

// Check membership and remove
if (weatherRadarGroup.hasLayer(cloudLayer)) {
  weatherRadarGroup.removeLayer(cloudLayer);
}

// Clear all layers at once
weatherRadarGroup.clearLayers();

// Invoke a method on all member layers simultaneously
weatherRadarGroup.invoke('setOpacity', 0.6);

// Iterate over each layer
weatherRadarGroup.eachLayer((layer) => {
  console.log('Active layer ID:', layer._leaflet_id);
});
```

### B. `L.FeatureGroup` (Interactive Batch Operations)

`L.FeatureGroup` extends `L.LayerGroup` with shared event forwarding, batch vector styling, popup/tooltip binding, and bounding box calculation:

```javascript
// Create feature group for fleet vehicles
const fleetGroup = L.featureGroup([truckA, truckB, warehouseZone]).addTo(map);

// 1. Batch vector styling
fleetGroup.setStyle({
  color: '#0084FF',
  weight: 3,
  opacity: 0.9
});

// 2. Batch popup & tooltip binding
fleetGroup.bindPopup((layer) => `Vehicle ID: ${layer.feature?.id || 'Unknown'}`);

// 3. Centralized event handling across all children
fleetGroup.on('click', (e) => {
  console.log('Clicked layer inside fleetGroup:', e.layer);
});

// 4. Zoom map to contain all elements in group
const bounds = fleetGroup.getBounds();
if (bounds.isValid()) {
  map.fitBounds(bounds, { padding: [40, 40] });
}

// 5. Visual layering priority
fleetGroup.bringToFront();
```

---

## 2. Leaflet Controls (`L.Control`)

All UI controls inherit from `L.Control` and attach to one of four corners:
* `'topleft'`
* `'topright'`
* `'bottomleft'`
* `'bottomright'`

### A. Dynamic Layer Switcher (`L.control.layers`)

Toggles between mutually exclusive base maps (radio buttons) and toggleable overlay layers (checkboxes):

```javascript
// 1. Define Base Maps (one active at a time)
const baseMaps = {
  "Streets": L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=KEY', { tileSize: 512, zoomOffset: -1 }),
  "Outdoor": L.tileLayer('https://api.maptiler.com/maps/outdoor-v4/{z}/{x}/{y}.png?key=KEY', { tileSize: 512, zoomOffset: -1 }),
  "Satellite": L.tileLayer('https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=KEY', { tileSize: 512, zoomOffset: -1 })
};

// 2. Define Overlays (independent checkboxes)
const overlays = {
  "Fleet Vehicles": fleetGroup,
  "Emergency Zones": emergencyZoneGroup
};

// 3. Instantiate Control
const layerControl = L.control.layers(baseMaps, overlays, {
  collapsed: true,         // Collapse into icon on mobile/desktop
  autoZIndex: true,        // Automatically assigns zIndex to layers
  hideSingleBase: false,   // Keeps radio list visible even with only 1 base layer
  position: 'topright'
}).addTo(map);

// Set default active base layer
baseMaps["Streets"].addTo(map);

// Dynamically add layers later at runtime
layerControl.addOverlay(weatherRadarGroup, "Live Weather");
```

### B. Built-in Utility Controls

```javascript
// 1. Scale Control (Metric & Imperial)
L.control.scale({
  position: 'bottomleft',
  maxWidth: 150,
  metric: true,
  imperial: false,
  updateWhenIdle: true // Update on moveend rather than during move
}).addTo(map);

// 2. Custom Attribution Control
map.attributionControl.setPrefix(
  '<a href="https://mapsnippets.org/" target="_blank">MapSnippets</a> &middot; <a href="https://leafletjs.com/">Leaflet</a>'
);
map.attributionControl.addAttribution('&copy; Custom Agency Data');
```

### C. Building Custom Map Controls (`L.Control.extend`)

Custom controls must implement `onAdd(map)` and optionally `onRemove(map)`.
**Critical Requirement:** You must use `L.DomEvent.disableClickPropagation` and `L.DomEvent.disableScrollPropagation` to prevent map clicks or zooms when interacting with control elements.

```javascript
// Create custom Legend / Action Control
const ActionButtonControl = L.Control.extend({
  options: {
    position: 'topright'
  },

  onAdd: function (map) {
    // Create root container element
    const container = L.DomUtil.create('div', 'leaflet-bar custom-action-control');
    container.style.backgroundColor = '#ffffff';
    container.style.padding = '8px 12px';
    container.style.borderRadius = '6px';
    container.style.boxShadow = '0 2px 8px rgba(0, 0, 0, 0.15)';
    container.style.cursor = 'pointer';
    container.style.display = 'flex';
    container.style.alignItems = 'center';
    container.style.gap = '8px';

    container.innerHTML = `
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#0084FF" stroke-width="2">
        <circle cx="12" cy="12" r="10"></circle>
        <polygon points="10 8 16 12 10 16 10 8"></polygon>
      </svg>
      <span style="font-size: 13px; font-weight: 600; color: #1e293b;">Reset View</span>
    `;

    // CRITICAL: Prevent map from panning/zooming when clicking control
    L.DomEvent.disableClickPropagation(container);
    L.DomEvent.disableScrollPropagation(container);

    // Attach click handler
    L.DomEvent.on(container, 'click', (e) => {
      L.DomEvent.stop(e);
      map.flyTo([50.0755, 14.4378], 13);
    });

    return container;
  },

  onRemove: function (map) {
    // Teardown DOM event listeners if needed
  }
});

// Factory method
L.control.actionButton = function (opts) {
  return new ActionButtonControl(opts);
};

// Add to map
L.control.actionButton({ position: 'topright' }).addTo(map);
```

---

## 3. Custom Grid Layers (`L.GridLayer`)

`L.GridLayer` is the base class for all tiled raster layers. Subclass it to draw custom HTML5 Canvas content directly per tile (e.g. debugging tile coordinates, heatmaps, or mathematical functions):

```javascript
const TileCoordinateDebugger = L.GridLayer.extend({
  createTile: function (coords, done) {
    // Create a 256x256 canvas for each tile
    const tile = document.createElement('canvas');
    const tileSize = this.getTileSize();
    tile.width = tileSize.x;
    tile.height = tileSize.y;

    const ctx = tile.getContext('2d');

    // Draw tile border
    ctx.strokeStyle = 'rgba(0, 132, 255, 0.6)';
    ctx.lineWidth = 1;
    ctx.strokeRect(0, 0, tileSize.x, tileSize.y);

    // Draw tile coordinate text: z / x / y
    ctx.fillStyle = '#0084FF';
    ctx.font = '14px monospace';
    ctx.fillText(`z: ${coords.z}`, 12, 24);
    ctx.fillText(`x: ${coords.x}`, 12, 44);
    ctx.fillText(`y: ${coords.y}`, 12, 64);

    // Notify Leaflet asynchronously when tile drawing is finished
    setTimeout(() => {
      done(null, tile);
    }, 0);

    return tile;
  }
});

// Add debugger layer on top
new TileCoordinateDebugger({ zIndex: 500, opacity: 0.8 }).addTo(map);
```

---

## 4. Leaflet OOP Architecture (`L.Class`, `L.Evented`, `L.Handler`)

### A. `L.Class` (Class Inheritance & Mixins)

Leaflet uses its internal `L.Class` engine to implement OOP patterns:

```javascript
const VehicleTracker = L.Class.extend({
  // Default options
  options: {
    updateInterval: 5000
  },

  // Constructor
  initialize: function (vehicleId, options) {
    L.setOptions(this, options);
    this.vehicleId = vehicleId;
  },

  start: function () {
    console.log('Tracking vehicle:', this.vehicleId, 'interval:', this.options.updateInterval);
  }
});

// Instantiate
const tracker = new VehicleTracker('TRK-901', { updateInterval: 2000 });
tracker.start();
```

### B. `L.Evented` (Event Dispatcher)

Mix in `L.Evented` to create custom observable modules:

```javascript
const CustomStore = L.Evented.extend({
  setData: function (data) {
    this.data = data;
    // Fire custom event
    this.fire('dataupdated', { data: this.data });
  }
});

const store = new CustomStore();
store.on('dataupdated', (e) => {
  console.log('Received updated store data:', e.data);
});
store.setData({ status: 'active', count: 128 });
```

### C. `L.Handler` (Toggleable Map Behaviors)

Extend `L.Handler` to add toggleable map interaction modes (e.g. measuring tape or draw mode):

```javascript
const RulerHandler = L.Handler.extend({
  addHooks: function () {
    // Activated via map.ruler.enable()
    L.DomEvent.on(this._map.getContainer(), 'click', this._onMapClick, this);
  },

  removeHooks: function () {
    // Deactivated via map.ruler.disable()
    L.DomEvent.off(this._map.getContainer(), 'click', this._onMapClick, this);
  },

  _onMapClick: function (e) {
    console.log('Ruler measure click at:', e.latlng);
  }
});

// Register handler on map class
L.Map.addInitHook('addHandler', 'ruler', RulerHandler);

// Usage:
// map.ruler.enable();
// map.ruler.disable();
```
