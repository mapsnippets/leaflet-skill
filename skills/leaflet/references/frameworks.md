# Leaflet Framework Integration

Same core pattern everywhere: create a map on mount, call `map.remove()` on unmount. Leaflet needs DOM — **client-side only**.

---

## React (react-leaflet v4)

The standard React wrapper for Leaflet. Uses declarative components.

```bash
npm install react-leaflet leaflet
npm install -D @types/leaflet  # TypeScript
```

### Basic Usage

```jsx
import { MapContainer, TileLayer, Marker, Popup, useMap } from 'react-leaflet';
import 'leaflet/dist/leaflet.css';

function MapView() {
  return (
    <MapContainer
      center={[50.1167, 14.4178]}
      zoom={12}
      style={{ height: '100vh', width: '100%' }}
    >
      <TileLayer
        url="https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY"
        attribution='&copy; <a href="https://www.maptiler.com/">MapTiler</a> &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap contributors</a>'
      />
      <Marker position={[50.1167, 14.4178]}>
        <Popup>Prague</Popup>
      </Marker>
    </MapContainer>
  );
}
```

### Fix Default Marker Icons (Vite/Webpack)

**This is required** when using bundlers. Leaflet's default icon paths break without it.

```jsx
// fix-leaflet-icons.js — import once in your app entry
import L from 'leaflet';
import markerIcon2x from 'leaflet/dist/images/marker-icon-2x.png';
import markerIcon from 'leaflet/dist/images/marker-icon.png';
import markerShadow from 'leaflet/dist/images/marker-shadow.png';

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconRetinaUrl: markerIcon2x,
  iconUrl: markerIcon,
  shadowUrl: markerShadow,
});
```

### Accessing the Map Instance

```jsx
function FlyToButton({ position }) {
  const map = useMap();

  return (
    <button onClick={() => map.flyTo(position, 14)}>
      Fly to Location
    </button>
  );
}

// Use inside MapContainer
<MapContainer center={[50, 14]} zoom={10}>
  <TileLayer url="..." />
  <FlyToButton position={[50.1167, 14.4178]} />
</MapContainer>
```

### GeoJSON Layer

```jsx
import { GeoJSON } from 'react-leaflet';

function GeoJSONMap({ data }) {
  return (
    <MapContainer center={[50, 14]} zoom={8}>
      <TileLayer url="https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY" />
      <GeoJSON
        data={data}
        style={(feature) => ({
          color: '#0066FF',
          weight: 2,
          fillOpacity: 0.3
        })}
        onEachFeature={(feature, layer) => {
          layer.bindPopup(feature.properties.name);
        }}
      />
    </MapContainer>
  );
}
```

### Vanilla Leaflet in React (without react-leaflet)

If you prefer imperative control:

```jsx
import { useEffect, useRef } from 'react';
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

function MapView() {
  const containerRef = useRef(null);
  const mapRef = useRef(null);

  useEffect(() => {
    if (mapRef.current) return; // Strict Mode guard

    mapRef.current = L.map(containerRef.current).setView([50.1167, 14.4178], 12);

    L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
      attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a>'
    }).addTo(mapRef.current);

    return () => {
      mapRef.current?.remove();
      mapRef.current = null;
    };
  }, []);

  return <div ref={containerRef} style={{ width: '100%', height: '400px' }} />;
}
```

**Strict Mode**: React 18 fires `useEffect` twice in dev. The `if (mapRef.current) return` guard + `null` reset in cleanup is essential.

### Next.js

Leaflet requires `window`/`document` — use dynamic import with SSR disabled:

```jsx
// components/Map.jsx
"use client";
import dynamic from 'next/dynamic';

const MapView = dynamic(() => import('./MapView'), { ssr: false });
export default MapView;
```

Env: `NEXT_PUBLIC_MAPTILER_KEY` for the API key.

---

## Vue 3 (@vue-leaflet/vue-leaflet)

```bash
npm install @vue-leaflet/vue-leaflet leaflet
```

### Basic Usage

```vue
<template>
  <l-map
    :zoom="12"
    :center="[50.1167, 14.4178]"
    style="height: 400px; width: 100%"
  >
    <l-tile-layer
      url="https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY"
      attribution="&copy; <a href='https://www.maptiler.com/'>MapTiler</a> &copy; <a href='https://www.openstreetmap.org/copyright'>OpenStreetMap contributors</a>"
    />
    <l-marker :lat-lng="[50.1167, 14.4178]">
      <l-popup>Prague</l-popup>
    </l-marker>
  </l-map>
</template>

<script setup>
import { LMap, LTileLayer, LMarker, LPopup } from '@vue-leaflet/vue-leaflet';
import 'leaflet/dist/leaflet.css';
</script>
```

### Available Components

| Component | Leaflet class |
|-----------|--------------|
| `LMap` | `L.map` |
| `LTileLayer` | `L.tileLayer` |
| `LMarker` | `L.marker` |
| `LPopup` | `L.popup` |
| `LTooltip` | `L.tooltip` |
| `LIcon` | `L.icon` |
| `LCircle` | `L.circle` |
| `LCircleMarker` | `L.circleMarker` |
| `LPolygon` | `L.polygon` |
| `LPolyline` | `L.polyline` |
| `LRectangle` | `L.rectangle` |
| `LGeoJson` | `L.geoJSON` |
| `LLayerGroup` | `L.layerGroup` |
| `LFeatureGroup` | `L.featureGroup` |
| `LControlLayers` | `L.control.layers` |
| `LControlScale` | `L.control.scale` |
| `LControlZoom` | `L.control.zoom` |
| `LControlAttribution` | `L.control.attribution` |

### Accessing Map Instance

```vue
<template>
  <l-map ref="mapRef" :zoom="12" :center="[50.1167, 14.4178]">
    <!-- ... -->
  </l-map>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const mapRef = ref(null);

onMounted(() => {
  // Access the Leaflet map instance
  const map = mapRef.value.leafletObject;
  map.on('click', (e) => console.log(e.latlng));
});
</script>
```

### Nuxt SSR

Wrap with `<ClientOnly>` or guard with `import.meta.client`:

```vue
<template>
  <ClientOnly>
    <l-map :zoom="12" :center="[50.1167, 14.4178]">
      <l-tile-layer url="..." />
    </l-map>
  </ClientOnly>
</template>
```

### Vanilla Leaflet in Vue (without vue-leaflet)

```vue
<template>
  <div ref="mapContainer" style="width: 100%; height: 400px" />
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

const mapContainer = ref(null);
let map = null;  // plain let, NOT ref() — Vue reactivity on map causes issues

onMounted(() => {
  map = L.map(mapContainer.value).setView([50.1167, 14.4178], 12);
  L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
    attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a>'
  }).addTo(map);
});

onUnmounted(() => { map?.remove(); map = null; });
</script>
```

---

## Angular

```bash
npm install leaflet
npm install -D @types/leaflet
```

```typescript
import { Component, ElementRef, OnInit, OnDestroy, ViewChild, AfterViewInit } from '@angular/core';
import * as L from 'leaflet';

@Component({
  selector: 'app-map',
  template: `<div #mapEl style="width: 100%; height: 400px"></div>`,
})
export class MapComponent implements AfterViewInit, OnDestroy {
  @ViewChild('mapEl', { static: true }) mapEl!: ElementRef;
  private map!: L.Map;

  ngAfterViewInit() {
    this.map = L.map(this.mapEl.nativeElement).setView([50.1167, 14.4178], 12);

    L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
      attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a>'
    }).addTo(this.map);
  }

  ngOnDestroy() {
    this.map?.remove();
  }
}
```

Add Leaflet CSS in `angular.json`:
```json
"styles": [
  "node_modules/leaflet/dist/leaflet.css",
  "src/styles.css"
]
```

**SSR (Angular Universal):** Guard with `isPlatformBrowser()` or use `afterNextRender()`.

---

## Svelte

```svelte
<script>
  import { onMount, onDestroy } from 'svelte';
  import L from 'leaflet';
  import 'leaflet/dist/leaflet.css';

  let mapContainer;
  let map;

  onMount(() => {
    map = L.map(mapContainer).setView([50.1167, 14.4178], 12);

    L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
      attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a>'
    }).addTo(map);
  });

  onDestroy(() => { map?.remove(); });
</script>

<div bind:this={mapContainer} style="width: 100%; height: 400px;" />
```

**SvelteKit SSR:** `onMount` only runs client-side, so the import is safe. For top-level dynamic imports, guard with `browser` from `$app/environment`.

---

## Cleanup Checklist

1. **Always call `map.remove()` on unmount** — prevents WebGL context leaks and memory growth
2. **Guard against double initialization** — React Strict Mode, HMR, and SPA navigation
3. **Fix marker icons** in bundled setups — see React section
4. **`height` is required** — container must have explicit CSS height
5. **SSR guard** — Leaflet needs `window`/`document`; use dynamic import or client-only wrappers

## Env Var Quick Reference

| Tool | Prefix | Access |
|------|--------|--------|
| Vite | `VITE_` | `import.meta.env.VITE_MAPTILER_KEY` |
| Next.js | `NEXT_PUBLIC_` | `process.env.NEXT_PUBLIC_MAPTILER_KEY` |
| CRA | `REACT_APP_` | `process.env.REACT_APP_MAPTILER_KEY` |
| Angular | — | `environment.ts` |
| SvelteKit | `PUBLIC_` | `$env/static/public` |
