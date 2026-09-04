# Leaflet Marker Clustering Reference (`leaflet.markercluster`) 📍✨

> Comprehensive technical guide to high-performance point clustering in Leaflet using `leaflet.markercluster`. Covers 50k+ point chunked ingestion, custom SVG cluster icons, spiderfy physics, event hooks, and dynamic data filtering.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Installation & Stylesheet Imports

```bash
npm install leaflet leaflet.markercluster
```

Required CSS imports:
```javascript
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import 'leaflet.markercluster/dist/MarkerCluster.css';
import 'leaflet.markercluster/dist/MarkerCluster.Default.css';
import 'leaflet.markercluster';
```

---

## 2. Cluster Group Options Reference

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `maxClusterRadius` | `Number \| Function` | `80` | Maximum pixel distance from the center point to cluster markers into one group. |
| `spiderfyOnMaxZoom` | `Boolean` | `true` | When clicking a cluster at the map's maximum zoom level, spreads overlapping markers into a spiral or circle. |
| `showCoverageOnHover` | `Boolean` | `true` | Displays a convex polygon polygon hull indicating the geographic footprint of points inside the cluster. |
| `zoomToBoundsOnClick` | `Boolean` | `true` | Zooms the map view to enclose all child markers when clicking a cluster bubble. |
| `singleMarkerMode` | `Boolean` | `false` | Renders even unclustered individual markers as mini-cluster badges. |
| `disableClusteringAtZoom`| `Number` | `null` | Zoom level at and above which clustering is completely turned off. |
| `removeOutsideVisibleBounds`| `Boolean` | `true` | Unloads markers that are outside the current viewport to maximize GPU/CPU performance. |
| `animate` | `Boolean` | `true` | Smooth zoom split and merge animations. |
| `animateAddingMarkers`| `Boolean` | `false` | Animates marker addition (keep `false` for large datasets). |
| `spiderfyDistanceMultiplier`| `Number`| `1` | Multiplier for the spread radius when spiderfying markers. |
| `chunkedLoading` | `Boolean` | `false` | Enables asynchronous, non-blocking chunked batch loading for datasets > 10,000 points. |
| `chunkInterval` | `Number` | `200` | Processing time slice per batch in milliseconds. |
| `chunkDelay` | `Number` | `50` | Sleep delay between batch loading chunks in milliseconds. |
| `chunkProgress` | `Function` | `null` | Callback function receiving `(processed, total, elapsedMs)`. |

---

## 3. High-Performance Bulk Data Ingestion (50,000+ Points)

**Do NOT call `markers.addLayer()` inside a `forEach` loop!** Adding markers individually forces Leaflet to recalculate the cluster spatial index on every single point, freezing the browser UI. Always use `markers.addLayers(arrayOfMarkers)` with `chunkedLoading: true`:

```javascript
// Initialize high-performance cluster group
const markers = L.markerClusterGroup({
  maxClusterRadius: 60,
  disableClusteringAtZoom: 17,
  chunkedLoading: true,
  chunkInterval: 100,
  chunkDelay: 20,
  chunkProgress: (processed, total, elapsed) => {
    const pct = Math.round((processed / total) * 100);
    console.log(`Clustering progress: ${pct}% (${processed}/${total} in ${elapsed}ms)`);
  },
  removeOutsideVisibleBounds: true
});

// Prepare 20,000 markers in memory
const markerList = [];
for (let i = 0; i < 20000; i++) {
  const lat = 50.0 + (Math.random() - 0.5) * 0.4;
  const lng = 14.4 + (Math.random() - 0.5) * 0.4;
  
  const m = L.marker([lat, lng], {
    title: `Point #${i}`
  }).bindPopup(`<b>Facility #${i}</b><br>Coords: ${lat.toFixed(4)}, ${lng.toFixed(4)}`);
  
  markerList.push(m);
}

// Bulk load asynchronously without blocking UI thread
markers.addLayers(markerList);
map.addLayer(markers);
```

---

## 4. Custom Cluster Icon Creation (`iconCreateFunction`)

Replace default generic green/yellow/red circles with branded, glowing UI bubbles:

```javascript
const customClusterGroup = L.markerClusterGroup({
  showCoverageOnHover: false,
  iconCreateFunction: function (cluster) {
    const count = cluster.getChildCount();
    
    // Tiered sizing and color hierarchy
    let bg = '#0084FF'; // MapTiler Electric Blue (< 20)
    let size = 36;
    let glow = 'rgba(0, 132, 255, 0.4)';

    if (count > 250) {
      bg = '#ef4444'; // Red (> 250)
      size = 52;
      glow = 'rgba(239, 68, 68, 0.5)';
    } else if (count > 50) {
      bg = '#f59e0b'; // Amber (> 50)
      size = 44;
      glow = 'rgba(245, 158, 11, 0.4)';
    }

    return L.divIcon({
      html: `
        <div style="
          width: ${size}px;
          height: ${size}px;
          background: ${bg};
          border: 3px solid #ffffff;
          border-radius: 50%;
          box-shadow: 0 0 14px ${glow}, 0 2px 6px rgba(0,0,0,0.25);
          display: flex;
          align-items: center;
          justify-content: center;
          color: #ffffff;
          font-family: system-ui, -apple-system, sans-serif;
          font-weight: 700;
          font-size: ${count > 999 ? '11px' : '13px'};
          cursor: pointer;
          transition: transform 0.15s ease;
        " onmouseover="this.style.transform='scale(1.1)'" onmouseout="this.style.transform='scale(1)'">
          ${count.toLocaleString()}
        </div>
      `,
      className: 'maptiler-cluster-badge',
      iconSize: L.point(size, size)
    });
  }
});
```

---

## 5. Cluster Events & Interactive Inspection

Listen to interactions directly on the cluster group:

```javascript
// 1. Zoom to bounds with custom easing and margin on cluster click
markers.on('clusterclick', (e) => {
  const cluster = e.layer;
  const childCount = cluster.getChildCount();
  console.log(`Cluster clicked with ${childCount} children`);

  // Inspect all child markers inside the clicked cluster
  const childMarkers = cluster.getAllChildMarkers();
  console.log('Sample child:', childMarkers[0].getLatLng());

  // Default behavior zooms into bounds, but you can override:
  // cluster.zoomToBounds({ padding: [50, 50] });
});

// 2. Spiderfy events (when markers fan out at max zoom)
markers.on('spiderfied', (e) => {
  console.log('Cluster spiderfied with markers:', e.markers.length);
});

markers.on('unspiderfied', (e) => {
  console.log('Cluster returned to collapsed state');
});

// 3. Cluster Hover Hull
markers.on('clustermouseover', (e) => {
  const count = e.layer.getChildCount();
  // Show custom HUD notification
  document.getElementById('status-hud').textContent = `Cluster density: ${count} entities`;
});

markers.on('clustermouseout', (e) => {
  document.getElementById('status-hud').textContent = 'Ready';
});
```

---

## 6. Dynamic Filtering & Updating

```javascript
function applyCategoryFilter(selectedCategory) {
  // Clear currently rendered markers
  markers.clearLayers();

  // Filter master dataset
  const filtered = allRawMarkers.filter(m => {
    if (selectedCategory === 'ALL') return true;
    return m.category === selectedCategory;
  }).map(m => m.layerInstance);

  // Bulk re-insert matching markers
  markers.addLayers(filtered);
}
```
