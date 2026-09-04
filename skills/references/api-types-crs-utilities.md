# Leaflet API Reference — Types, CRS & DOM Utilities

This document details foundational geometric types (`LatLng`, `LatLngBounds`, `Point`, `Bounds`), Coordinate Reference Systems (`CRS`, `Projection`), and low-level DOM utilities (`DomUtil`, `DomEvent`, `Browser`).

---

## 1. Geometric Data Types

### `L.LatLng`
Represents a geographical point with a latitude and longitude.
* **Constructor:** `L.latLng(lat: Number, lng: Number, alt?: Number): LatLng`
* **Properties:** `lat: Number`, `lng: Number`, `alt?: Number`
* **Methods:**
  * `equals(otherLatLng: LatLngExpression, maxMargin?: Number): Boolean`
  * `toString(): String`
  * `distanceTo(otherLatLng: LatLngExpression): Number` — Returns distance in **meters** using Haversine formula on spherical earth.
  * `wrap(): LatLng` — Returns coordinates clamped between -180 and +180 longitude.
  * `toBounds(sizeInMeters: Number): LatLngBounds` — Creates bounding box surrounding point with given buffer.

### `L.LatLngBounds`
Represents a rectangular geographical area.
* **Constructor:** `L.latLngBounds(corner1: LatLngExpression, corner2: LatLngExpression): LatLngBounds` or `L.latLngBounds(latlngs: LatLngExpression[]): LatLngBounds`
* **Methods:**
  * `extend(latlng: LatLngExpression | LatLngBoundsExpression): this` — Extends bounds to contain point or other bounds.
  * `getCenter(): LatLng` — Returns geographical center.
  * `getSouthWest(): LatLng` / `getNorthEast(): LatLng` / `getNorthWest(): LatLng` / `getSouthEast(): LatLng`
  * `getWest(): Number` / `getSouth(): Number` / `getEast(): Number` / `getNorth(): Number`
  * `contains(otherBounds | latlng): Boolean`
  * `intersects(otherBounds: LatLngBoundsExpression): Boolean`
  * `overlaps(otherBounds: LatLngBoundsExpression): Boolean`
  * `toBBoxString(): String` — Returns `'southwest_lng,southwest_lat,northeast_lng,northeast_lat'`.
  * `isValid(): Boolean`

### `L.Point`
Represents a pixel point with x and y coordinates.
* **Constructor:** `L.point(x: Number, y: Number, round?: Boolean): Point`
* **Methods:** `add(point)`, `subtract(point)`, `multiplyBy(num)`, `divideBy(num)`, `scaleBy(point)`, `distanceTo(point)`, `round()`, `floor()`, `ceil()`.

### `L.Bounds`
Represents a pixel bounding box.
* **Constructor:** `L.bounds(corner1: PointExpression, corner2: PointExpression): Bounds`

---

## 2. Coordinate Reference Systems (CRS) & Projections

`L.CRS` defines how geographical coordinates are projected to pixel coordinates and map units.

### Available Predefined CRSs:
* **`L.CRS.EPSG3857` (Default)** — Spherical Mercator projection used by MapTiler, OpenStreetMap, Google Maps.
* **`L.CRS.EPSG4326`** — Plate Carree / Equirectangular projection (WGS84 lat/lng as Cartesian coordinates).
* **`L.CRS.EPSG3395`** — Elliptical Mercator projection.
* **`L.CRS.Simple`** — Flat Cartesian coordinate system `[x, y]` used for non-geographical indoor floorplans, gaming maps, and high-res image viewers.
  * Example: `const map = L.map('map', { crs: L.CRS.Simple, minZoom: -3 });`

---

## 3. DOM Utilities & Event Handlers

### `L.DomUtil` (DOM Manipulation Helpers)
* `L.DomUtil.get(id: String | HTMLElement): HTMLElement`
* `L.DomUtil.create(tagName: String, className?: String, container?: HTMLElement): HTMLElement`
* `L.DomUtil.addClass(el: HTMLElement, name: String)` / `removeClass(el, name)` / `hasClass(el, name)`
* `L.DomUtil.setPosition(el: HTMLElement, position: Point)` — Sets CSS translate / top-left positioning.
* `L.DomUtil.getPosition(el: HTMLElement): Point`
* `L.DomUtil.setOpacity(el: HTMLElement, opacity: Number)`

### `L.DomEvent` (Event Helper Functions)
* `L.DomEvent.on(el: HTMLElement, types: String, fn: Function, context?: Object)`
* `L.DomEvent.off(el: HTMLElement, types: String, fn: Function, context?: Object)`
* `L.DomEvent.stopPropagation(ev: Event)` — Prevents event bubbling.
* `L.DomEvent.preventDefault(ev: Event)`
* `L.DomEvent.stop(ev: Event)` — Combines `stopPropagation` and `preventDefault`.
* `L.DomEvent.disableClickPropagation(el: HTMLElement)` — **Critical**: prevents map click/drag when clicking inside custom UI controls or panels.
* `L.DomEvent.disableScrollPropagation(el: HTMLElement)` — Prevents mousewheel zooming map when scrolling a custom UI dropdown or list.

### `L.Browser` (Hardware & Runtime Flags)
* `L.Browser.ie` / `L.Browser.edge` / `L.Browser.webkit` / `L.Browser.gecko`
* `L.Browser.android` / `L.Browser.safari`
* `L.Browser.mobile`: `Boolean` — True on touchscreen mobile devices.
* `L.Browser.retina`: `Boolean` — True on high-resolution displays (`window.devicePixelRatio > 1`).
* `L.Browser.touch`: `Boolean` — True if touch events are supported.
