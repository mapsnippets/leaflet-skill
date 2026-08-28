# Leaflet API Reference — Map, Views & Lifecycle

This document provides the complete API reference for `L.Map`, including constructor options, methods, events, coordinate conversion functions, and DOM panes.

---

## 1. Creation & Factory

* `L.map(id: String | HTMLElement, options?: MapOptions): Map`
  * Example: `const map = L.map('map', { center: [50.0755, 14.4378], zoom: 13 });`

---

## 2. `MapOptions` Reference

### Map State Options
| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `preferCanvas` | `Boolean` | `false` | Force canvas renderer for vector paths instead of SVG. |
| `attributionControl`| `Boolean` | `true` | Whether a attribution control will be added to the map by default. |
| `zoomControl` | `Boolean` | `true` | Whether a zoom control is added to the map. |
| `closePopupOnClick` | `Boolean` | `true` | Close popups on map click. |
| `zoomSnap` | `Number` | `1` | Forces map zoom level to snap to multiples of this value. Set to `0` for continuous zoom. |
| `zoomDelta` | `Number` | `1` | Zoom level delta when using buttons or keyboard. |
| `trackResize` | `Boolean` | `true` | Automatically track window resize and update map viewport. |
| `minZoom` | `Number` | `0` | Minimum zoom level of the map. |
| `maxZoom` | `Number` | `Infinity`| Maximum zoom level of the map. |
| `maxBounds` | `LatLngBounds` | `null` | Constrain the map view within given geographical bounds. |
| `renderer` | `Renderer` | `null` | Default renderer (`L.svg()` or `L.canvas()`). |
| `crs` | `CRS` | `L.CRS.EPSG3857` | Coordinate Reference System to use. |

### Interaction Options
| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `dragging` | `Boolean` | `true` | Enable mouse/touch dragging. |
| `touchZoom` | `Boolean` | `true` | Enable touch pinch-to-zoom. |
| `scrollWheelZoom`| `Boolean` / `String`| `true` | Enable mouse wheel zoom. Set to `'center'` to zoom towards center. |
| `doubleClickZoom`| `Boolean` / `String`| `true` | Double click zoom behavior. |
| `boxZoom` | `Boolean` | `true` | Shift-drag box zoom. |
| `tapHold` | `Boolean` | `false` | Simulates contextmenu on long touch press. |
| `keyboard` | `Boolean` | `true` | Keyboard navigation (arrows, +, -). |

### Animation Options
| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `zoomAnimation` | `Boolean` | `true` | Enable smooth zoom transitions. |
| `zoomAnimationThreshold`| `Number` | `4` | Won't animate if zoom difference is greater. |
| `fadeAnimation` | `Boolean` | `true` | Enable tile fade-in animation. |
| `markerZoomAnimation` | `Boolean` | `true` | Animate marker positions during zoom. |

---

## 3. Map Methods Reference

### Modifying Map State
* `setView(center: LatLngExpression, zoom?: Number, options?: ZoomPanOptions): this` — Sets the view of the map.
* `setZoom(zoom: Number, options?: ZoomOptions): this` — Sets the zoom level.
* `zoomIn(delta?: Number, options?: ZoomOptions): this` — Increases zoom level.
* `zoomOut(delta?: Number, options?: ZoomOptions): this` — Decreases zoom level.
* `setZoomAround(latlng: LatLngExpression | Point, zoom: Number, options?: ZoomOptions): this` — Zooms while keeping a specified point stationary.
* `fitBounds(bounds: LatLngBoundsExpression, options?: FitBoundsOptions): this` — Sets a map view that contains the given geographical bounds with maximum possible zoom.
  * Options: `padding: [x, y]`, `paddingTopLeft: [x, y]`, `paddingBottomRight: [x, y]`, `maxZoom: Number`.
* `fitWorld(options?: FitBoundsOptions): this` — Fits the maximum view of the entire world.
* `panTo(latlng: LatLngExpression, options?: PanOptions): this` — Pans the map to a given center.
* `panBy(offset: PointExpression, options?: PanOptions): this` — Pans the map by a given amount of pixels.
* `flyTo(latlng: LatLngExpression, zoom?: Number, options?: ZoomPanOptions): this` — Smooth animated transition to a given center and zoom.
* `flyToBounds(bounds: LatLngBoundsExpression, options?: FitBoundsOptions): this` — Smooth animated flight to bounds.
* `setMaxBounds(bounds: LatLngBoundsExpression): this` — Restricts map viewport to bounds.
* `setMinZoom(zoom: Number): this` — Sets minimum zoom.
* `setMaxZoom(zoom: Number): this` — Sets maximum zoom.
* `panInsideBounds(bounds: LatLngBoundsExpression, options?: PanOptions): this` — Pans map if current center is outside bounds.
* `invalidateSize(options?: Boolean | InvalidateSizeOptions): this` — Checks container size and updates map viewport (critical for tabs/modals/dynamic containers).
* `stop(): this` — Stops current pan or zoom animation.
* `remove(): this` — Destroys the map, unbinds all event listeners, and cleans up DOM.

### Getting Map State
* `getCenter(): LatLng` — Returns geographical center.
* `getZoom(): Number` — Returns current zoom level.
* `getBounds(): LatLngBounds` — Returns geographical bounds of current viewport.
* `getMinZoom(): Number` — Returns min zoom level.
* `getMaxZoom(): Number` — Returns max zoom level.
* `getBoundsZoom(bounds: LatLngBoundsExpression, inside?: Boolean, padding?: Point): Number` — Returns the zoom level needed to fit bounds.
* `getSize(): Point` — Returns size of map container in pixels.
* `getPixelBounds(): Bounds` — Returns bounds in projected pixel coordinates.
* `getPixelOrigin(): Point` — Returns top-left projected pixel coordinates.
* `getPixelWorldBounds(zoom?: Number): Bounds` — Returns pixel coordinates of whole world at given zoom.

### Coordinate Conversion Methods
* `latLngToLayerPoint(latlng: LatLngExpression): Point` — Converts LatLng to pixel coordinates relative to map container.
* `layerPointToLatLng(point: PointExpression): LatLng` — Converts pixel coordinates relative to container to LatLng.
* `latLngToContainerPoint(latlng: LatLngExpression): Point` — Converts LatLng to pixel coordinates relative to browser viewport.
* `containerPointToLatLng(point: PointExpression): LatLng` — Converts viewport pixel coordinates to LatLng.
* `project(latlng: LatLngExpression, zoom: Number): Point` — Projects LatLng to absolute pixel coordinates at zoom.
* `unproject(point: PointExpression, zoom: Number): LatLng` — Inverse of project.
* `mouseEventToContainerPoint(ev: MouseEvent): Point` — Extracts viewport pixel point from mouse event.
* `mouseEventToLayerPoint(ev: MouseEvent): Point` — Extracts map container pixel point from mouse event.
* `mouseEventToLatLng(ev: MouseEvent): LatLng` — Extracts geographical coordinate directly from mouse event.

### Layers and Controls Methods
* `addLayer(layer: Layer): this` — Adds a layer.
* `removeLayer(layer: Layer): this` — Removes a layer.
* `hasLayer(layer: Layer): Boolean` — Checks if layer is on map.
* `eachLayer(fn: Function, context?: Object): this` — Iterates through all layers on map.
* `openPopup(popup: Popup): this` / `openPopup(content: String | HTMLElement, latlng: LatLngExpression, options?: PopupOptions): this`
* `closePopup(popup?: Popup): this` — Closes popup.
* `openTooltip(tooltip: Tooltip): this` / `closeTooltip(tooltip?: Tooltip): this`
* `addControl(control: Control): this` / `removeControl(control: Control): this`
* `createPane(name: String, container?: HTMLElement): HTMLElement` — Creates custom pane.
* `getPane(pane: String | HTMLElement): HTMLElement` — Returns pane by name.
* `getPanes(): Object` — Returns dictionary of all panes.
* `getContainer(): HTMLElement` — Returns map container DOM element.

---

## 4. Map Events Reference

| Event | Data Type | Description |
| :--- | :--- | :--- |
| `click` | `LeafletMouseEvent` | Fired when the map is clicked: `e.latlng`, `e.layerPoint`, `e.containerPoint`. |
| `dblclick` | `LeafletMouseEvent` | Fired on double click. |
| `mousedown` / `mouseup` | `LeafletMouseEvent` | Mouse press / release. |
| `mouseover` / `mouseout`| `LeafletMouseEvent` | Mouse enters / leaves map container. |
| `mousemove` | `LeafletMouseEvent` | Mouse moves across map. |
| `contextmenu` | `LeafletMouseEvent` | Right click or long touch press. |
| `movestart` / `move` / `moveend` | `Event` | Map view center starts, continues, or finishes moving. |
| `zoomstart` / `zoom` / `zoomend` | `Event` | Zoom level starts changing, changes, or finishes. |
| `resize` | `ResizeEvent` | Map container was resized (`e.oldSize`, `e.newSize`). |
| `load` | `Event` | Map is initialized with center and zoom. |
| `unload` | `Event` | Map was destroyed with `remove()`. |
| `layeradd` / `layerremove` | `LayerEvent` | Layer was added (`e.layer`) or removed. |
| `popupopen` / `popupclose` | `PopupEvent` | Popup was opened or closed (`e.popup`). |
| `tooltipopen` / `tooltipclose` | `TooltipEvent` | Tooltip was opened or closed (`e.tooltip`). |
| `locationfound` | `LocationEvent` | Geolocation succeeded (`e.latlng`, `e.bounds`, `e.accuracy`). |
| `locationerror` | `ErrorEvent` | Geolocation failed (`e.message`, `e.code`). |
