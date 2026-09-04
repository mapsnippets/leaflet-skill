# Leaflet API Reference — Other Layers, Controls & Base Classes

Source: https://leafletjs.com/reference.html#layergroup

---

## 1. Other Layers

### `L.LayerGroup`
* `L.layerGroup(layers?: Layer[], options?: LayerOptions): LayerGroup`
* **Methods:** `addLayer(layer)`, `removeLayer(layer)`, `hasLayer(layer)`, `clearLayers()`, `invoke(methodName, ...)`, `eachLayer(fn, context)`, `getLayer(id)`, `getLayers(): Layer[]`, `setZIndex(zIndex)`.

### `L.FeatureGroup`
* Extends `LayerGroup` with event forwarding and batch operations.
* `L.featureGroup(layers?: Layer[], options?: LayerOptions): FeatureGroup`
* **Methods:** `bindPopup(content, options)`, `unbindPopup()`, `bindTooltip(content, options)`, `unbindTooltip()`, `setStyle(style: PathOptions): this`, `bringToFront(): this`, `bringToBack(): this`, `getBounds(): LatLngBounds`.

### `L.GeoJSON`
* `L.geoJSON(geojson?: Object, options?: GeoJSONOptions): GeoJSON`
* **Static Methods:**
  * `L.GeoJSON.geometryToLayer(featureData, options)`
  * `L.GeoJSON.coordsToLatLng(coords: [lng, lat]): LatLng`
  * `L.GeoJSON.coordsToLatLngs(coords, levelsDeep)`
  * `L.GeoJSON.latLngToCoords(latlng: LatLng): [lng, lat]`
  * `L.GeoJSON.latLngsToCoords(latlngs, levelsDeep, closed)`
  * `L.GeoJSON.asFeature(geojson): Object`
* **Options:**
  * `pointToLayer(geoJsonPoint, latlng): Layer`
  * `style(geoJsonFeature): PathOptions`
  * `onEachFeature(feature, layer)`
  * `filter(geoJsonFeature): Boolean`
  * `coordsToLatLng(coords): LatLng`
* **Methods:** `addData(data: Object): this`, `resetStyle(layer?: Layer): this`.

### `L.GridLayer`
* Base class for all tiled grid layers (including `TileLayer`).
* `L.gridLayer(options?: GridLayerOptions): GridLayer`
* **Options:** `tileSize`, `opacity`, `updateWhenIdle`, `updateWhenZooming`, `updateInterval`, `zIndex`, `bounds`, `minZoom`, `maxZoom`, `maxNativeZoom`, `minNativeZoom`, `noWrap`, `pane`, `className`, `keepBuffer`.
* **Methods:** `bringToFront()`, `bringToBack()`, `getContainer()`, `setOpacity(opacity)`, `setZIndex(zIndex)`, `isLoading(): Boolean`, `redraw()`, `getTileSize(): Point`.

---

## 2. Controls

All controls inherit from `L.Control`.
* **Position options:** `'topleft'`, `'topright'`, `'bottomleft'`, `'bottomright'`.
* **Methods:** `getPosition(): String`, `setPosition(position: String): this`, `getContainer(): HTMLElement`, `addTo(map: Map): this`, `remove(): this`.

### Built-in Controls:
* **`L.control.zoom(options?: ZoomOptions)`**: `zoomInText`, `zoomOutText`, `zoomInTitle`, `zoomOutTitle`.
* **`L.control.attribution(options?: AttributionOptions)`**: `prefix` (`'Leaflet' | false`), `addAttribution(text)`, `removeAttribution(text)`, `setPrefix(prefix)`.
* **`L.control.scale(options?: ScaleOptions)`**: `maxWidth` (`100`), `metric` (`true`), `imperial` (`true`), `updateWhenIdle` (`false`).
* **`L.control.layers(baseLayers?: Object, overlays?: Object, options?: LayersOptions)`**:
  * Methods: `addBaseLayer(layer, name)`, `addOverlay(layer, name)`, `removeLayer(layer)`, `expand()`, `collapse()`.
  * Options: `collapsed` (`true`), `autoZIndex` (`true`), `hideSingleBase` (`false`), `sortLayers` (`false`), `sortFunction`.

---

## 3. Base Classes

### `L.Class`
Leaflet's OOP base object providing inheritance:
* `L.Class.extend(props: Object): Function`
* `L.Class.include(props: Object): this` — Mixins additional methods.
* `L.Class.mergeOptions(options: Object): this`
* `L.Class.addInitHook(fn: Function): this`

### `L.Evented`
Base class for event handling:
* `on(type: String | Object, fn?: Function, context?: Object): this`
* `off(type?: String | Object, fn?: Function, context?: Object): this`
* `fire(type: String, data?: Object, propagate?: Boolean): this`
* `listens(type: String, propagate?: Boolean): Boolean`
* `once(type: String, fn: Function, context?: Object): this`
* `addEventParent(obj: Evented): this` / `removeEventParent(obj: Evented): this`

### `L.Layer`
Base class for all map layers:
* `addTo(map: Map | LayerGroup): this`
* `remove(): this` / `removeFrom(map: Map): this`
* `getPane(name?: String): HTMLElement`
* `getAttribution(): String`

### `L.Handler`
Base class for toggleable map interaction modules:
* `enable(): this` / `disable(): this` / `enabled(): Boolean`
