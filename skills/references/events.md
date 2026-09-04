# Leaflet Events Reference

Complete reference for Leaflet map, layer, marker, and popup events.

> [Leaflet Events Docs](https://leafletjs.com/reference.html#map-event)

---

## Map Events

### View / Camera Events

| Event | Fires when | Handler arg |
|-------|-----------|-------------|
| `move` | Map center changes (any cause) | `Event` |
| `movestart` | Map movement begins | `Event` |
| `moveend` | Map movement ends | `Event` |
| `zoom` | Zoom level changes | `Event` |
| `zoomstart` | Zoom change begins | `Event` |
| `zoomend` | Zoom change ends | `Event` |
| `zoomlevelschange` | Zoom levels change (layer add/remove) | `Event` |
| `viewreset` | Map needs to redraw (zoom, CRS change) | `Event` |
| `resize` | Map container resized | `ResizeEvent` |

```javascript
map.on('moveend', () => {
  const center = map.getCenter();
  console.log(`Center: ${center.lat.toFixed(4)}, ${center.lng.toFixed(4)}`);
  console.log(`Zoom: ${map.getZoom()}`);
  console.log(`Bounds:`, map.getBounds());
});

map.on('zoomend', () => {
  console.log('Zoom:', map.getZoom());
});
```

### Interaction Events

| Event | Fires when | Handler arg |
|-------|-----------|-------------|
| `click` | Map clicked | `MouseEvent` |
| `dblclick` | Map double-clicked | `MouseEvent` |
| `contextmenu` | Right-click on map | `MouseEvent` |
| `mousedown` | Mouse button pressed | `MouseEvent` |
| `mouseup` | Mouse button released | `MouseEvent` |
| `mouseover` | Mouse enters map container | `MouseEvent` |
| `mouseout` | Mouse leaves map container | `MouseEvent` |
| `mousemove` | Mouse moves over map | `MouseEvent` |
| `keypress` | Key pressed while map focused | `KeyboardEvent` |
| `keydown` | Key down while map focused | `KeyboardEvent` |
| `keyup` | Key up while map focused | `KeyboardEvent` |
| `preclick` | Before click fires (useful for popup closing) | `MouseEvent` |

```javascript
// Click — most common interaction
map.on('click', (e) => {
  console.log('Clicked at:', e.latlng.lat, e.latlng.lng);
  console.log('Container point:', e.containerPoint);
  console.log('Layer point:', e.layerPoint);
});

// Right-click context menu
map.on('contextmenu', (e) => {
  L.popup()
    .setLatLng(e.latlng)
    .setContent(`Coordinates: ${e.latlng.lat.toFixed(5)}, ${e.latlng.lng.toFixed(5)}`)
    .openOn(map);
});

// Track cursor position
map.on('mousemove', (e) => {
  document.getElementById('coords').textContent =
    `${e.latlng.lat.toFixed(4)}, ${e.latlng.lng.toFixed(4)}`;
});
```

### Touch Events

| Event | Fires when | Handler arg |
|-------|-----------|-------------|
| `touchstart` | Touch begins | `TouchEvent` |
| `touchend` | Touch ends | `TouchEvent` |
| `touchmove` | Touch moves | `TouchEvent` |
| `touchcancel` | Touch cancelled | `TouchEvent` |

### Layer Events

| Event | Fires when | Handler arg |
|-------|-----------|-------------|
| `layeradd` | Layer added to map | `LayerEvent` |
| `layerremove` | Layer removed from map | `LayerEvent` |
| `baselayerchange` | Base layer changed via layer control | `LayerEvent` |
| `overlayadd` | Overlay added via layer control | `LayerEvent` |
| `overlayremove` | Overlay removed via layer control | `LayerEvent` |

```javascript
map.on('baselayerchange', (e) => {
  console.log('Switched to:', e.name);
});

map.on('overlayadd', (e) => {
  console.log('Overlay shown:', e.name);
});
```

### Other Map Events

| Event | Fires when |
|-------|-----------|
| `load` | Map initialized (view set, tiles loading) |
| `unload` | Map destroyed via `remove()` |
| `locationfound` | Geolocation success (`map.locate()`) |
| `locationerror` | Geolocation error |
| `popupopen` | Any popup opens on map |
| `popupclose` | Any popup closes on map |
| `tooltipopen` | Any tooltip opens |
| `tooltipclose` | Any tooltip closes |

```javascript
// Geolocation
map.on('locationfound', (e) => {
  L.marker(e.latlng).addTo(map)
    .bindPopup(`You are within ${e.accuracy} meters`);
  L.circle(e.latlng, { radius: e.accuracy }).addTo(map);
});

map.on('locationerror', (e) => {
  alert('Location error: ' + e.message);
});

map.locate({ setView: true, maxZoom: 16 });
```

---

## Marker Events

```javascript
const marker = L.marker([50.1167, 14.4178]).addTo(map);

marker.on('click', (e) => {
  console.log('Marker clicked');
});

marker.on('dragend', (e) => {
  const pos = e.target.getLatLng();
  console.log('Dragged to:', pos.lat, pos.lng);
});

marker.on('mouseover', () => {
  marker.openPopup();
});

marker.on('mouseout', () => {
  marker.closePopup();
});
```

| Event | Fires when |
|-------|-----------|
| `click` | Marker clicked |
| `dblclick` | Marker double-clicked |
| `mousedown` | Mouse pressed on marker |
| `mouseover` | Mouse enters marker |
| `mouseout` | Mouse leaves marker |
| `contextmenu` | Right-click on marker |
| `dragstart` | Drag begins (draggable markers) |
| `drag` | During drag |
| `dragend` | Drag ends |
| `move` | Marker moved (programmatic or drag) |
| `add` | Marker added to map |
| `remove` | Marker removed from map |
| `popupopen` | Marker's popup opens |
| `popupclose` | Marker's popup closes |

### Draggable Markers

```javascript
const marker = L.marker([50.1167, 14.4178], { draggable: true }).addTo(map);

marker.on('dragend', async (e) => {
  const { lat, lng } = e.target.getLatLng();

  // Reverse geocode the new position using MapTiler
  const response = await fetch(
    `https://api.maptiler.com/geocoding/${lng},${lat}.json?key=YOUR_MAPTILER_KEY`
  );
  const data = await response.json();

  if (data.features.length > 0) {
    marker.setPopupContent(data.features[0].place_name).openPopup();
  }
});
```

---

## GeoJSON Layer Events

```javascript
const layer = L.geoJSON(data, {
  onEachFeature: (feature, layer) => {
    // Events per feature
    layer.on({
      click: (e) => {
        console.log('Feature:', feature.properties);
      },
      mouseover: (e) => {
        e.target.setStyle({
          weight: 4,
          color: '#666',
          fillOpacity: 0.9
        });
        e.target.bringToFront();
      },
      mouseout: (e) => {
        // Reset to original style
        layer.resetStyle(e.target);  // requires reference to parent L.geoJSON
      }
    });
  }
}).addTo(map);
```

---

## Popup Events

```javascript
const popup = L.popup()
  .setLatLng([50.1167, 14.4178])
  .setContent('Hello!');

popup.on('add', () => console.log('Popup added to map'));
popup.on('remove', () => console.log('Popup removed from map'));

// Global map-level popup events
map.on('popupopen', (e) => {
  console.log('Popup opened:', e.popup.getContent());
});

map.on('popupclose', (e) => {
  console.log('Popup closed');
});
```

---

## Tile Layer Events

```javascript
const tiles = L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY');

tiles.on('loading', () => console.log('Tiles loading...'));
tiles.on('load', () => console.log('All tiles loaded'));
tiles.on('tileload', (e) => console.log('Tile loaded:', e.coords));
tiles.on('tileerror', (e) => console.error('Tile error:', e.coords, e.error));
tiles.on('tileunload', (e) => console.log('Tile unloaded:', e.coords));
```

---

## MouseEvent Object

All mouse/touch events provide:

| Property | Type | Description |
|----------|------|-------------|
| `latlng` | LatLng | Geographic coordinates of click |
| `layerPoint` | Point | Pixel coords relative to map origin |
| `containerPoint` | Point | Pixel coords relative to container |
| `originalEvent` | DOMEvent | Original browser event |

---

## Event Methods

```javascript
// Add listener
map.on('click', handler);

// Remove listener
map.off('click', handler);

// One-time listener
map.once('moveend', () => {
  console.log('First move completed');
});

// Fire custom event
map.fire('myevent', { data: 'hello' });
map.on('myevent', (e) => console.log(e.data));

// Check if has listener
map.listens('click');  // boolean
```
