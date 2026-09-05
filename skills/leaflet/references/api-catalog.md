# Leaflet Official API Reference Catalog 📖🗺️

> Exhaustive index of all **55 official classes, layers, controls, and utility modules** from [`leafletjs.com/reference.html`](https://leafletjs.com/reference.html).

Every class links directly to the live official Leaflet documentation anchor to enable immediate API reference lookups.

---

## 📑 Architectural Domain Index

* [**Map & Core Architecture**](#map-core-architecture) (6 classes)
* [**UI Layers & Overlays**](#ui-layers-overlays) (6 classes)
* [**Raster & Tile Layers**](#raster-tile-layers) (6 classes)
* [**Vector Geometries & Data**](#vector-geometries-data) (9 classes)
* [**Controls & UI Elements**](#controls-ui-elements) (5 classes)
* [**Projections, CRS & Geometry Math**](#projections-crs-geometry-math) (7 classes)
* [**DOM, Utilities & Renderers**](#dom-utilities-renderers) (9 classes)

---

## Map & Core Architecture

| Class / Entity | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`L.Map`** | L.Map - Core interactive map instance managing viewport, layers, handlers, and panes. | [Docs &rarr;](https://leafletjs.com/reference.html#map) |
| **`L.Map Panes`** | Standardized DOM panes (tilePane, overlayPane, shadowPane, markerPane, tooltipPane, popupPane). | [Docs &rarr;](https://leafletjs.com/reference.html#map-pane) |
| **`L.Class`** | L.Class - Root class for OOP inheritance, options merging, and plugins. | [Docs &rarr;](https://leafletjs.com/reference.html#class) |
| **`L.Evented`** | L.Evented - Core event publishing/subscribing mechanism (`on`, `off`, `fire`). | [Docs &rarr;](https://leafletjs.com/reference.html#evented) |
| **`L.Layer`** | L.Layer - Abstract base class for all map layers and overlays. | [Docs &rarr;](https://leafletjs.com/reference.html#layer) |
| **`L.Interactive Layer`** | Base class adding mouse/pointer interaction support to vector layers. | [Docs &rarr;](https://leafletjs.com/reference.html#interactive-layer) |

---

## UI Layers & Overlays

| Class / Entity | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`L.Marker`** | L.Marker - Clickable/draggable point marker anchored to geographic coordinates. | [Docs &rarr;](https://leafletjs.com/reference.html#marker) |
| **`L.Popup`** | L.Popup - Information window anchored to a geographic location or marker. | [Docs &rarr;](https://leafletjs.com/reference.html#popup) |
| **`L.Tooltip`** | L.Tooltip - Lightweight label or hover tooltip for vector geometries and markers. | [Docs &rarr;](https://leafletjs.com/reference.html#tooltip) |
| **`L.Icon`** | L.Icon - Custom image icon definition for markers with anchor and retina support. | [Docs &rarr;](https://leafletjs.com/reference.html#icon) |
| **`L.DivIcon`** | L.DivIcon - Lightweight marker using custom HTML/CSS and CSS transforms. | [Docs &rarr;](https://leafletjs.com/reference.html#divicon) |
| **`L.DivOverlay`** | Base class for Popup and Tooltip shared behavior. | [Docs &rarr;](https://leafletjs.com/reference.html#divoverlay) |

---

## Raster & Tile Layers

| Class / Entity | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`L.TileLayer`** | L.TileLayer - 256px/512px raster XYZ tile provider with retina/zoomOffset support. | [Docs &rarr;](https://leafletjs.com/reference.html#tilelayer) |
| **`L.TileLayer.WMS`** | L.TileLayer.WMS - OGC Web Map Service client with GetMap bounding box queries. | [Docs &rarr;](https://leafletjs.com/reference.html#tilelayer-wms) |
| **`L.GridLayer`** | L.GridLayer - Generic grid of HTML elements (canvas, DOM, WebGL tiles). | [Docs &rarr;](https://leafletjs.com/reference.html#gridlayer) |
| **`L.ImageOverlay`** | L.ImageOverlay - Georeferenced static image (PNG/JPG) stretched over coordinates. | [Docs &rarr;](https://leafletjs.com/reference.html#imageoverlay) |
| **`L.VideoOverlay`** | L.VideoOverlay - Georeferenced video player stretched over coordinates. | [Docs &rarr;](https://leafletjs.com/reference.html#videooverlay) |
| **`L.SVGOverlay`** | L.SVGOverlay - Georeferenced inline SVG overlay stretched over coordinates. | [Docs &rarr;](https://leafletjs.com/reference.html#svgoverlay) |

---

## Vector Geometries & Data

| Class / Entity | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`L.GeoJSON`** | L.GeoJSON - Ingests GeoJSON features with dynamic styling (`style`) and popups (`onEachFeature`). | [Docs &rarr;](https://leafletjs.com/reference.html#geojson) |
| **`L.Path`** | L.Path - Abstract base class for all SVG/Canvas vector geometry renderers. | [Docs &rarr;](https://leafletjs.com/reference.html#path) |
| **`L.Polyline`** | L.Polyline - Multi-vertex line string with stroke styling and geodesic options. | [Docs &rarr;](https://leafletjs.com/reference.html#polyline) |
| **`L.Polygon`** | L.Polygon - Closed polygon geometry with interior hole ringing support. | [Docs &rarr;](https://leafletjs.com/reference.html#polygon) |
| **`L.Rectangle`** | L.Rectangle - Geographic bounding box polygon derived from LatLngBounds. | [Docs &rarr;](https://leafletjs.com/reference.html#rectangle) |
| **`L.Circle`** | L.Circle - Geographic circle defined by center coordinate and radius in meters. | [Docs &rarr;](https://leafletjs.com/reference.html#circle) |
| **`L.CircleMarker`** | L.CircleMarker - Screen-pixel sized fixed circle (doesn't scale with zoom). | [Docs &rarr;](https://leafletjs.com/reference.html#circlemarker) |
| **`L.LayerGroup`** | L.LayerGroup - Aggregation container to batch-manage and toggle layers together. | [Docs &rarr;](https://leafletjs.com/reference.html#layergroup) |
| **`L.FeatureGroup`** | L.FeatureGroup - LayerGroup extended with shared event dispatching and getBounds(). | [Docs &rarr;](https://leafletjs.com/reference.html#featuregroup) |

---

## Controls & UI Elements

| Class / Entity | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`L.Control`** | L.Control - Base class for UI controls positioned in map corners. | [Docs &rarr;](https://leafletjs.com/reference.html#control) |
| **`L.Control.Zoom`** | Standard '+' / '-' zoom buttons. | [Docs &rarr;](https://leafletjs.com/reference.html#control-zoom) |
| **`L.Control.Attribution`** | Collapsible copyright and data attribution bar. | [Docs &rarr;](https://leafletjs.com/reference.html#control-attribution) |
| **`L.Control.Layers`** | Radio/checkbox switcher for switching basemaps and overlay layers. | [Docs &rarr;](https://leafletjs.com/reference.html#control-layers) |
| **`L.Control.Scale`** | Metric and imperial dynamic visual scale bar. | [Docs &rarr;](https://leafletjs.com/reference.html#control-scale) |

---

## Projections, CRS & Geometry Math

| Class / Entity | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`L.LatLng`** | L.LatLng - Geographic point represented by latitude and longitude. | [Docs &rarr;](https://leafletjs.com/reference.html#latlng) |
| **`L.LatLngBounds`** | L.LatLngBounds - Rectangular geographical area defined by SW and NE coordinates. | [Docs &rarr;](https://leafletjs.com/reference.html#latlngbounds) |
| **`L.Point`** | L.Point - Screen coordinate point represented by x and y in pixels. | [Docs &rarr;](https://leafletjs.com/reference.html#point) |
| **`L.Bounds`** | L.Bounds - 2D rectangular pixel bounding box. | [Docs &rarr;](https://leafletjs.com/reference.html#bounds) |
| **`L.CRS`** | L.CRS - Coordinate Reference System (EPSG:3857, EPSG:4326, Simple). | [Docs &rarr;](https://leafletjs.com/reference.html#crs) |
| **`L.Projection`** | L.Projection - Mathematical projection algorithms (Spherical Mercator, LonLat). | [Docs &rarr;](https://leafletjs.com/reference.html#projection) |
| **`L.Transformation`** | Affine transformation matrix for mapping CRS to pixel coordinates. | [Docs &rarr;](https://leafletjs.com/reference.html#transformation) |

---

## DOM, Utilities & Renderers

| Class / Entity | Description | Official Documentation Link |
| :--- | :--- | :--- |
| **`L.Renderer`** | Base class for vector renderers. | [Docs &rarr;](https://leafletjs.com/reference.html#renderer) |
| **`L.SVG`** | SVG-based vector rendering engine (default on desktop). | [Docs &rarr;](https://leafletjs.com/reference.html#svg) |
| **`L.Canvas`** | HTML5 Canvas 2D vector rendering engine (preferCanvas: true). | [Docs &rarr;](https://leafletjs.com/reference.html#canvas) |
| **`L.DomEvent`** | Cross-browser DOM event normalization and listener management. | [Docs &rarr;](https://leafletjs.com/reference.html#domevent) |
| **`L.DomUtil`** | DOM manipulation utility functions (create, addClass, setTransform, getPosition). | [Docs &rarr;](https://leafletjs.com/reference.html#domutil) |
| **`L.Browser`** | Runtime browser capabilities detection (mobile, retina, touch, svg). | [Docs &rarr;](https://leafletjs.com/reference.html#browser) |
| **`L.Util`** | General utility functions (extend, bind, stamp, formatNum). | [Docs &rarr;](https://leafletjs.com/reference.html#util) |
| **`L.Draggable`** | Pointer/touch dragging utility class. | [Docs &rarr;](https://leafletjs.com/reference.html#draggable) |
| **`L.PosAnimation`** | Smooth CSS 3D translation animations for map panning. | [Docs &rarr;](https://leafletjs.com/reference.html#posanimation) |

---
