# Leaflet Official Recipe Directory & API Cross-Reference 📚🛠️

> An encyclopedic technical directory connecting every Leaflet API class, layer, plugin, and interaction directly to its verified, production-grade standalone recipe in `skills/examples/`. Engineered with modern MapTiler Planet v4 styles.

---

## 1. Core Map, Basemaps & Projections

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Quickstart Basemap** | `L.map`, `L.tileLayer`, MapTiler `streets-v4`, 512px retina offset. | [`quickstart.md`](../examples/quickstart.md) |
| **High-DPI Retina Tiles** | `zoomOffset: -1`, `tileSize: 512`, `detectRetina: false`. | [`retina-hidpi-tiles.md`](../examples/retina-hidpi-tiles.md) |
| **Fractional Zoom Physics** | `zoomSnap: 0.25`, `zoomDelta: 0.5`, `wheelPxPerZoomLevel: 120`. | [`zoom-levels.md`](../examples/zoom-levels.md) |
| **Non-Geographic Flat Coordinates** | `L.CRS.Simple`, pixel coordinates, floorplans, game maps. | [`crs-simple.md`](../examples/crs-simple.md) |
| **Mobile Geolocation Tracking** | `map.locate`, `locationfound`, `locationerror`, GPS circle. | [`mobile-geolocation.md`](../examples/mobile-geolocation.md) |
| **Accessible Screen-Reader Map** | ARIA roles, live regions, keyboard pan/zoom announcements. | [`accessibility-aria.md`](../examples/accessibility-aria.md) |

---

## 2. GeoJSON, Vector Styling & Choropleths

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **GeoJSON Interactive Choropleth** | `L.geoJSON`, dynamic styling, hover feedback, custom HTML legend. | [`geojson-choropleth.md`](../examples/geojson-choropleth.md) |
| **Animated Ant Path Polylines** | `leaflet-ant-path`, marching-ants SVG animation, trajectory tracking. | [`animated-polyline-ant-path.md`](../examples/animated-polyline-ant-path.md) |
| **Geodesic Great-Circle Paths** | `leaflet-geodesic`, ellipsoidal flight paths, international distance. | [`geodesic-measure.md`](../examples/geodesic-measure.md) |
| **Interactive Measure Tool** | Click-to-measure distance polylines, bearing, dynamic tooltips. | [`interactive-measure-tool.md`](../examples/interactive-measure-tool.md) |
| **GPX Hiking Track Viewer** | `leaflet-gpx`, parsing elevation profiles, waypoints, track bounds. | [`gpx-track-viewer.md`](../examples/gpx-track-viewer.md) |

---

## 3. Markers, Icons & Clustering

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Marker Clustering (10k+ points)** | `leaflet.markercluster`, Spiderfy, donut cluster badges, chunking. | [`marker-clustering.md`](../examples/marker-clustering.md) |
| **Custom PNG / SVG Retinal Icons** | `L.icon`, `iconSize`, `iconAnchor`, `popupAnchor`, shadow mapping. | [`custom-icons.md`](../examples/custom-icons.md) |
| **Radar Pulse Animation Marker** | `L.divIcon`, CSS keyframe sonar waves, glowing cyber dot. | [`custom-radar-pulse-marker.md`](../examples/custom-radar-pulse-marker.md) |
| **Rotating Heading Marker** | `leaflet-rotatedmarker`, vehicle/plane yaw angle, GPS course. | [`rotating-marker-heading.md`](../examples/rotating-marker-heading.md) |
| **Canvas Point Density Heatmap** | `leaflet.heat`, intensity gradients, blur/radius tuning. | [`heatmaps.md`](../examples/heatmaps.md) |

---

## 4. CAD Digitization & Geometry Editing

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Geoman Vector CAD Editing** | `@geoman-io/leaflet-geoman-free`, drawing, vertex editing, snapping. | [`geoman-geometry-editing.md`](../examples/geoman-geometry-editing.md) |

---

## 5. Imagery, Overlays, WMS & Vector Tiles

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Floorplan / Orthophoto Overlay** | `L.imageOverlay`, `LatLngBounds`, image georeferencing, opacity. | [`image-overlay-floorplan.md`](../examples/image-overlay-floorplan.md) |
| **Cloud-Optimized GeoTIFF (COG)** | `georaster-layer-for-leaflet`, client-side multiband rendering. | [`geotiff-raster-layer.md`](../examples/geotiff-raster-layer.md) |
| **Enterprise OGC WMS Layer** | `L.tileLayer.wms`, transparent PNG, CQL filters, version 1.3.0. | [`wms-layers.md`](../examples/wms-layers.md) |
| **WMS GetFeatureInfo on Click** | Spatial point inspection via GetFeatureInfo URL, HTML response popup. | [`wms-getfeatureinfo-click.md`](../examples/wms-getfeatureinfo-click.md) |
| **Vector Tiles in Leaflet (MVT)** | `Leaflet.VectorGrid.protobuf`, client-side MVT rendering, styling. | [`vector-grid-mvt.md`](../examples/vector-grid-mvt.md) |

---

## 6. Controls, Custom Panes & UX Interactions

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Custom Stacking Panes** | `map.createPane`, z-index layering, labels above polygons. | [`map-panes.md`](../examples/map-panes.md) |
| **Split-Screen Layer Swipe** | `leaflet-side-by-side`, comparing Streets vs Satellite imagery. | [`side-by-side.md`](../examples/side-by-side.md) |
| **Mini-Map / Inset Overview** | `leaflet-minimap`, secondary synchronized camera, rectangle bounds. | [`minimap-overview.md`](../examples/minimap-overview.md) |
| **Custom UI Control** | `L.Control.extend`, DOM event isolation, glassmorphism panel. | [`custom-control.md`](../examples/custom-control.md) |
| **Basemap & Overlay Switcher** | `L.control.layers`, radio basemaps, toggleable feature groups. | [`layers-control.md`](../examples/layers-control.md) |
| **HTML5 Fullscreen Toggle** | `leaflet.fullscreen`, FullScreen API, mobile orientation handling. | [`fullscreen-toggle.md`](../examples/fullscreen-toggle.md) |
