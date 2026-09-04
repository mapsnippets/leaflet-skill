# Leaflet API Reference — UI Layers & Overlay Elements 📌

> Complete technical reference for Leaflet UI elements: `L.Marker`, `L.Popup`, `L.Tooltip`, `L.Icon`, and `L.DivIcon`, including interactive drag-and-drop, custom SVG radar beacons, event delegation, and modern bundler asset resolution.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. `L.Marker` (Point Anchors)

### Creation & Basic Setup

```javascript
import L from 'leaflet';

const marker = L.marker([50.0755, 14.4378], {
  title: 'Prague Operations Center',
  alt: 'Location marker for Prague HQ',
  opacity: 1.0,
  riseOnHover: true,
  riseOffset: 250,
  draggable: true,
  autoPan: true,
  autoPanPadding: [50, 50],
  autoPanSpeed: 10
}).addTo(map);
```

### Constructor Options Reference

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `icon` | `Icon \| DivIcon` | `new L.Icon.Default()` | Icon instance used to render the marker on the map. |
| `keyboard` | `Boolean` | `true` | Makes the marker accessible and focusable via keyboard navigation. |
| `title` | `String` | `''` | Native browser tooltip text on hover (`title` attribute). |
| `alt` | `String` | `'Marker'` | Accessibility screen-reader text (`alt` attribute). |
| `zIndexOffset` | `Number` | `0` | Additional z-index offset for priority stacking. |
| `opacity` | `Number` | `1.0` | Visual opacity (`0.0` to `1.0`). |
| `riseOnHover` | `Boolean` | `false` | Dynamically increases marker z-index when hovered. |
| `riseOffset` | `Number` | `250` | Z-index increment when `riseOnHover` triggers. |
| `pane` | `String` | `'markerPane'` | Target map pane DOM element. |
| `shadowPane` | `String` | `'shadowPane'` | Target pane for the icon drop shadow. |
| `draggable` | `Boolean` | `false` | Enables mouse and touch dragging. |
| `autoPan` | `Boolean` | `false` | Smoothly pans map when dragging marker near container edge. |
| `autoPanPadding` | `Point` | `[50, 50]` | Inset distance in pixels from viewport edge for autoPan. |

### Marker Dragging & Coordinate Capture

```javascript
marker.on('dragstart', (e) => {
  marker.setOpacity(0.7);
});

marker.on('drag', (e) => {
  const currentPos = marker.getLatLng();
  // Update live coordinate readouts in real-time
  document.getElementById('coord-readout').textContent = 
    `${currentPos.lat.toFixed(5)}, ${currentPos.lng.toFixed(5)}`;
});

marker.on('dragend', (e) => {
  marker.setOpacity(1.0);
  const newPos = e.target.getLatLng();
  console.log('Marker moved to:', newPos.lat, newPos.lng);
  
  // Persist updated coordinates or reverse geocode
  updateEntityLocation(newPos.lat, newPos.lng);
});
```

---

## 2. `L.Popup` (Interactive Information Cards)

### Constructor Options Reference

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `maxWidth` | `Number` | `300` | Maximum width of the popup content container in pixels. |
| `minWidth` | `Number` | `50` | Minimum width in pixels. |
| `maxHeight` | `Number` | `null` | Maximum height in pixels before vertical scrolling is enabled. |
| `autoPan` | `Boolean` | `true` | Automatically pans map so the popup is completely visible. |
| `autoPanPaddingTopLeft`| `Point` | `null` | Margin padding for top/left viewport edges during autoPan. |
| `autoPanPaddingBottomRight`| `Point`| `null` | Margin padding for bottom/right viewport edges during autoPan. |
| `autoPanPadding` | `Point` | `[5, 5]` | Global autoPan margin padding. |
| `keepInView` | `Boolean` | `false` | Pans map to keep popup in view if user attempts to pan away. |
| `closeButton` | `Boolean` | `true` | Displays the standard top-right close `×` button. |
| `autoClose` | `Boolean` | `true` | Closes active popup when another popup is opened. |
| `closeOnEscapeKey`| `Boolean` | `true` | Closes the popup on pressing Escape. |
| `closeOnClick` | `Boolean` | `null` | Overrides map `closePopupOnClick` option. |
| `className` | `String` | `''` | Custom CSS class attached to the root popup container element. |
| `offset` | `Point` | `[0, 7]` | Pixel offset distance from anchor coordinate. |

### Production HTML Popup with Event Delegation

```javascript
// Bind rich responsive card to marker
marker.bindPopup((layer) => {
  const coords = layer.getLatLng();
  
  // Return custom styled HTML card
  return `
    <div class="custom-card" style="font-family: system-ui, sans-serif; min-width: 220px;">
      <h4 style="margin: 0 0 6px; color: #0084FF; font-size: 15px;">Facility Overview</h4>
      <p style="margin: 0 0 10px; font-size: 13px; color: #475569;">
        Lat: ${coords.lat.toFixed(4)}<br/>
        Lng: ${coords.lng.toFixed(4)}
      </p>
      <div style="display: flex; gap: 8px;">
        <button id="btn-inspect" style="flex: 1; padding: 6px 10px; background: #0084FF; color: white; border: none; border-radius: 4px; cursor: pointer; font-size: 12px; font-weight: 600;">Inspect</button>
        <button id="btn-delete" style="padding: 6px 10px; background: #ef4444; color: white; border: none; border-radius: 4px; cursor: pointer; font-size: 12px;">Delete</button>
      </div>
    </div>
  `;
}, {
  maxWidth: 280,
  className: 'card-popup-container',
  offset: [0, -28]
});

// Safely attach event listeners once popup DOM is rendered
marker.on('popupopen', (e) => {
  const container = e.popup.getElement();
  
  const inspectBtn = container.querySelector('#btn-inspect');
  if (inspectBtn) {
    inspectBtn.onclick = () => {
      alert('Inspecting entity at: ' + marker.getLatLng());
    };
  }
  
  const deleteBtn = container.querySelector('#btn-delete');
  if (deleteBtn) {
    deleteBtn.onclick = () => {
      map.removeLayer(marker);
    };
  }
});
```

### Standalone Programmatic Popups

```javascript
// Open popup directly at coordinates without attaching to a marker
const standalonePopup = L.popup({
  autoClose: false,
  closeOnClick: false
})
  .setLatLng([50.0755, 14.4378])
  .setContent('<p>Independent notification pinned to map coordinates.</p>')
  .openOn(map);
```

---

## 3. `L.Tooltip` (Hover Labels & Badges)

Tooltips provide lightweight textual annotations that appear on hover or remain permanently visible.

### Constructor Options Reference

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `pane` | `String` | `'tooltipPane'` | Target map pane. |
| `offset` | `Point` | `[0, 0]` | Pixel offset from anchor. |
| `direction` | `String` | `'auto'` | Opening direction: `'right'`, `'left'`, `'top'`, `'bottom'`, `'center'`, or `'auto'`. |
| `permanent` | `Boolean` | `false` | Whether tooltip stays constantly visible regardless of hover. |
| `sticky` | `Boolean` | `false` | Follows mouse cursor across vector paths instead of fixing to anchor. |
| `interactive` | `Boolean` | `false` | Enables click/hover DOM events on the tooltip element itself. |
| `opacity` | `Number` | `0.9` | Opacity value (`0.0` to `1.0`). |

### Tooltip Implementations

```javascript
// 1. Permanent numeric badge above marker
marker.bindTooltip('Sensor #42', {
  permanent: true,
  direction: 'top',
  offset: [0, -32],
  className: 'sensor-badge'
});

// 2. Dynamic sticky hover tooltip on a Polygon/Polyline
polygon.bindTooltip((layer) => `Parcel Area: <b>${layer.feature.properties.area_sqm} m²</b>`, {
  sticky: true,     // Follows mouse cursor across polygon interior
  direction: 'auto',
  opacity: 0.95
});
```

---

## 4. Custom Icons: `L.Icon` vs `L.DivIcon`

### A. Classic Image Icons (`L.Icon`)

```javascript
const customPinIcon = L.icon({
  iconUrl: '/assets/marker-blue.png',
  iconRetinaUrl: '/assets/marker-blue@2x.png',
  iconSize: [32, 42],       // Size of icon image in pixels
  iconAnchor: [16, 42],     // Point of the icon which corresponds to marker's location (tip)
  popupAnchor: [0, -40],    // Point from which popup should open relative to iconAnchor
  tooltipAnchor: [0, -42],  // Point from which tooltip should open
  shadowUrl: '/assets/marker-shadow.png',
  shadowSize: [41, 41],
  shadowAnchor: [14, 41]
});

const pinMarker = L.marker([50.0755, 14.4378], { icon: customPinIcon }).addTo(map);
```

### B. Bundler Asset Resolution (`L.Icon.Default`)

Modern build tools (Vite, Webpack 5, Next.js, Rollup) do not automatically resolve Leaflet's relative CSS image URLs, causing broken 404 image icons. Fix this globally:

```javascript
// In your application entry file (main.ts / app.jsx):
import L from 'leaflet';
import iconUrl from 'leaflet/dist/images/marker-icon.png';
import iconRetinaUrl from 'leaflet/dist/images/marker-icon-2x.png';
import shadowUrl from 'leaflet/dist/images/marker-shadow.png';

delete L.Icon.Default.prototype._getIconUrl;

L.Icon.Default.mergeOptions({
  iconUrl,
  iconRetinaUrl,
  shadowUrl
});
```

### C. Animated Radar Beacon (`L.DivIcon`)

Pure CSS and SVG pulse animations without external raster image files:

```javascript
const radarBeaconIcon = L.divIcon({
  className: 'radar-beacon-container',
  html: `
    <div style="position: relative; width: 24px; height: 24px;">
      <div style="position: absolute; width: 24px; height: 24px; background: rgba(0, 132, 255, 0.4); border-radius: 50%; animation: pulse-ring 1.8s cubic-bezier(0.215, 0.61, 0.355, 1) infinite;"></div>
      <div style="position: absolute; top: 6px; left: 6px; width: 12px; height: 12px; background: #0084FF; border: 2px solid #ffffff; border-radius: 50%; box-shadow: 0 0 8px rgba(0, 132, 255, 0.8);"></div>
    </div>
    <style>
      @keyframes pulse-ring {
        0% { transform: scale(0.6); opacity: 0.9; }
        80%, 100% { transform: scale(2.2); opacity: 0; }
      }
    </style>
  `,
  iconSize: [24, 24],
  iconAnchor: [12, 12]
});

const activeBeacon = L.marker([50.088, 14.420], { icon: radarBeaconIcon }).addTo(map);
```
