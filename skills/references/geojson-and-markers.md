# Leaflet GeoJSON, Markers & Styling Reference

This guide covers GeoJSON ingestion, dynamic feature styling, custom HTML markers with `L.divIcon`, popups, and tooltips.

---

## 1. GeoJSON Layer Management (`L.geoJSON`)

`L.geoJSON` parses GeoJSON FeatureCollections, points, lines, and polygons.

### Complete Example with Custom Styling & Popups:
```javascript
import L from "leaflet";

const geojsonData = {
  type: "FeatureCollection",
  features: [
    {
      type: "Feature",
      geometry: { type: "Point", coordinates: [14.4378, 50.0755] }, // [lng, lat] in GeoJSON
      properties: { name: "Prague Office", category: "HQ", capacity: 120 }
    },
    {
      type: "Feature",
      geometry: {
        type: "Polygon",
        coordinates: [[[14.41, 50.07], [14.45, 50.07], [14.45, 50.09], [14.41, 50.09], [14.41, 50.07]]]
      },
      properties: { name: "Delivery Zone A", riskLevel: "high" }
    }
  ]
};

const geojsonLayer = L.geoJSON(geojsonData, {
  // 1. Convert Points to Custom CircleMarkers or Markers
  pointToLayer: (feature, latlng) => {
    return L.circleMarker(latlng, {
      radius: 8,
      fillColor: "#0084ff",
      color: "#ffffff",
      weight: 2,
      opacity: 1,
      fillOpacity: 0.85
    });
  },

  // 2. Dynamic Style Function for Polygons / Lines
  style: (feature) => {
    if (feature.geometry.type === "Polygon" || feature.geometry.type === "MultiPolygon") {
      const isHighRisk = feature.properties.riskLevel === "high";
      return {
        fillColor: isHighRisk ? "#e74c3c" : "#2ecc71",
        weight: 2,
        opacity: 1,
        color: isHighRisk ? "#c0392b" : "#27ae60",
        dashArray: "3",
        fillOpacity: 0.4
      };
    }
  },

  // 3. Attach Events and Popups to each Feature
  onEachFeature: (feature, layer) => {
    const props = feature.properties;
    layer.bindPopup(`
      <div class="custom-popup">
        <h4 style="margin:0 0 4px;">${props.name}</h4>
        <p style="margin:0;">Details: ${props.category || props.riskLevel}</p>
      </div>
    `);

    // Hover Highlight Effect
    layer.on({
      mouseover: (e) => {
        const l = e.target;
        if (l.setStyle) {
          l.setStyle({ weight: 4, fillOpacity: 0.7 });
        }
      },
      mouseout: (e) => {
        geojsonLayer.resetStyle(e.target);
      }
    });
  }
}).addTo(map);

// Fit map to data bounds
map.fitBounds(geojsonLayer.getBounds(), { padding: [40, 40] });
```

---

## 2. Custom HTML Markers (`L.divIcon`)

`L.divIcon` allows rendering any HTML, SVG, or CSS inside a Leaflet marker:

```javascript
const customHtmlIcon = L.divIcon({
  className: "custom-pin-container",
  html: `
    <div style="
      background-color: #0084ff;
      color: white;
      font-weight: 700;
      font-size: 12px;
      padding: 6px 10px;
      border-radius: 20px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.25);
      display: flex;
      align-items: center;
      gap: 6px;
      white-space: nowrap;
    ">
      <span>📍</span>
      <span>Store #42</span>
    </div>
  `,
  iconSize: [100, 32],
  iconAnchor: [50, 16] // Center of anchor
});

L.marker([50.0755, 14.4378], { icon: customHtmlIcon }).addTo(map);
```
