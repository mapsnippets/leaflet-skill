# Vector Tiles in Leaflet Reference

This guide details how to render and interact with vector tiles in Leaflet using open-source plugins.

---

## 1. Using `@maplibre/maplibre-gl-leaflet` (Recommended)

The `@maplibre/maplibre-gl-leaflet` plugin bridges MapLibre's WebGL rendering engine with Leaflet's coordinate and layer management. It renders crisp, smooth vector tile styles with native MapLibre performance while allowing standard Leaflet markers, popups, and GeoJSON on top.

### Installation:
```bash
npm install leaflet maplibre-gl @maplibre/maplibre-gl-leaflet
```

### Implementation:
```javascript
import L from "leaflet";
import maplibregl from "maplibre-gl";
import "@maplibre/maplibre-gl-leaflet";
import "leaflet/dist/leaflet.css";

const map = L.map("map").setView([50.0755, 14.4378], 13); // [lat, lng]

const glLayer = L.maplibreGL({
  style: "https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY",
  interactive: true
}).addTo(map);
```

### Switching Vector Styles Dynamically:
```javascript
function switchVectorStyle(styleId, apiKey) {
  const newStyleUrl = `https://api.maptiler.com/maps/${styleId}/style.json?key=${apiKey}`;
  const maplibreMap = glLayer.getMaplibreMap();
  maplibreMap.setStyle(newStyleUrl);
}

// Example: Switch to Dark or Satellite
switchVectorStyle("streets-v4-dark", "YOUR_API_KEY");
```

---

## 2. Using `Leaflet.VectorGrid` (Protobuf PBF Tiles)

For scenarios requiring direct vector tile slicing without WebGL:

### Installation:
```bash
npm install leaflet.vectorgrid
```

### Implementation:
```javascript
import L from "leaflet";
import "leaflet.vectorgrid";

const vectorTileUrl = "https://api.maptiler.com/tiles/v3/{z}/{x}/{y}.pbf?key=YOUR_API_KEY";

const vectorGridLayer = L.vectorGrid.protobuf(vectorTileUrl, {
  vectorTileLayerStyles: {
    water: {
      fill: true,
      weight: 1,
      fillColor: "#a0c8f0",
      color: "#a0c8f0",
      fillOpacity: 1,
      opacity: 1
    },
    transportation: function(properties, zoom) {
      const cls = properties.class;
      if (cls === "motorway" || cls === "primary") {
        return { weight: 3, color: "#f39c12", opacity: 1 };
      }
      return { weight: 1, color: "#bdc3c7", opacity: 0.8 };
    },
    building: {
      fill: true,
      weight: 0.5,
      fillColor: "#e0e0e0",
      color: "#cccccc",
      fillOpacity: 0.8
    }
  },
  interactive: true
}).addTo(map);

vectorGridLayer.on("click", function(e) {
  const props = e.layer.properties;
  L.popup()
    .setLatLng(e.latlng)
    .setContent(`<b>${props.name || "Feature"}</b><br>Class: ${props.class || "N/A"}`)
    .openOn(map);
});
```
