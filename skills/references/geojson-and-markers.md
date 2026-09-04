# Leaflet GeoJSON, Markers & Custom HTML Styling Reference

This guide covers complete GeoJSON data ingestion, dynamic thematic styling, custom HTML marker pins (`L.divIcon`), animated CSS pulse beacons, popups, and hover tooltips in Leaflet.

---

## 1. GeoJSON Layer Management (`L.geoJSON`)

`L.geoJSON` is Leaflet's primary vector parser and data layer. It parses GeoJSON `FeatureCollection`, `Feature`, and raw geometry objects.

### Full Constructor Options & Architecture
```javascript
import L from 'leaflet';

const geojsonLayer = L.geoJSON(data, {
  // 1. Convert Point Geometries to Markers or CircleMarkers
  pointToLayer: (feature, latlng) => {
    // Return either L.marker or L.circleMarker
    return L.circleMarker(latlng, {
      radius: 8,
      fillColor: '#0084FF',
      color: '#ffffff',
      weight: 2,
      opacity: 1,
      fillOpacity: 0.9
    });
  },

  // 2. Dynamic Thematic Styling for Polygons and LineStrings
  style: (feature) => {
    const props = feature.properties || {};
    
    // Check geometry type or attribute values
    if (feature.geometry.type === 'LineString') {
      return {
        color: '#00D2FF',
        weight: 4,
        opacity: 0.8,
        dashArray: '6, 6'
      };
    }

    // Polygon styling based on property tiers
    const status = props.status || 'default';
    const colorMap = {
      active: '#10b981',    // Emerald Green
      warning: '#f59e0b',   // Amber
      danger: '#ef4444'     // Red
    };

    return {
      fillColor: colorMap[status] || '#64748b',
      fillOpacity: 0.45,
      color: colorMap[status] || '#334155',
      weight: 2,
      opacity: 1
    };
  },

  // 3. Client-Side Filtering (Exclude features before rendering)
  filter: (feature) => {
    // Only display features with capacity > 50
    return feature.properties && feature.properties.capacity > 50;
  },

  // 4. Attach Interactions, Popups & Tooltips to Each Feature
  onEachFeature: (feature, layer) => {
    const props = feature.properties || {};

    // Bind rich HTML popup
    layer.bindPopup(`
      <div style="font-family: Inter, sans-serif; padding: 4px;">
        <h4 style="margin: 0 0 4px; font-size: 15px; color: #0f172a;">${props.name || 'Feature'}</h4>
        <p style="margin: 0; font-size: 13px; color: #64748b;">Capacity: <b>${props.capacity}</b></p>
      </div>
    `, {
      maxWidth: 300,
      className: 'custom-leaflet-popup'
    });

    // Bind sticky hover tooltip
    layer.bindTooltip(`${props.name}`, {
      sticky: true,           // Follow mouse pointer across polygon
      direction: 'top',
      offset: L.point(0, -10)
    });

    // Interactive Hover Highlighting
    layer.on({
      mouseover: (e) => {
        const target = e.target;
        if (target.setStyle) {
          target.setStyle({
            weight: 4,
            fillOpacity: 0.75,
            color: '#ffffff'
          });
          target.bringToFront();
        }
      },
      mouseout: (e) => {
        geojsonLayer.resetStyle(e.target);
      }
    });
  }
});

map.addLayer(geojsonLayer);

// Auto-fit viewport to data bounds with safety padding
map.fitBounds(geojsonLayer.getBounds(), { padding: [50, 50] });
```

---

## 2. Dynamic Data Updates (`addData` & `clearLayers`)

To update a live feed (e.g. tracking vehicles or sensor telemetry) without re-instantiating the map or recreating listeners:

```javascript
function updateGeoJsonData(newFeatureCollection) {
  geojsonLayer.clearLayers();
  geojsonLayer.addData(newFeatureCollection);
}
```

---

## 3. Custom HTML Markers with `L.divIcon`

Unlike traditional static images, `L.divIcon` allows rendering any HTML, SVG, and CSS animations inside Leaflet markers.

### A. Modern Glowing SVG Marker Pin
```javascript
const modernPinIcon = L.divIcon({
  className: 'custom-pin-wrapper',
  html: `
    <div style="
      position: relative;
      width: 32px;
      height: 32px;
      background: linear-gradient(135deg, #0084FF, #00D2FF);
      border-radius: 50% 50% 50% 0;
      transform: rotate(-45deg);
      border: 2px solid #ffffff;
      box-shadow: 0 4px 12px rgba(0, 132, 255, 0.4);
      display: flex;
      align-items: center;
      justify-content: center;
    ">
      <div style="
        width: 12px;
        height: 12px;
        background: #ffffff;
        border-radius: 50%;
        transform: rotate(45deg);
      "></div>
    </div>
  `,
  iconSize: [32, 32],
  iconAnchor: [16, 32],     // Bottom center tip of the rotated pin
  popupAnchor: [0, -34]     // Positions popup directly above pin tip
});

L.marker([50.0755, 14.4378], { icon: modernPinIcon }).addTo(map);
```

### B. Animated Radar Pulse Beacon
```javascript
const pulsingBeaconIcon = L.divIcon({
  className: 'pulsing-beacon-container',
  html: `
    <style>
      @keyframes leaflet-pulse {
        0% { transform: scale(0.9); box-shadow: 0 0 0 0 rgba(0, 132, 255, 0.7); }
        70% { transform: scale(1); box-shadow: 0 0 0 14px rgba(0, 132, 255, 0); }
        100% { transform: scale(0.9); box-shadow: 0 0 0 0 rgba(0, 132, 255, 0); }
      }
      .beacon-dot {
        width: 14px;
        height: 14px;
        background-color: #0084FF;
        border-radius: 50%;
        border: 2px solid #ffffff;
        animation: leaflet-pulse 1.8s infinite;
      }
    </style>
    <div class="beacon-dot"></div>
  `,
  iconSize: [14, 14],
  iconAnchor: [7, 7]
});

L.marker([50.088, 14.42], { icon: pulsingBeaconIcon }).addTo(map);
```

---

## 4. Standard Raster Marker Pins (`L.Icon`)

When using external PNG or WebP assets, define complete anchor metrics to avoid "floating" or shifting markers during zoom animations:

```javascript
const standardIcon = L.icon({
  iconUrl: 'assets/marker-icon.png',
  iconRetinaUrl: 'assets/marker-icon-2x.png',
  shadowUrl: 'assets/marker-shadow.png',
  iconSize: [25, 41],
  iconAnchor: [12, 41],     // Point corresponding to marker's location
  popupAnchor: [1, -34],    // Point from which popup should open relative to iconAnchor
  shadowSize: [41, 41],
  shadowAnchor: [12, 41]
});
```

---

## 5. CircleMarker vs Circle (Screen Pixels vs Real Meters)

| Metric | `L.circleMarker(latlng, { radius: 10 })` | `L.circle(latlng, { radius: 500 })` |
| :--- | :--- | :--- |
| **Radius Units** | **Screen Pixels** (`px`). | **Real-world Earth Meters** (`m`). |
| **Behavior on Zoom** | Stays the exact same size on screen regardless of zoom. | Expands/contracts visually as the user zooms in/out. |
| **Primary Use Case** | Point symbols, cluster targets, sensor pins. | Geofencing radii, broadcast zones, coverage areas. |
