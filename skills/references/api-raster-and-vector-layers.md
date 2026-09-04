# Leaflet API Reference — Raster & Vector Layers

Source: https://leafletjs.com/reference.html#tilelayer

---

## 1. Raster Layers

### `L.TileLayer`
* `L.tileLayer(urlTemplate: String, options?: TileLayerOptions): TileLayer`
* **Options:** `minZoom`, `maxZoom`, `maxNativeZoom`, `minNativeZoom`, `subdomains` (`'abc'`), `errorTileUrl`, `zoomOffset`, `tms`, `zoomReverse`, `detectRetina`, `crossOrigin`, `referrerPolicy`.
* **Methods:** `setUrl(url: String, noRedraw?: Boolean): this`, `createTile(coords: Object, done: Function): HTMLElement`, `redraw(): this`, `getContainer(): HTMLElement`.

### `L.TileLayer.WMS`
* `L.tileLayer.wms(baseUrl: String, options: WMSOptions): TileLayer.WMS`
* **Options:** `layers` (required), `styles`, `format` (`'image/jpeg'`), `transparent` (`false`), `version` (`'1.1.1'`), `crs`, `uppercase`.
* **Methods:** `setParams(params: Object, noRedraw?: Boolean): this`.

### `L.ImageOverlay`, `L.VideoOverlay` & `L.SVGOverlay`
* **Image:** `L.imageOverlay(imageUrl: String, bounds: LatLngBoundsExpression, options?: ImageOverlayOptions)`
  * Methods: `setOpacity(opacity)`, `setUrl(url)`, `setBounds(bounds)`, `getElement(): HTMLImageElement`.
* **Video:** `L.videoOverlay(video: String | Array | HTMLVideoElement, bounds: LatLngBoundsExpression, options?: VideoOverlayOptions)`
  * Methods: `getElement(): HTMLVideoElement` (controls: `.play()`, `.pause()`).
* **SVG:** `L.svgOverlay(svg: SVGElement | String, bounds: LatLngBoundsExpression, options?: SVGOverlayOptions)`
  * Methods: `getElement(): SVGElement`.

---

## 2. Vector Layers (`L.Path`)

All vector layers inherit from `L.Path`.

### Path Options (`PathOptions`):
* `stroke` (`Boolean` = `true`): Draw outline stroke.
* `color` (`String` = `'#3388ff'`): Stroke color.
* `weight` (`Number` = `3`): Stroke width in pixels.
* `opacity` (`Number` = `1.0`): Stroke opacity.
* `lineCap` (`String` = `'round'`): `'butt' | 'round' | 'square'`.
* `lineJoin` (`String` = `'round'`): `'miter' | 'round' | 'bevel'`.
* `dashArray` (`String` = `null`): Stroke dash pattern (e.g. `'5, 10'`).
* `dashOffset` (`String` = `null`): Dash offset.
* `fill` (`Boolean` = `depends`): Fill the interior of path.
* `fillColor` (`String` = `color`): Interior fill color.
* `fillOpacity` (`Number` = `0.2`): Interior fill opacity.
* `fillRule` (`String` = `'evenodd'`): Interior fill calculation rule.
* `renderer` (`Renderer`): Dedicated `L.svg()` or `L.canvas()` instance.
* `className` (`String` = `null`): Custom SVG / canvas class name.

### Path Methods:
* `redraw(): this`
* `setStyle(style: PathOptions): this`
* `bringToFront(): this` / `bringToBack(): this`
* `getElement(): Element | undefined`

### Vector Classes:
* **`L.Polyline`**: `L.polyline(latlngs: LatLngExpression[] | LatLngExpression[][], options?: PolylineOptions)`
  * Methods: `getLatLngs()`, `setLatLngs()`, `addLatLng(latlng)`, `isEmpty()`, `closestLayerPoint(p)`, `toGeoJSON()`.
* **`L.Polygon`**: `L.polygon(latlngs: LatLngExpression[] | LatLngExpression[][], options?: PolylineOptions)`
* **`L.Rectangle`**: `L.rectangle(bounds: LatLngBoundsExpression, options?: PolylineOptions)`
  * Methods: `setBounds(bounds: LatLngBoundsExpression): this`.
* **`L.Circle`**: `L.circle(latlng: LatLngExpression, options?: CircleOptions | radius: Number)`
  * **Radius is in meters** (geographical scale).
  * Methods: `getRadius(): Number`, `setRadius(radius: Number): this`.
* **`L.CircleMarker`**: `L.circleMarker(latlng: LatLngExpression, options?: CircleMarkerOptions)`
  * **Radius is fixed in screen pixels** (constant size across zoom levels).
  * Methods: `getRadius(): Number`, `setRadius(radius: Number): this`.
* **`L.SVG` & `L.Canvas`**:
  * Vector rendering engines.
  * Factory: `L.svg(options?: RendererOptions)` / `L.canvas(options?: RendererOptions)`.
  * Option: `padding` (`Number` = `0.5`) — How much to extend bounds beyond viewport.
