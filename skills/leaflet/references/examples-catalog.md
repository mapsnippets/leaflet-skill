# Leaflet — Complete Official Examples Catalog & Recipe Matrix 📚🗺️

> Exhaustive index of all **official Leaflet tutorials and examples** from [`leafletjs.com/examples.html`](https://leafletjs.com/examples.html) and core ecosystem plugin capabilities, cross-referenced with MapSnippets production recipes in [`skills/leaflet/examples/`](../examples/INDEX.md).

---

## 🧭 1. Official Core Leaflet Tutorials (`leafletjs.com`)

| Official Tutorial | Key APIs & Techniques | Standalone Recipe |
| :--- | :--- | :--- |
| **[Leaflet Quick Start Guide](https://leafletjs.com/examples/quick-start/)** | Core `quick-start` official tutorial. | — |
| **[Leaflet on Mobile](https://leafletjs.com/examples/mobile/)** | Core `mobile` official tutorial. | — |
| **[Markers with Custom Icons](https://leafletjs.com/examples/custom-icons/)** | Core `custom-icons` official tutorial. | — |
| **[Accessible maps](https://leafletjs.com/examples/accessibility/)** | Core `accessibility` official tutorial. | — |
| **[Using GeoJSON with Leaflet](https://leafletjs.com/examples/geojson/)** | Core `geojson` official tutorial. | — |
| **[Interactive Choropleth Map](https://leafletjs.com/examples/choropleth/)** | Core `choropleth` official tutorial. | — |
| **[Layer Groups and Layers Control](https://leafletjs.com/examples/layers-control/)** | Core `layers-control` official tutorial. | — |
| **[Zoom levels](https://leafletjs.com/examples/zoom-levels/)** | Core `zoom-levels` official tutorial. | — |
| **[Working with map panes](https://leafletjs.com/examples/map-panes/)** | Core `map-panes` official tutorial. | — |
| **[Overlays: Image, Video, SVG](https://leafletjs.com/examples/overlays/)** | Core `overlays` official tutorial. | — |

---

## 🧭 2. MapSnippets Production Recipe Matrix (Categorized by Domain)

### 📁 Category 1: Core Map, Basemaps & Mobile

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Vector Map Quickstart** | `L.map`, `@maplibre/maplibre-gl-leaflet`, MapTiler `streets-v4`. | [`quickstart.md`](../examples/quickstart.md) |
| **High-DPI Retina Tiles** | High-DPI 512px `@2x` raster tiles with `detectRetina: true`. | [`retina-hidpi-tiles.md`](../examples/retina-hidpi-tiles.md) |
| **Layer Group Switcher** | Dynamic basemap & overlay toggle via `L.control.layers`. | [`layers-control.md`](../examples/layers-control.md) |
| **Fractional Zoom Levels** | Fine-grained zoom animation via `zoomSnap: 0.25`, `zoomDelta: 0.5`. | [`zoom-levels.md`](../examples/zoom-levels.md) |
| **Mobile Geolocation Tracking** | `map.locate({ setView: true, watch: true })` with accuracy circle. | [`mobile-geolocation.md`](../examples/mobile-geolocation.md) |
| **Accessible Screen-Reader Map** | Full keyboard focus trapping, ARIA roles, and tab index order. | [`accessibility-aria.md`](../examples/accessibility-aria.md) |
| **Non-Geographical Pixel CRS** | High-resolution image/game plan navigation with `L.CRS.Simple`. | [`crs-simple.md`](../examples/crs-simple.md) |

### 📁 Category 2: GeoJSON, Vector Styling & Choropleths

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Data-Driven Choropleth** | Population density classification with dynamic hover state info box. | [`geojson-choropleth.md`](../examples/geojson-choropleth.md) |
| **Animated Polylines (Ant Path)** | Pulsing marching-ants SVG stroke dash animation on routes. | [`animated-polyline-ant-path.md`](../examples/animated-polyline-ant-path.md) |
| **GPS Track & Waypoint Viewer** | Parsing GPX/GeoJSON tracks with elevation and waypoint styling. | [`gpx-track-viewer.md`](../examples/gpx-track-viewer.md) |

### 📁 Category 3: Markers, Icons & Clustering

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Custom Icons & Anchors** | Subclassing `L.Icon` with retina support, anchor points, and offsets. | [`custom-icons.md`](../examples/custom-icons.md) |
| **Custom HTML Radar Markers** | DOM markers using `L.divIcon` with pulsating CSS radar waves. | [`custom-radar-pulse-marker.md`](../examples/custom-radar-pulse-marker.md) |
| **High-Density Clustering** | 50,000+ points clustered with `leaflet.markercluster` & spiders. | [`marker-clustering.md`](../examples/marker-clustering.md) |
| **Rotating Heading Markers** | Continuous marker rotation tracking vehicle/vessel heading. | [`rotating-marker-heading.md`](../examples/rotating-marker-heading.md) |

### 📁 Category 4: CAD Digitization & Geometry Editing

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Geoman GIS Drawing Toolbar** | Complete vector digitization (drawing, editing, cutting polygons). | [`geoman-geometry-editing.md`](../examples/geoman-geometry-editing.md) |
| **Interactive Measure Tool** | Interactive geodesic distance and polygon area measurement control. | [`interactive-measure-tool.md`](../examples/interactive-measure-tool.md) |
| **Geodesic Path & Spherical Distance** | Great-circle navigation curves and spherical geodesic calculations. | [`geodesic-measure.md`](../examples/geodesic-measure.md) |

### 📁 Category 5: Imagery, Overlays, WMS & Vector Tiles

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Architectural Floorplan Overlay** | Anchoring high-resolution CAD/floorplan raster with `L.imageOverlay`. | [`image-overlay-floorplan.md`](../examples/image-overlay-floorplan.md) |
| **Enterprise OGC WMS Layer** | Direct integration of NOAA/radar WMS layers using `L.tileLayer.wms`. | [`wms-layers.md`](../examples/wms-layers.md) |
| **WMS GetFeatureInfo on Click** | Interactive attribute queries from remote WMS service on click. | [`wms-getfeatureinfo-click.md`](../examples/wms-getfeatureinfo-click.md) |
| **Client-Side GeoTIFF (COG)** | In-browser GeoTIFF raster parsing with `georaster-layer-for-leaflet`.| [`geotiff-raster-layer.md`](../examples/geotiff-raster-layer.md) |
| **Binary Vector Tiles (MVT)** | Native Canvas rendering of Mapbox Vector Tiles via `VectorGrid`. | [`vector-grid-mvt.md`](../examples/vector-grid-mvt.md) |

### 📁 Category 6: Controls, Custom Panes & UX Interactions

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Custom Stacking Panes (Z-Index)** | Isolating labels above vector overlays via `map.createPane()`. | [`map-panes.md`](../examples/map-panes.md) |
| **Split-Screen Map Swipe** | Synchronized side-by-side layer swipe slider using `side-by-side`. | [`side-by-side.md`](../examples/side-by-side.md) |
| **Fullscreen Map Toggle** | Responsive fullscreen viewport toggle control. | [`fullscreen-toggle.md`](../examples/fullscreen-toggle.md) |
| **Point Density Heatmap** | Continuous heat density rendering with WebGL / `simpleheat`. | [`heatmaps.md`](../examples/heatmaps.md) |
| **Corner Mini-Map Overview** | Synchronized overview locator map in the viewport corner. | [`minimap-overview.md`](../examples/minimap-overview.md) |
| **Custom Subclassed Control** | Custom map control subclassing `L.Control.extend` with propagation stops. | [`custom-control.md`](../examples/custom-control.md) |
