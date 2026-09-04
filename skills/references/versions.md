# Leaflet & Ecosystem Versions 📦⚡

This guide lists the current production versions of Leaflet, verified official companion plugins, vector tile renderers, clustering engines, and MapTiler Planet v4 style endpoints. Use these versions when creating HTML scripts, `package.json` dependencies, or CDN links.

---

## 1. Core Library & Companion Plugins Matrix

| Library / Package | Current Version | Ingestion / Type | Primary Purpose |
| :--- | :--- | :--- | :--- |
| **leaflet** | `1.9.4` | UMD / ESM / NPM | Core lightweight 2D mapping library (Stable) |
| **leaflet.markercluster** | `1.5.3` | UMD Plugin + CSS | High-performance point density clustering |
| **esri-leaflet** | `3.0.12` | UMD Plugin | ArcGIS feature, tile, and geocoding services |
| **leaflet-draw** | `1.0.4` | UMD Plugin + CSS | Geometry drawing, editing, and measurement toolbar |
| **@geoman-io/leaflet-geoman-free** | `2.18.3` | UMD / ESM + CSS | Modern vector editing, snapping, and rotation tools |
| **leaflet.vectorgrid** | `1.3.0` | UMD Plugin | Protobuf (MVT) vector tile rendering |
| **proj4leaflet** | `1.0.2` | UMD Plugin | Non-Mercator coordinate projections with Proj4js |
| **leaflet-geosearch** | `4.0.0` | UMD / ESM + CSS | Address search and autocomplete control |
| **leaflet-side-by-side** | `2.2.0` | UMD Plugin | Interactive split-screen layer swipe comparison |
| **leaflet.heat** | `0.2.0` | UMD Plugin | Dynamic heatmap density surface renderer |

---

## 2. Official CDN Endpoints

### Leaflet v1.9.4
* **JavaScript:** `https://unpkg.com/leaflet@1.9.4/dist/leaflet.js`
* **CSS Stylesheet:** `https://unpkg.com/leaflet@1.9.4/dist/leaflet.css`
* **Integrity Hash (JS):** `sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=`
* **Integrity Hash (CSS):** `sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=`

### Companion Plugins (CDN)
* **Leaflet.markercluster v1.5.3:**
  - JS: `https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js`
  - CSS: `https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css`
  - Default Theme CSS: `https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css`
* **Leaflet-draw v1.0.4:**
  - JS: `https://unpkg.com/leaflet-draw@1.0.4/dist/leaflet.draw.js`
  - CSS: `https://unpkg.com/leaflet-draw@1.0.4/dist/leaflet.draw.css`
* **Leaflet-Geoman v2.18.3:**
  - JS: `https://unpkg.com/@geoman-io/leaflet-geoman-free@2.18.3/dist/leaflet-geoman.min.js`
  - CSS: `https://unpkg.com/@geoman-io/leaflet-geoman-free@2.18.3/dist/leaflet-geoman.css`
* **Esri Leaflet v3.0.12:** `https://unpkg.com/esri-leaflet@3.0.12/dist/esri-leaflet.js`
* **Proj4Leaflet v1.0.2:** `https://unpkg.com/proj4leaflet@1.0.2/src/proj4leaflet.js`
* **Leaflet.heat v0.2.0:** `https://unpkg.com/leaflet.heat@0.2.0/dist/leaflet-heat.js`

---

## 3. MapTiler Planet v4 Basemap Registry

### Standard Raster XYZ Configuration:
MapTiler raster tiles are delivered at high-resolution 512x512 pixels. In Leaflet, always configure `tileSize: 512` and `zoomOffset: -1` for crisp, correctly scaled tiles:

```javascript
L.tileLayer('https://api.maptiler.com/maps/<style-id>/{z}/{x}/{y}.png?key=YOUR_API_KEY', {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  maxZoom: 20,
  attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> &copy; <a href="https://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>',
  crossOrigin: true
}).addTo(map);
```

| Style ID (`<style-id>`) | Variants | Category | Best Use Case |
| :--- | :--- | :--- | :--- |
| **`streets-v4`** | `streets-v4-dark`, `streets-v4-pastel` | Raster 512px | Default general-purpose street map |
| **`outdoor-v4`** | `outdoor-v4-dark` | Raster 512px | Topographic hiking map with contour lines & hillshading |
| **`satellite-v4`** | `satellite-v4-dark` | Raster Orthophoto | High-resolution satellite imagery |
| **`hybrid-v4`** | `hybrid-v4-dark` | Hybrid | Satellite imagery with road, place, and boundary overlays |
| **`dataviz-v4-dark`**| `dataviz-v4-light` | Minimal Raster | Muted palette for data visualization overlays |
| **`base-v4`** | `base-v4-dark`, `base-v4-light` | Minimal Raster | Minimalist base layout (replaces deprecated `basic-v2`) |
| **`winter-v4`** | `winter-v4-dark` | Raster 512px | Winter sports, ski trails, and lifts |
| **`ocean-v4`** | `ocean-v4-dark` | Raster 512px | Marine data view and bathymetry contours |
| **`topo-v4`** | `topo-v4-dark`, `topo-v4-pastel` | Topographic | High-detail topographic survey layout |

---

## 4. Legacy-to-Modern Style Translation Table

> [!WARNING]
> All `v2` style variants are deprecated. Always replace legacy strings with their modern **V4** production counterparts:

| Deprecated Key (v2) | Modern Replacement (v4) | Notes / Action |
| :--- | :--- | :--- |
| `streets-v2` | `streets-v4` | Full Planet v4 schema update |
| `streets-v2-dark` / `-night` | `streets-v4-dark` | Dark mode street basemap |
| `streets-v2-pastel` | `streets-v4-pastel` | Low-contrast pastel palette |
| `basic-v2` | `base-v4` | Replace deprecated basic-v2 with modern base-v4 |
| `basic-v2-dark` | `base-v4-dark` | Dark minimalist base |
| `basic-v2-light` | `base-v4-light` | Light minimalist base |
| `outdoor-v2` | `outdoor-v4` | Detailed hiking contours & peaks |
| `outdoor-v2-dark` | `outdoor-v4-dark` | Dark mode outdoor map |
| `satellite` / `satellite-v2` | `satellite-v4` | Clean cloudless satellite imagery |
| `hybrid` / `hybrid-v2` | `hybrid-v4` | Satellite with roads and labels |
| `dataviz` / `dataviz-dark` | `dataviz-v4-dark` | High-contrast data visualization |
| `dataviz-light` | `dataviz-v4-light` | Clean light data visualization |
| `toner-v2` | `base-v4-dark` | High-contrast monochrome dark |
| `voyager-v2` | `streets-v4-pastel` or `base-v4-light` | Muted neutral tones |
| `topo-v2` | `topo-v4` | Standard topographic relief |
