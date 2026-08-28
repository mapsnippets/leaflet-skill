# Leaflet Custom Panes & Z-Index Management

Leaflet organizes layers into **Panes** (DOM containers). By default, polygons render below markers, and popups render above everything. Creating **Custom Panes** allows precise control over layer stacking (e.g. keeping labels, district borders, or interactive vector highlights strictly above background tiles and fills).

---

## 1. Default Leaflet Panes Hierarchy

| Pane Name | Default `z-index` | Purpose |
| :--- | :--- | :--- |
| `mapPane` | `auto` | Root container for all other panes |
| `tilePane` | `200` | Raster and base vector tiles |
| `overlayPane` | `400` | Vector paths, polylines, polygons, GeoJSON |
| `shadowPane` | `500` | Marker shadow images |
| `markerPane` | `600` | Markers (`L.marker`) |
| `tooltipPane` | `650` | Tooltips (`L.tooltip`) |
| `popupPane` | `700` | Popups (`L.popup`) |

---

## 2. Creating a Custom Pane

To place a GeoJSON polygon boundary above other overlay fills but below markers:

```javascript
// 1. Create a dedicated pane with custom zIndex
map.createPane("boundaryPane");
map.getPane("boundaryPane").style.zIndex = 450; // Sits between overlayPane (400) and markerPane (600)

// Optional: Pass pointer events through to underlying layers if needed
map.getPane("boundaryPane").style.pointerEvents = "none";

// 2. Assign layer to the custom pane
L.geoJSON(districtsGeoJSON, {
  pane: "boundaryPane",
  style: {
    color: "#ff0055",
    weight: 3,
    fillOpacity: 0
  }
}).addTo(map);
```

---

## 3. Top Labels Pane Pattern (Labels Above Vector Polygons)

When rendering custom GeoJSON polygons that might obscure base map labels, use a separate tile layer pane:

```javascript
// Create labels pane above overlayPane
map.createPane("labelsPane");
map.getPane("labelsPane").style.zIndex = 650;
map.getPane("labelsPane").style.pointerEvents = "none";

// Add base map tiles (no labels) to default tilePane (200)
L.tileLayer("https://api.maptiler.com/maps/base-v4/{z}/{x}/{y}.png?key=KEY", {
  tileSize: 512,
  zoomOffset: -1
}).addTo(map);

// Add dense data polygons to default overlayPane (400)
L.geoJSON(denseData).addTo(map);

// Add transparent labels layer to labelsPane (650)
L.tileLayer("https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=KEY", {
  pane: "labelsPane",
  tileSize: 512,
  zoomOffset: -1
}).addTo(map);
```
