# Marker Clustering with `leaflet.markercluster`

This guide explains how to cluster thousands of points cleanly using `leaflet.markercluster`.

---

## 1. Setup & Installation

```bash
npm install leaflet.markercluster
```

Include CSS in your application:
```javascript
import "leaflet/dist/leaflet.css";
import "leaflet.markercluster/dist/MarkerCluster.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import "leaflet.markercluster";
```

---

## 2. Basic Marker Cluster Group

```javascript
import L from "leaflet";
import "leaflet.markercluster";

// Initialize Cluster Group
const markers = L.markerClusterGroup({
  maxClusterRadius: 50,
  spiderfyOnMaxZoom: true,
  showCoverageOnHover: false,
  zoomToBoundsOnClick: true,
  disableClusteringAtZoom: 17
});

// Add 1,000 sample points
for (let i = 0; i < 1000; i++) {
  const lat = 50.0 + (Math.random() - 0.5) * 0.2;
  const lng = 14.4 + (Math.random() - 0.5) * 0.2;
  
  const marker = L.marker([lat, lng]).bindPopup(`Point #${i + 1}`);
  markers.addLayer(marker);
}

// Add group to map
map.addLayer(markers);
```

---

## 3. Custom Cluster Icon Creation (`iconCreateFunction`)

Create custom styled cluster bubbles with color coding based on count:

```javascript
const customClusterGroup = L.markerClusterGroup({
  iconCreateFunction: (cluster) => {
    const count = cluster.getChildCount();
    let bg = "#0084ff"; // Blue (< 10)
    let size = 36;

    if (count > 50) {
      bg = "#e74c3c"; // Red (> 50)
      size = 48;
    } else if (count > 20) {
      bg = "#f39c12"; // Orange (> 20)
      size = 42;
    }

    return L.divIcon({
      html: `
        <div style="
          background-color: ${bg};
          width: ${size}px;
          height: ${size}px;
          border-radius: 50%;
          border: 3px solid rgba(255, 255, 255, 0.8);
          box-shadow: 0 4px 10px rgba(0,0,0,0.3);
          color: #ffffff;
          display: flex;
          align-items: center;
          justify-content: center;
          font-weight: bold;
          font-family: sans-serif;
          font-size: 13px;
        ">
          ${count}
        </div>
      `,
      className: "custom-cluster-icon",
      iconSize: L.point(size, size)
    });
  }
});
```

---

## 4. Bulk Data Ingestion (`addLayers`)

When loading large datasets (10,000+ points), use `addLayers()` with an array instead of looping `addLayer()`:

```javascript
const markerList = points.map(p => L.marker([p.lat, p.lng]).bindPopup(p.name));
markers.addLayers(markerList);
```
