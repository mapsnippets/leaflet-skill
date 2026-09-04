# Leaflet in Modern Frontend Frameworks

This guide details best practices and lifecycle management for Leaflet in React, Next.js, Vue, and Svelte.

---

## 1. React with `react-leaflet` (v4/v5)

`react-leaflet` provides declarative bindings for Leaflet.

### Installation:
```bash
npm install leaflet react-leaflet
npm install -D @types/leaflet
```

### Component Implementation:
```tsx
import React from "react";
import { MapContainer, TileLayer, Marker, Popup, useMap } from "react-leaflet";
import "leaflet/dist/leaflet.css";

// Helper component to access map instance
function MapUpdater({ center }: { center: [number, number] }) {
  const map = useMap();
  React.useEffect(() => {
    map.setView(center, map.getZoom());
  }, [center, map]);
  return null;
}

export default function MyMap() {
  const position: [number, number] = [50.0755, 14.4378];

  return (
    <div style={{ height: "100vh", width: "100%" }}>
      <MapContainer
        center={position}
        zoom={13}
        scrollWheelZoom={true}
        style={{ height: "100%", width: "100%" }}
      >
        <TileLayer
          url="https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY"
          tileSize={512}
          zoomOffset={-1}
          minZoom={1}
          maxZoom={20}
          attribution='&copy; <a href="https://www.maptiler.com/copyright/">MapTiler</a>'
        />
        <Marker position={position}>
          <Popup>Welcome to Prague!</Popup>
        </Marker>
        <MapUpdater center={position} />
      </MapContainer>
    </div>
  );
}
```

---

## 2. Next.js (App Router & Pages Router SSR Fix)

Leaflet relies directly on `window` and `document`. In Next.js, rendering Leaflet during Server-Side Rendering (SSR) causes `window is not defined`.

### Next.js App Router Solution (`next/dynamic`):
```tsx
// components/Map.tsx
"use client";
import { useEffect, useRef } from "react";
import L from "leaflet";
import "leaflet/dist/leaflet.css";

export default function Map() {
  const mapRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!mapRef.current) return;
    const map = L.map(mapRef.current).setView([50.0755, 14.4378], 13);

    L.tileLayer("https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY", {
      tileSize: 512,
      zoomOffset: -1
    }).addTo(map);

    return () => {
      map.remove();
    };
  }, []);

  return <div ref={mapRef} style={{ height: "100vh", width: "100%" }} />;
}

// app/page.tsx
import dynamic from "next/dynamic";

const DynamicMap = dynamic(() => import("../components/Map"), {
  ssr: false,
  loading: () => <div style={{ height: "100vh", display: "flex", alignItems: "center", justifyContent: "center" }}>Loading map...</div>
});

export default function Page() {
  return <DynamicMap />;
}
```

---

## 3. Svelte / SvelteKit

```html
<script>
  import { onMount, onDestroy } from "svelte";
  import "leaflet/dist/leaflet.css";

  let mapElement;
  let map;

  onMount(async () => {
    const L = await import("leaflet");
    map = L.map(mapElement).setView([50.0755, 14.4378], 13);

    L.tileLayer("https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY", {
      tileSize: 512,
      zoomOffset: -1
    }).addTo(map);
  });

  onDestroy(() => {
    if (map) map.remove();
  });
</script>

<div bind:this={mapElement} style="height: 100vh; width: 100%;"></div>
```
