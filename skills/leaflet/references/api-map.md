# Leaflet API Reference — Map, Map Methods & Map Misc

Source: https://leafletjs.com/reference.html#map-example

---

## 1. Creation & Factory

* `L.map(id: String | HTMLElement, options?: MapOptions): Map`
  * Example: `const map = L.map('map', { center: [50.0755, 14.4378], zoom: 13 });`

---

## 2. `MapOptions`

### Map State Options
* `preferCanvas` (`Boolean` = `false`): Force canvas rendering for vectors.
* `attributionControl` (`Boolean` = `true`): Add default attribution.
* `zoomControl` (`Boolean` = `true`): Add default zoom control.
* `closePopupOnClick` (`Boolean` = `true`): Close popups on map click.
* `zoomSnap` (`Number` = `1`): Zoom level snap multiple. Set `0` for continuous zoom.
* `zoomDelta` (`Number` = `1`): Zoom level change step.
* `trackResize` (`Boolean` = `true`): Auto-update map on window resize.
* `boxZoom` (`Boolean` = `true`): Shift-drag zoom box.
* `doubleClickZoom` (`Boolean | 'center'` = `true`): Double click zoom behavior.
* `dragging` (`Boolean` = `true`): Mouse/touch pan dragging.
* `crs` (`CRS` = `L.CRS.EPSG3857`): Coordinate reference system.
* `center` (`LatLngExpression`): Initial map center.
* `zoom` (`Number`): Initial zoom level.
* `minZoom` (`Number` = `0`): Minimum zoom level.
* `maxZoom` (`Number` = `Infinity`): Maximum zoom level.
* `maxBounds` (`LatLngBoundsExpression`): Restrict viewport to bounds.
* `maxBoundsViscosity` (`Number` = `0.0`): Solidness of bounds boundary (0.0 to 1.0).
* `renderer` (`Renderer`): Default renderer (`L.svg()` or `L.canvas()`).

### Animation Options
* `zoomAnimation` (`Boolean` = `true`): Smooth zoom transitions.
* `zoomAnimationThreshold` (`Number` = `4`): Max zoom delta to animate.
* `fadeAnimation` (`Boolean` = `true`): Tile fade animation.
* `markerZoomAnimation` (`Boolean` = `true`): Animate markers during zoom.
* `transform3DLimit` (`Number` = `2^23`): Coordinate limit for 3D transforms.

### Interaction & Panning Options
* `inertia` (`Boolean` = `true`): Momentum panning after drag release.
* `inertiaDeceleration` (`Number` = `3000`): Deceleration rate (px/s²).
* `inertiaMaxSpeed` (`Number` = `Infinity`): Max pan velocity.
* `easeLinearity` (`Number` = `0.2`): Cubic bezier curvature.
* `worldCopyJump` (`Boolean` = `false`): Track world copies and jump center on pan.
* `maxBoundsViscosity` (`Number` = `0.0`): Resistance when panning outside bounds.
* `keyboard` (`Boolean` = `true`): Keyboard navigation (+, -, arrow keys).
* `keyboardPanDelta` (`Number` = `80`): Pixels moved per arrow key press.
* `scrollWheelZoom` (`Boolean | 'center'` = `true`): Mouse wheel zooming.
* `wheelDebounceTime` (`Number` = `40`): Debounce interval in ms.
* `wheelPxPerZoomLevel` (`Number` = `60`): Scroll delta per zoom level.
* `tapHold` (`Boolean` = `false`): Simulates contextmenu on long touch press.
* `touchZoom` (`Boolean | 'center'` = `true`): Pinch-to-zoom on touchscreens.

---

## 3. Map Methods

### Modifying Map State
* `setView(center: LatLngExpression, zoom?: Number, options?: ZoomPanOptions): this`
* `setZoom(zoom: Number, options?: ZoomOptions): this`
* `zoomIn(delta?: Number, options?: ZoomOptions): this`
* `zoomOut(delta?: Number, options?: ZoomOptions): this`
* `setZoomAround(latlng: LatLngExpression | Point, zoom: Number, options?: ZoomOptions): this`
* `fitBounds(bounds: LatLngBoundsExpression, options?: FitBoundsOptions): this`
  * Options: `padding: [x,y]`, `paddingTopLeft`, `paddingBottomRight`, `maxZoom`, `animate`.
* `fitWorld(options?: FitBoundsOptions): this`
* `panTo(latlng: LatLngExpression, options?: PanOptions): this`
* `panBy(offset: PointExpression, options?: PanOptions): this`
* `flyTo(latlng: LatLngExpression, zoom?: Number, options?: ZoomPanOptions): this`
* `flyToBounds(bounds: LatLngBoundsExpression, options?: FitBoundsOptions): this`
* `setMaxBounds(bounds: LatLngBoundsExpression): this`
* `setMinZoom(zoom: Number): this` / `setMaxZoom(zoom: Number): this`
* `panInside(latlng: LatLngExpression, options?: PanInsideOptions): this`
* `panInsideBounds(bounds: LatLngBoundsExpression, options?: PanOptions): this`
* `invalidateSize(options?: Boolean | InvalidateSizeOptions): this` — Critical for resize.
* `stop(): this` — Halts current movement animation.
* `remove(): this` — Destroys map, event handlers, and cleans DOM.

### Getting Map State
* `getCenter(): LatLng`
* `getZoom(): Number`
* `getBounds(): LatLngBounds`
* `getMinZoom(): Number` / `getMaxZoom(): Number`
* `getBoundsZoom(bounds: LatLngBoundsExpression, inside?: Boolean, padding?: Point): Number`
* `getSize(): Point` — Dimensions in pixels.
* `getPixelBounds(): Bounds` / `getPixelOrigin(): Point` / `getPixelWorldBounds(zoom?: Number): Bounds`

### Layer & Control Management
* `addLayer(layer: Layer): this` / `removeLayer(layer: Layer): this` / `hasLayer(layer: Layer): Boolean`
* `eachLayer(fn: Function, context?: Object): this`
* `openPopup(popup: Popup): this` / `closePopup(popup?: Popup): this`
* `openTooltip(tooltip: Tooltip): this` / `closeTooltip(tooltip?: Tooltip): this`
* `addControl(control: Control): this` / `removeControl(control: Control): this`
* `createPane(name: String, container?: HTMLElement): HTMLElement`
* `getPane(pane: String | HTMLElement): HTMLElement`
* `getPanes(): Object`
* `getContainer(): HTMLElement`
* `addHandler(name: String, HandlerClass: Function): this`
* `whenReady(fn: Function, context?: Object): this`

### Coordinate Conversion Methods
* `latLngToLayerPoint(latlng: LatLngExpression): Point`
* `layerPointToLatLng(point: PointExpression): LatLng`
* `latLngToContainerPoint(latlng: LatLngExpression): Point`
* `containerPointToLatLng(point: PointExpression): LatLng`
* `layerPointToContainerPoint(point: PointExpression): Point`
* `containerPointToLayerPoint(point: PointExpression): Point`
* `project(latlng: LatLngExpression, zoom: Number): Point`
* `unproject(point: PointExpression, zoom: Number): LatLng`
* `mouseEventToContainerPoint(ev: MouseEvent): Point`
* `mouseEventToLayerPoint(ev: MouseEvent): Point`
* `mouseEventToLatLng(ev: MouseEvent): LatLng`

### Location Methods
* `locate(options?: LocateOptions): this` — Request browser geolocation.
  * Options: `watch`, `setView`, `maxZoom`, `timeout`, `maximumAge`, `enableHighAccuracy`.
* `stopLocate(): this`

---

## 4. Map Misc (Properties & Panes)

### Handlers Properties (Direct toggles on `map` instance):
* `map.boxZoom`: `Handler`
* `map.doubleClickZoom`: `Handler`
* `map.dragging`: `Handler`
* `map.keyboard`: `Handler`
* `map.scrollWheelZoom`: `Handler`
* `map.tapHold`: `Handler`
* `map.touchZoom`: `Handler`

### Default Panes & Default z-indexes:
| Pane Name | Default z-index | Purpose |
| :--- | :--- | :--- |
| `mapPane` | `auto` | Root pane for all map elements |
| `tilePane` | `200` | GridLayer and TileLayer tiles |
| `overlayPane` | `400` | Vector paths, polylines, polygons, GeoJSON |
| `shadowPane` | `500` | Marker shadow images |
| `markerPane` | `600` | Marker icons (`L.marker`) |
| `tooltipPane` | `650` | Tooltips (`L.tooltip`) |
| `popupPane` | `700` | Popups (`L.popup`) |
