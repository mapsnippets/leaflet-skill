# Leaflet API Reference — UI & Vector Layers

This document provides the complete API reference for Markers, Popups, Tooltips, DivIcons, and Vector Paths (`Polyline`, `Polygon`, `Circle`, `CircleMarker`, `Rectangle`, `SVG`, `Canvas`).

---

## 1. UI Layers

### `L.Marker`
* **Factory:** `L.marker(latlng: LatLngExpression, options?: MarkerOptions): Marker`
* **Key Options:**
  * `icon`: `L.Icon | L.DivIcon` — Custom icon.
  * `draggable`: `Boolean` (`false`) — Whether the marker can be dragged.
  * `autoPan`: `Boolean` (`false`) — Pan map when dragging near edge.
  * `keyboard`: `Boolean` (`true`) — Tab key focusable.
  * `title`: `String` (`''`) — Browser tooltip on hover.
  * `alt`: `String` (`'Marker'`) — Accessibility alt text.
  * `zIndexOffset`: `Number` (`0`) — Offset z-index for ordering.
  * `opacity`: `Number` (`1.0`) — Marker opacity.
  * `riseOnHover`: `Boolean` (`false`) — Pull marker to top z-index on mouseover.
  * `pane`: `String` (`'markerPane'`) — Target pane.
* **Key Methods:**
  * `getLatLng(): LatLng` / `setLatLng(latlng: LatLngExpression): this`
  * `setIcon(icon: Icon | DivIcon): this`
  * `setOpacity(opacity: Number): this`
  * `setZIndexOffset(offset: Number): this`
  * `bindPopup(content: String | HTMLElement | Function | Popup, options?: PopupOptions): this`
  * `unbindPopup(): this` / `openPopup(): this` / `closePopup(): this` / `togglePopup(): this`
  * `bindTooltip(content: String | HTMLElement | Function | Tooltip, options?: TooltipOptions): this`
  * `toGeoJSON(precision?: Number): Object`
* **Events:** `dragstart`, `movestart`, `drag`, `move`, `dragend`, `moveend`.

### `L.Popup`
* **Factory:** `L.popup(options?: PopupOptions, source?: Layer): Popup`
* **Key Options:**
  * `maxWidth`: `Number` (`300`) / `minWidth`: `Number` (`50`) / `maxHeight`: `Number` (`null`)
  * `autoPan`: `Boolean` (`true`) — Auto-pan map so popup is visible.
  * `autoPanPadding`: `Point` (`[5, 5]`)
  * `keepInView`: `Boolean` (`false`) — Prevent user panning from obscuring popup.
  * `closeButton`: `Boolean` (`true`) — Show 'x' close button.
  * `autoClose`: `Boolean` (`true`) — Close when another popup opens.
  * `closeOnEscapeKey`: `Boolean` (`true`)
  * `className`: `String` (`''`) — Custom CSS class for popup container.
* **Key Methods:**
  * `setLatLng(latlng: LatLngExpression): this`
  * `setContent(htmlContent: String | HTMLElement | Function): this`
  * `openOn(map: Map): this`

### `L.Tooltip`
* **Factory:** `L.tooltip(options?: TooltipOptions, source?: Layer): Tooltip`
* **Key Options:**
  * `pane`: `String` (`'tooltipPane'`)
  * `offset`: `Point` (`[0, 0]`)
  * `direction`: `'right' | 'left' | 'top' | 'bottom' | 'center' | 'auto'` (`'auto'`)
  * `permanent`: `Boolean` (`false`) — Always open (not just on hover).
  * `sticky`: `Boolean` (`false`) — Follow mouse cursor.
  * `opacity`: `Number` (`0.9`)

### `L.Icon` & `L.DivIcon`
* **`L.icon(options: IconOptions)`**:
  * `iconUrl`: `String` (required)
  * `iconRetinaUrl`: `String`
  * `iconSize`: `PointExpression` (e.g. `[32, 32]`)
  * `iconAnchor`: `PointExpression` (point of icon corresponding to marker location)
  * `popupAnchor`: `PointExpression` (offset for popup opening)
  * `shadowUrl`: `String` / `shadowSize`: `Point` / `shadowAnchor`: `Point`
* **`L.divIcon(options: DivIconOptions)`**:
  * `html`: `String | HTMLElement | false` — Custom HTML snippet.
  * `bgPos`: `PointExpression`
  * `className`: `String` (`'leaflet-div-icon'`)

---

## 2. Vector Layers & Paths

All vector shapes inherit from `L.Path`.

### Path Options (`PathOptions`)
| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `stroke` | `Boolean` | `true` | Whether to draw stroke along the path. |
| `color` | `String` | `'#3388ff'` | Stroke color (hex, rgb, css name). |
| `weight` | `Number` | `3` | Stroke width in pixels. |
| `opacity` | `Number` | `1.0` | Stroke opacity. |
| `lineCap` | `String` | `'round'` | Shape at end of stroke (`'butt'`, `'round'`, `'square'`). |
| `lineJoin` | `String` | `'round'` | Shape at corners (`'miter'`, `'round'`, `'bevel'`). |
| `dashArray` | `String` | `null` | Stroke dash pattern (e.g. `'5, 10'`). |
| `dashOffset` | `String` | `null` | Dash pattern offset. |
| `fill` | `Boolean` | `depends` | Whether to fill the path with color. |
| `fillColor` | `String` | `color` | Fill color. |
| `fillOpacity` | `Number` | `0.2` | Fill opacity. |
| `fillRule` | `String` | `'evenodd'` | How interior of path is determined. |
| `renderer` | `Renderer` | `null` | Explicit `L.canvas()` or `L.svg()` instance. |
| `className` | `String` | `null` | Custom SVG/Canvas class name. |

### Vector Classes & Factories:
* **`L.polyline(latlngs: LatLngExpression[] | LatLngExpression[][], options?: PolylineOptions): Polyline`**
  * `getLatLngs(): LatLng[] | LatLng[][]` / `setLatLngs(latlngs: LatLngExpression[]): this`
  * `addLatLng(latlng: LatLngExpression): this` — Appends a point.
  * `isEmpty(): Boolean`
  * `closestLayerPoint(p: Point): Point`
* **`L.polygon(latlngs: LatLngExpression[] | LatLngExpression[][] | LatLngExpression[][][], options?: PolylineOptions): Polygon`**
  * Same as Polyline with interior fill enabled by default and closed geometry.
* **`L.rectangle(bounds: LatLngBoundsExpression, options?: PolylineOptions): Rectangle`**
  * `setBounds(bounds: LatLngBoundsExpression): this`
* **`L.circle(latlng: LatLngExpression, options?: CircleOptions | radius: Number): Circle`**
  * Radius is defined in **meters** (scales with latitude/zoom on globe).
  * `getRadius(): Number` / `setRadius(radius: Number): this`
* **`L.circleMarker(latlng: LatLngExpression, options?: CircleMarkerOptions): CircleMarker`**
  * Radius is fixed in **screen pixels** (does not change size with zoom).
  * `getRadius(): Number` / `setRadius(radius: Number): this`
