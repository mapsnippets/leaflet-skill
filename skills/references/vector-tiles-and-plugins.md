# Vector Tiles in Leaflet Reference 🗺️⚡

> Technical guide to rendering, styling, and interacting with MapTiler Planet v4 vector tiles in Leaflet using `@maplibre/maplibre-gl-leaflet` and `Leaflet.VectorGrid`.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Using `@maplibre/maplibre-gl-leaflet` (Recommended)

The `@maplibre/maplibre-gl-leaflet` plugin bridges MapLibre's WebGL hardware-accelerated rendering engine with Leaflet's DOM layer stack. It delivers 60 FPS vector tile rendering with MapLibre style JSON specifications while preserving Leaflet's marker clustering, popups, and GeoJSON overlays.

### Installation

```bash
npm install leaflet maplibre-gl @maplibre/maplibre-gl-leaflet
```

### Production Implementation

```javascript
import L from 'leaflet';
import maplibregl from 'maplibre-gl';
import '@maplibre/maplibre-gl-leaflet';
import 'leaflet/dist/leaflet.css';

const map = L.map('map', {
  center: [50.0755, 14.4378], // [lat, lng]
  zoom: 13,
  zoomSnap: 0.5
});

// Add MapLibre GL Layer powered by MapTiler Planet v4 vector style
const glLayer = L.maplibreGL({
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
  interactive: true,
  pane: 'tilePane'
}).addTo(map);

// Add standard Leaflet overlays on top of WebGL basemap
const marker = L.marker([50.0755, 14.4378])
  .bindPopup('<b>Prague</b><br>Standard Leaflet marker on WebGL vector tiles.')
  .addTo(map);
```

### Dynamic Vector Style Switching & MapLibre Access

```javascript
// Access the internal MapLibre GL map instance to manipulate style layers directly
function switchVectorStyle(styleId, apiKey) {
  const newStyleUrl = `https://api.maptiler.com/maps/${styleId}/style.json?key=${apiKey}`;
  const maplibreInstance = glLayer.getMaplibreMap();
  maplibreInstance.setStyle(newStyleUrl);
}

// Example: Switch to Dataviz Dark
switchVectorStyle('dataviz-v4-dark', 'YOUR_MAPTILER_KEY');
```

---

## 2. Using `Leaflet.VectorGrid` (Protobuf PBF Tiles)

When WebGL is unavailable or when you want direct DOM/Canvas slicing of raw vector protobuf tiles:

### Installation

```bash
npm install leaflet.vectorgrid
```

### Implementation with MapTiler Planet v4

```javascript
import L from 'leaflet';
import 'leaflet.vectorgrid';

// Planet v4 Vector PBF Tile Endpoint
const vectorTileUrl = 'https://api.maptiler.com/tiles/v4/{z}/{x}/{y}.pbf?key=YOUR_MAPTILER_KEY';

const vectorGridLayer = L.vectorGrid.protobuf(vectorTileUrl, {
  rendererFactory: L.canvas.tile, // Fast HTML5 Canvas tile rendering
  maxNativeZoom: 14,
  maxZoom: 19,
  minZoom: 0,
  interactive: true,
  vectorTileLayerStyles: {
    // Style the 'water' vector sublayer
    water: {
      fill: true,
      weight: 1,
      fillColor: '#0084FF',
      color: '#0084FF',
      fillOpacity: 0.85,
      opacity: 1
    },
    // Dynamic styling for 'transportation' by road class
    transportation: function (properties, zoom) {
      const cls = properties.class;
      if (cls === 'motorway' || cls === 'primary') {
        return { weight: 3, color: '#f59e0b', opacity: 1 };
      }
      return { weight: 1, color: '#94a3b8', opacity: 0.7 };
    },
    // Style 'building' footprint geometries
    building: {
      fill: true,
      weight: 0.5,
      fillColor: '#cbd5e1',
      color: '#94a3b8',
      fillOpacity: 0.7
    }
  }
}).addTo(map);

// Interactive feature click inspection
vectorGridLayer.on('click', function (e) {
  const props = e.layer.properties;
  L.popup()
    .setLatLng(e.latlng)
    .setContent(`
      <div style="font-family: system-ui; font-size: 13px;">
        <strong style="color: #0084FF;">${props.name || 'Vector Feature'}</strong><br/>
        <span>Layer: ${e.layerName}</span><br/>
        <span>Class: ${props.class || 'N/A'}</span>
      </div>
    `)
    .openOn(map);
});
```
