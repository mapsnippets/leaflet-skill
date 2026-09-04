# Leaflet API Reference — Layers, Groups & Controls

This document details raster tile sources (`TileLayer`, `TileLayer.WMS`, `ImageOverlay`, `VideoOverlay`), layer management containers (`LayerGroup`, `FeatureGroup`, `GeoJSON`, `GridLayer`), and UI controls (`Zoom`, `Attribution`, `Layers`, `Scale`).

---

## 1. Raster Layers

### `L.TileLayer`
* **Factory:** `L.tileLayer(urlTemplate: String, options?: TileLayerOptions): TileLayer`
* **URL Template Variables:**
  * `{z}`: Zoom level
  * `{x}`, `{y}`: Tile coordinates
  * `{s}`: Subdomain (`a`, `b`, `c`)
  * `{r}`: Retina suffix (`@2x`)
* **Key Options:**
  * `minZoom`: `Number` (`0`) / `maxZoom`: `Number` (`18`)
  * `maxNativeZoom`: `Number` (`null`) — Overzoom tiles past native resolution without 404s.
  * `minNativeZoom`: `Number` (`null`)
  * `tileSize`: `Number | Point` (`256` default, use `512` for MapTiler tiles)
  * `zoomOffset`: `Number` (`0` default, use `-1` for 512px tiles)
  * `zoomReverse`: `Boolean` (`false`)
  * `subdomains`: `String | String[]` (`'abc'`)
  * `errorTileUrl`: `String` (`''`) — Fallback image if tile load errors.
  * `attribution`: `String` (`''`) — Attribution string.
  * `tms`: `Boolean` (`false`) — Inverts Y axis coordinate for TMS.
  * `crossOrigin`: `Boolean | String` (`true`)

### `L.TileLayer.WMS`
* **Factory:** `L.tileLayer.wms(baseUrl: String, options: WMSOptions): TileLayer.WMS`
* **Key Options:** `layers` (required comma-separated layers), `styles`, `format` (`'image/jpeg'` or `'image/png'`), `transparent` (`false`), `version` (`'1.1.1'`), `crs` (`CRS`).

### `L.ImageOverlay` & `L.VideoOverlay`
* **Image Factory:** `L.imageOverlay(imageUrl: String, bounds: LatLngBoundsExpression, options?: ImageOverlayOptions): ImageOverlay`
  * `setOpacity(opacity: Number): this` / `setUrl(url: String): this` / `setBounds(bounds: LatLngBounds): this`
* **Video Factory:** `L.videoOverlay(video: String | Array | HTMLVideoElement, bounds: LatLngBoundsExpression, options?: VideoOverlayOptions): VideoOverlay`
  * `getElement(): HTMLVideoElement` (access HTML5 video controls `.play()`, `.pause()`).

---

## 2. Layer Containers & GeoJSON

### `L.LayerGroup`
* **Factory:** `L.layerGroup(layers?: Layer[], options?: LayerOptions): LayerGroup`
* **Key Methods:**
  * `addLayer(layer: Layer): this`
  * `removeLayer(layer: Layer | String): this`
  * `hasLayer(layer: Layer): Boolean`
  * `clearLayers(): this`
  * `eachLayer(fn: Function, context?: Object): this`

### `L.FeatureGroup`
* Extends `LayerGroup` with events and popup/tooltip convenience methods.
* **Factory:** `L.featureGroup(layers?: Layer[], options?: LayerOptions): FeatureGroup`
* **Key Methods:**
  * `bindPopup(content, options)` / `bindTooltip(content, options)`
  * `setStyle(style: PathOptions): this` — Sets styling on all child vector paths simultaneously.
  * `bringToFront(): this` / `bringToBack(): this`
  * `getBounds(): LatLngBounds` — Calculates combined geographic bounding box.

### `L.GeoJSON`
* **Factory:** `L.geoJSON(geojson?: Object, options?: GeoJSONOptions): GeoJSON`
* **Static Methods:**
  * `L.GeoJSON.geometryToLayer(featureData: Object, options?: GeoJSONOptions): Layer`
  * `L.GeoJSON.coordsToLatLng(coords: Array): LatLng` — Translates GeoJSON `[lng, lat]` to Leaflet `LatLng(lat, lng)`.
  * `L.GeoJSON.coordsToLatLngs(coords: Array, levelsDeep?: Number): Array`
  * `L.GeoJSON.latLngToCoords(latlng: LatLng): Array` — Translates Leaflet `LatLng` to GeoJSON `[lng, lat]`.
* **Options:**
  * `pointToLayer(geoJsonPoint, latlng): Layer` — Defines how points are rendered (e.g. into `L.marker` or `L.circleMarker`).
  * `style(geoJsonFeature): PathOptions` — Dynamic style object for lines and polygons.
  * `onEachFeature(feature, layer)` — Called on each created layer for event binding and popups.
  * `filter(geoJsonFeature): Boolean` — Returns `false` to omit specific features.
* **Methods:**
  * `addData(data: Object): this` — Adds GeoJSON features incrementally.
  * `resetStyle(layer?: Layer): this` — Resets layer style back to original `style` function.

---

## 3. UI Controls

All controls inherit from `L.Control` and support positioning via `position`: `'topleft'`, `'topright'`, `'bottomleft'`, `'bottomright'`.

* **`L.control.zoom(options?: ZoomOptions)`** — Zoom in/out buttons (`zoomInText`, `zoomOutText`, `zoomInTitle`, `zoomOutTitle`).
* **`L.control.attribution(options?: AttributionOptions)`** — Attributions footer (`prefix: 'Leaflet' | false`, `addAttribution(text)`, `removeAttribution(text)`).
* **`L.control.scale(options?: ScaleOptions)`** — Distance scale (`maxWidth: 100`, `metric: true`, `imperial: true`).
* **`L.control.layers(baseLayers?: Object, overlays?: Object, options?: LayersOptions)`** — Layer switcher control.
  * `addBaseLayer(layer: Layer, name: String): this`
  * `addOverlay(layer: Layer, name: String): this`
  * `removeLayer(layer: Layer): this`
  * `expand(): this` / `collapse(): this`
* **Custom Control Extension Pattern (`L.Control.extend`)**:
  ```javascript
  const CustomLegendControl = L.Control.extend({
    options: { position: 'bottomright' },
    onAdd: function(map) {
      const container = L.DomUtil.create('div', 'leaflet-bar custom-legend');
      container.style.backgroundColor = 'white';
      container.style.padding = '8px 12px';
      container.innerHTML = '<h4>Map Legend</h4><p>Custom HTML</p>';
      L.DomEvent.disableClickPropagation(container);
      return container;
    },
    onRemove: function(map) {
      // Teardown event listeners if added
    }
  });
  map.addControl(new CustomLegendControl());
  ```
