# Leaflet API Reference — UI Layers

Source: https://leafletjs.com/reference.html#marker

---

## 1. `L.Marker`

### Creation:
`L.marker(latlng: LatLngExpression, options?: MarkerOptions): Marker`

### Options:
* `icon` (`Icon | DivIcon`): Marker icon instance.
* `keyboard` (`Boolean` = `true`): Tab focusable.
* `title` (`String` = `''`): Native browser tooltip text on hover.
* `alt` (`String` = `'Marker'`): Accessibility text.
* `zIndexOffset` (`Number` = `0`): Custom z-index offset.
* `opacity` (`Number` = `1.0`): Opacity (0.0 to 1.0).
* `riseOnHover` (`Boolean` = `false`): Increase z-index on mouseover.
* `riseOffset` (`Number` = `250`): Z-index bump amount on hover.
* `pane` (`String` = `'markerPane'`): Target pane.
* `shadowPane` (`String` = `'shadowPane'`): Target shadow pane.
* `draggable` (`Boolean` = `false`): Whether marker can be dragged with mouse/touch.
* `autoPan` (`Boolean` = `false`): Pan map when dragging near edge.
* `autoPanPadding` (`Point` = `[50, 50]`): Distance from viewport edge.
* `autoPanSpeed` (`Number` = `10`): Max autoPan speed in px/s.

### Methods:
* `getLatLng(): LatLng` / `setLatLng(latlng: LatLngExpression): this`
* `setIcon(icon: Icon | DivIcon): this`
* `setOpacity(opacity: Number): this`
* `setZIndexOffset(offset: Number): this`
* `toGeoJSON(precision?: Number): Object`
* `getElement(): HTMLElement | undefined`

---

## 2. `L.Popup`

### Creation:
`L.popup(options?: PopupOptions, source?: Layer): Popup`

### Options:
* `maxWidth` (`Number` = `300`): Max width in pixels.
* `minWidth` (`Number` = `50`): Min width in pixels.
* `maxHeight` (`Number` = `null`): Max height in pixels with scrolling.
* `autoPan` (`Boolean` = `true`): Pan map so popup is visible.
* `autoPanPaddingTopLeft` (`Point`): Padding for autoPan.
* `autoPanPaddingBottomRight` (`Point`): Padding for autoPan.
* `autoPanPadding` (`Point` = `[5, 5]`): Global autoPan padding.
* `keepInView` (`Boolean` = `false`): Pan map if user pans popup outside viewport.
* `closeButton` (`Boolean` = `true`): Display 'x' close button.
* `autoClose` (`Boolean` = `true`): Close when another popup opens.
* `closeOnEscapeKey` (`Boolean` = `true`): Close on Escape key press.
* `closeOnClick` (`Boolean`): Override map `closePopupOnClick`.
* `className` (`String` = `''`): Custom CSS class on container.
* `pane` (`String` = `'popupPane'`): Target pane.

### Methods:
* `getLatLng(): LatLng` / `setLatLng(latlng: LatLngExpression): this`
* `getContent(): String | HTMLElement | Function` / `setContent(htmlContent): this`
* `openOn(map: Map): this`
* `isOpen(): Boolean`
* `bringToFront(): this` / `bringToBack(): this`

---

## 3. `L.Tooltip`

### Creation:
`L.tooltip(options?: TooltipOptions, source?: Layer): Tooltip`

### Options:
* `pane` (`String` = `'tooltipPane'`): Target pane.
* `offset` (`Point` = `[0, 0]`): Pixel offset.
* `direction` (`'right' | 'left' | 'top' | 'bottom' | 'center' | 'auto'` = `'auto'`): Opening direction.
* `permanent` (`Boolean` = `false`): Open constantly (not just on hover).
* `sticky` (`Boolean` = `false`): Follow mouse cursor position.
* `interactive` (`Boolean` = `false`): Allow mouse events on tooltip.
* `opacity` (`Number` = `0.9`): Tooltip opacity.

---

## 4. `L.Icon`, `L.Icon.Default` & `L.DivIcon`

### `L.Icon`:
```javascript
const myIcon = L.icon({
  iconUrl: 'my-icon.png',
  iconRetinaUrl: 'my-icon@2x.png',
  iconSize: [38, 95],
  iconAnchor: [22, 94],
  popupAnchor: [-3, -76],
  shadowUrl: 'my-icon-shadow.png',
  shadowSize: [68, 95],
  shadowAnchor: [22, 94]
});
```

### `L.Icon.Default` (Bundler Asset Fix):
```javascript
delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png')
});
```

### `L.DivIcon` (Custom HTML / SVG Marker):
```javascript
const divIcon = L.divIcon({
  className: 'my-custom-div-icon',
  html: '<div style="background: red; border-radius: 50%; width: 12px; height: 12px;"></div>',
  iconSize: [12, 12],
  iconAnchor: [6, 6]
});
```
