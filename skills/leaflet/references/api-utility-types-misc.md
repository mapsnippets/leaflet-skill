# Leaflet API Reference — Basic Types, Utilities, DOM & Misc

Source: https://leafletjs.com/reference.html#latlng

---

## 1. Basic Types

### `L.LatLng`
* `L.latLng(latitude: Number, longitude: Number, altitude?: Number): LatLng`
* Properties: `lat`, `lng`, `alt`.
* Methods:
  * `equals(otherLatLng, maxMargin?: Number): Boolean`
  * `toString(): String`
  * `distanceTo(otherLatLng: LatLngExpression): Number` — Haversine distance in meters.
  * `wrap(): LatLng` — Clamps longitude between -180 and +180.
  * `toBounds(sizeInMeters: Number): LatLngBounds`

### `L.LatLngBounds`
* `L.latLngBounds(corner1: LatLngExpression, corner2: LatLngExpression): LatLngBounds`
* Methods:
  * `extend(latlng | bounds): this`
  * `getCenter(): LatLng`
  * `getSouthWest(): LatLng` / `getNorthEast(): LatLng` / `getNorthWest(): LatLng` / `getSouthEast(): LatLng`
  * `getWest(): Number` / `getSouth(): Number` / `getEast(): Number` / `getNorth(): Number`
  * `contains(otherBounds | latlng): Boolean`
  * `intersects(otherBounds): Boolean`
  * `overlaps(otherBounds): Boolean`
  * `toBBoxString(): String` — Returns `'sw_lng,sw_lat,ne_lng,ne_lat'`.
  * `isValid(): Boolean`

### `L.Point`
* `L.point(x: Number, y: Number, round?: Boolean): Point`
* Methods: `add(p)`, `subtract(p)`, `multiplyBy(num)`, `divideBy(num)`, `scaleBy(p)`, `unscaleBy(p)`, `round()`, `floor()`, `ceil()`, `trunc()`, `distanceTo(p)`, `equals(p)`, `contains(p)`, `toString()`.

### `L.Bounds`
* `L.bounds(corner1: PointExpression, corner2: PointExpression): Bounds`
* Methods: `extend(p)`, `getCenter(round?: Boolean)`, `getBottomLeft()`, `getTopRight()`, `getTopLeft()`, `getBottomRight()`, `getSize()`, `contains(b | p)`, `intersects(b)`, `overlaps(b)`, `isValid()`.

---

## 2. Utility & Algorithms

### `L.Util`
* `L.Util.extend(dest, src1, ...)` — Shallow object copy (like `Object.assign`).
* `L.Util.create(proto, properties)` — `Object.create` polyfill.
* `L.Util.bind(fn, obj)` — `Function.prototype.bind`.
* `L.Util.stamp(obj): Number` — Unique ID generator for objects.
* `L.Util.throttle(fn, time, context)` — Throttle execution.
* `L.Util.wrapNum(x, range, includeMax)` — Wraps number within range (e.g. longitude wrap).
* `L.Util.formatNum(num, digits)` — Rounds number to decimal precision.
* `L.Util.splitWords(str): String[]`
* `L.Util.setOptions(obj, options): Object`
* `L.Util.getParamString(obj, existingUrl?, uppercase?)`
* `L.Util.template(str, data): String` — Replaces `{variable}` tokens in string.
* `L.Util.requestAnimFrame(fn, context, immediate)` / `cancelAnimFrame(id)`

### `L.Transformation`
* `L.transformation(a: Number, b: Number, c: Number, d: Number): Transformation`
* `transform(point: Point, scale?: Number): Point` (`x_ = a * x + b`, `y_ = c * y + d`).
* `untransform(point: Point, scale?: Number): Point`

### `L.LineUtil` & `L.PolyUtil`
* `L.LineUtil.simplify(points: Point[], tolerance: Number): Point[]` — Douglas-Peucker line simplification.
* `L.LineUtil.pointToSegmentDistance(p, p1, p2): Number`
* `L.LineUtil.closestPointOnSegment(p, p1, p2): Point`
* `L.LineUtil.clipSegment(a, b, bounds, useLastCode, round)` — Cohen-Sutherland line clipping.
* `L.PolyUtil.clipPolygon(points: Point[], bounds: Bounds, round?: Boolean): Point[]` — Sutherland-Hodgman clipping.
* `L.PolyUtil.polygonCenter(latlngs, crs): LatLng` — Computes polygon center.

---

## 3. DOM Utility & Events

### `L.DomEvent`
* `on(el, types, fn, context)` / `off(el, types, fn, context)`
* `stopPropagation(e: Event)` — Stops bubbling.
* `preventDefault(e: Event)`
* `stop(e: Event)` — Stops propagation and prevents default.
* `disableClickPropagation(el: HTMLElement)` — **Critical**: stops map click when clicking UI element.
* `disableScrollPropagation(el: HTMLElement)` — Stops map scroll-zoom when scrolling custom UI list.
* `getMousePosition(e: MouseEvent, container?: HTMLElement): Point`
* `getWheelDelta(e: Event): Number`

### `L.DomUtil`
* `get(id: String | HTMLElement): HTMLElement`
* `getStyle(el: HTMLElement, styleAttrib: String): String`
* `create(tagName: String, className?: String, container?: HTMLElement): HTMLElement`
* `remove(el: HTMLElement)`
* `empty(el: HTMLElement)`
* `toFront(el: HTMLElement)` / `toBack(el: HTMLElement)`
* `hasClass(el, name)` / `addClass(el, name)` / `removeClass(el, name)` / `setClass(el, name)` / `getClass(el)`
* `setOpacity(el, opacity)`
* `setPosition(el: HTMLElement, position: Point)` / `getPosition(el: HTMLElement): Point`
* `disableTextSelection()` / `enableTextSelection()`
* `disableImageDrag()` / `enableImageDrag()`
* `preventOutline(el)` / `restoreOutline()`

### `L.Browser`
Hardware & browser feature detection flags:
* `L.Browser.mobile`, `L.Browser.retina`, `L.Browser.touch`, `L.Browser.pointer`
* `L.Browser.chrome`, `L.Browser.safari`, `L.Browser.gecko`, `L.Browser.edge`, `L.Browser.ie`

---

## 4. Coordinate Reference Systems (CRS) & Projections

### `L.CRS`
* `L.CRS.EPSG3857` (Default Spherical Mercator)
* `L.CRS.EPSG4326` (WGS84 Equirectangular)
* `L.CRS.EPSG3395` (World Mercator)
* `L.CRS.Simple` (Flat Cartesian for floorplans, game maps)
* **Methods on CRS:**
  * `latLngToPoint(latlng: LatLng, zoom: Number): Point`
  * `pointToLatLng(point: Point, zoom: Number): LatLng`
  * `project(latlng: LatLng): Point`
  * `unproject(point: Point): LatLng`
  * `scale(zoom: Number): Number`
  * `zoom(scale: Number): Number`
  * `distance(latlng1: LatLng, latlng2: LatLng): Number`

---

## 5. Event Objects Reference

* **`Event`**: `type`, `target`, `sourceTarget`.
* **`KeyboardEvent`**: `originalEvent: KeyboardEvent`.
* **`MouseEvent`**: `latlng: LatLng`, `layerPoint: Point`, `containerPoint: Point`, `originalEvent: MouseEvent`.
* **`LocationEvent`**: `latlng: LatLng`, `bounds: LatLngBounds`, `accuracy: Number`, `altitude: Number`, `speed: Number`, `heading: Number`, `timestamp: Number`.
* **`ErrorEvent`**: `message: String`, `code: Number`.
* **`LayerEvent`**: `layer: Layer`.
* **`LayersControlEvent`**: `layer: Layer`, `name: String`.
* **`TileEvent`**: `tile: HTMLElement`, `coords: Point`.
* **`TileErrorEvent`**: `tile: HTMLElement`, `coords: Point`, `error: Error`.
* **`ResizeEvent`**: `oldSize: Point`, `newSize: Point`.
* **`PopupEvent`**: `popup: Popup`.
* **`TooltipEvent`**: `tooltip: Tooltip`.
* **`DragEndEvent`**: `distance: Number`.
* **`ZoomAnimEvent`**: `center: LatLng`, `zoom: Number`, `noUpdate: Boolean`.

---

## 6. Global Switches & Misc

* `L.noConflict(): typeof L` — Relinquishes Leaflet's control of the global `L` variable.
* `L.version`: `String` — Current Leaflet version (e.g. `'1.9.4'`).
