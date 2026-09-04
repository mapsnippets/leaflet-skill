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
| **Vector Map Quickstart** | `L.map`, `@maplibre/maplibre-gl-leaflet`, MapTiler `streets-v4`. | [`quickstart-map.md`](../examples/quickstart-map.md) |
| **High-Performance Vector Tiles** | WebGL vector basemap rendering inside standard Leaflet. | [`vector-tiles-maptiler.md`](../examples/vector-tiles-maptiler.md) |
| **Layer Group Switcher** | Dynamic basemap & overlay toggle via `L.control.layers`. | [`layer-groups-control.md`](../examples/layer-groups-control.md) |
| **Fractional Zoom Levels** | Fine-grained zoom animation via `zoomSnap: 0.25`, `zoomDelta: 0.5`. | [`fractional-zoom-display.md`](../examples/fractional-zoom-display.md) |
| **Mobile Geolocation Tracking** | `map.locate({ setView: true, watch: true })` with accuracy circle. | [`mobile-touch-events.md`](../examples/mobile-touch-events.md) |
| **Accessible Screen-Reader Map** | Full keyboard focus trapping, ARIA roles, and tab index order. | [`accessible-map.md`](../examples/accessible-map.md) |
| **Multi-Language Basemap** | MapTiler vector basemap with dynamic language localization. | [`multilingual-map.md`](../examples/multilingual-map.md) |
| **Non-Geographical Pixel CRS** | High-resolution image/game plan navigation with `L.CRS.Simple`. | [`non-geographical-crs-simple.md`](../examples/non-geographical-crs-simple.md) |

### 📁 Category 2: GeoJSON, Vector Styling & Choropleths

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Data-Driven Choropleth** | Population density classification with dynamic hover state info box. | [`interactive-choropleth.md`](../examples/interactive-choropleth.md) |
| **GeoJSON Polygon Styling** | Interactive hover highlights, feature filtering, and click zooms. | [`geojson-polygon-styling.md`](../examples/geojson-polygon-styling.md) |
| **Animated Polylines (Ant Path)** | Pulsing marching-ants SVG stroke dash animation on routes. | [`animated-polyline-antpath.md`](../examples/animated-polyline-antpath.md) |
| **GPS Elevation Profile** | Parsing GPX/GeoJSON tracks with elevation profile charts. | [`gpx-elevation-profile.md`](../examples/gpx-elevation-profile.md) |
| **Spatial GeoJSON Tooltip** | Lightweight coordinate & attribute tooltips via `L.tooltip`. | [`spatial-tooltip.md`](../examples/spatial-tooltip.md) |

### 📁 Category 3: Markers, Icons & Clustering

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Custom HTML Radar Markers** | DOM markers using `L.divIcon` with pulsating CSS radar waves. | [`custom-marker-icons.md`](../examples/custom-marker-icons.md) |
| **High-Density Clustering** | 50,000+ points clustered with `leaflet.markercluster` & spiders. | [`marker-clustering-large-dataset.md`](../examples/marker-clustering-large-dataset.md) |
| **Rotating Heading Markers** | Continuous marker rotation tracking vehicle/vessel heading. | [`rotating-heading-markers.md`](../examples/rotating-heading-markers.md) |
| **Draggable Geocoding Marker** | Reverse geocoding marker with real-time address updates. | [`draggable-reverse-geocoding-marker.md`](../examples/draggable-reverse-geocoding-marker.md) |
| **Bouncing Drop Animation** | Physics-based marker drop animation on viewport initialization. | [`bouncing-marker-drop.md`](../examples/bouncing-marker-drop.md) |

### 📁 Category 4: CAD Digitization & Geometry Editing

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Geoman GIS Drawing Toolbar** | Polygon, rectangle, circle, and cut tools via `@geoman-io`. | [`geoman-drawing-toolbar.md`](../examples/geoman-drawing-toolbar.md) |
| **Geodesic Path Ruler** | Click-to-measure geodesic distance and polygon area calculator. | [`click-to-measure-distance.md`](../examples/click-to-measure-distance.md) |
| **Interactive Bounding Box Filter** | Rubber-band rectangle drag selection querying vector features. | [`bounding-box-filter.md`](../examples/bounding-box-filter.md) |

### 📁 Category 5: Imagery, Overlays, WMS & Vector Tiles

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Georeferenced Drone Orthophoto** | Anchoring high-resolution aerial imagery using `L.imageOverlay`. | [`image-overlay.md`](../examples/image-overlay.md) |
| **Georeferenced Video Stream** | Synchronizing looping MP4 drone footage with `L.videoOverlay`. | [`video-overlay.md`](../examples/video-overlay.md) |
| **Enterprise OGC WMS Layer** | Direct integration of NOAA/radar WMS layers using `L.tileLayer.wms`. | [`wms-environmental-layers.md`](../examples/wms-environmental-layers.md) |
| **Client-Side GeoTIFF (COG)** | In-browser GeoTIFF raster parsing with `georaster-layer-for-leaflet`.| [`cog-geotiff-rendering.md`](../examples/cog-geotiff-rendering.md) |

### 📁 Category 6: Controls, Custom Panes & UX Interactions

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Custom Stacking Panes (Z-Index)** | Isolating labels above vector overlays via `map.createPane()`. | [`custom-map-panes.md`](../examples/custom-map-panes.md) |
| **Split-Screen Map Swipe** | Synchronized side-by-side layer swipe slider using `leaflet-side-by-side`. | [`split-screen-swipe.md`](../examples/split-screen-swipe.md) |
| **Export Map to High-Res Image** | Full client-side map canvas screenshot export via `leaflet-image`. | [`export-map-canvas-image.md`](../examples/export-map-canvas-image.md) |
