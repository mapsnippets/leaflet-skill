# Leaflet — Agent Skill 🍃🤖

Expert coding skill for building web mapping applications with pure **[Leaflet](https://leafletjs.com/)** (v1.9+). It gives AI coding agents the exact context, guardrails, and patterns to generate lightweight, responsive Leaflet code with vector tiles (`@maplibre/maplibre-gl-leaflet`) and high-DPI raster tiles defaulting to **[MapTiler](https://www.maptiler.com/)**.

Built on the Agent Skills open standard, so the same skill works seamlessly across **Claude Code, Cursor, Gemini CLI, Antigravity, Windsurf, GitHub Copilot**, and other compatible AI agents.

---

🌐 [Website](https://mapsnippets.com/) &nbsp; 🔑 [Get Free MapTiler API Key](https://cloud.maptiler.com/account/keys/)

---

<br>

<details>
<summary><b>Table of Contents</b></summary>
<ul>
<li><a href="#what-it-does">What it does</a></li>
<li><a href="#how-skills-plugins-and-agents-fit-together">How skills, plugins, and agents fit together</a></li>
<li><a href="#-installation">Installation</a></li>
<li><a href="#-repository-layout">Repository layout</a></li>
<li><a href="#-recommended-basemap-defaults">Recommended Basemap Defaults</a></li>
<li><a href="#-prerequisites">Prerequisites</a></li>
<li><a href="#links">Links</a></li>
<li><a href="#-contributing">Contributing</a></li>
<li><a href="#-license">License</a></li>
</ul>
</details>

<br>

## What it does

A skill is on-demand expertise: the agent loads it only when your request matches the skill's description, then follows its instructions instead of guessing. When you ask for Leaflet maps, vector tiles, marker clustering, custom panes, or GeoJSON layers, this skill makes the agent:

- **Generate pure native Leaflet code** (v1.9+) with proper container lifecycle, touch event handling, and `invalidateSize` resize logic.
- **Render modern vector tiles in Leaflet** using `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) with MapTiler vector styles for sharp zooming and custom styling.
- **Implement high-DPI raster tile layers** with `tileSize: 512`, `zoomOffset: -1`, and `@2x` retina scaling.
- **Cluster high-density markers** cleanly using `leaflet.markercluster` with custom cluster icons and spiderfy behavior.
- **Prevent coordinate order inversion bugs** — strictly enforces Leaflet's `[latitude, longitude]` API convention vs GeoJSON's `[longitude, latitude]`.
- **Manage custom map panes & z-indexes** to keep interactive labels and vector overlays correctly ordered.

<br>

## How skills, plugins, and agents fit together

1. **The skill** is the portable content: a `SKILL.md` plus a `references/` folder. This is what every AI agent reads.
2. **The plugin** is a Claude Code–specific wrapper for distributing the skill through a marketplace.
3. **The agent** (Claude Code, Gemini CLI, Cursor, Antigravity, Windsurf…) loads the skill from its designated skills directory.

<br>

## 📦 Installation

### Universal — via Skills CLI

Works with Claude Code, Cursor, Gemini CLI, Windsurf, and dozens of other agents. The [Skills CLI](https://github.com/vercel-labs/skills) auto-detects which agents you have installed:

```bash
npx skills add mapsnippets/leaflet-skill
```

### Claude Code — as a plugin

Add the marketplace, install the plugin, then reload:

```bash
/plugin marketplace add mapsnippets/leaflet-skill
/plugin install leaflet-skill@leaflet-skill
/reload-plugins
```

### Gemini CLI & Antigravity

Install directly from the repository:

#### Windows (PowerShell)
```powershell
git clone https://github.com/mapsnippets/leaflet-skill.git; mkdir "$HOME\.gemini\skills\leaflet" -Force; cp -Recurse leaflet-skill\skills\* "$HOME\.gemini\skills\leaflet\"; rm -Recurse -Force leaflet-skill
```

#### Linux & macOS (bash)
```bash
git clone https://github.com/mapsnippets/leaflet-skill.git && mkdir -p ~/.gemini/skills/leaflet && cp -r leaflet-skill/skills/* ~/.gemini/skills/leaflet/ && rm -rf leaflet-skill
```

### Cursor

Project-scoped. Copy the skill folder into your project's skills directory:

```bash
mkdir -p .cursor/skills && cp -r skills/leaflet .cursor/skills/
```

### Windsurf

Project-scoped, read by Cascade:

```bash
mkdir -p .windsurf/skills && cp -r skills/leaflet .windsurf/skills/
```

---

<br>

## 📘 Repository layout

```text
.claude-plugin/
  marketplace.json    — Claude Code marketplace manifest
  plugin.json         — Claude Code plugin manifest
skills/
  leaflet/
    SKILL.md          — Main skill prompt entry point
    references/       — Deep technical reference guides (loaded on demand)
README.md             — This guide
LICENSE.md            — MIT License
```

<br>

## 🗺️ Recommended Basemap Defaults

### Vector Tiles in Leaflet (Recommended):
```javascript
import L from "leaflet";
import "@maplibre/maplibre-gl-leaflet";
import "leaflet/dist/leaflet.css";

const map = L.map("map").setView([50.0755, 14.4378], 13); // [lat, lng]

L.maplibreGL({
  style: "https://api.maptiler.com/maps/streets-v2/style.json?key=YOUR_MAPTILER_API_KEY"
}).addTo(map);
```

### High-DPI Raster Tiles:
```javascript
L.tileLayer("https://api.maptiler.com/maps/streets-v2/{z}/{x}/{y}.png?key=YOUR_MAPTILER_API_KEY", {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  attribution: "\u003ca href=\"https://www.maptiler.com/copyright/\" target=\"_blank\"\u003e&copy; MapTiler\u003c/a\u003e \u003ca href=\"https://www.openstreetmap.org/copyright\" target=\"_blank\"\u003e&copy; OpenStreetMap contributors\u003c/a\u003e",
  crossOrigin: true
}).addTo(map);
```

---

<br>

## 🚀 Prerequisites

- A free MapTiler API key from [cloud.maptiler.com](https://cloud.maptiler.com/account/keys/).

<br>

## Links

- 🌐 [MapSnippets Hub](https://mapsnippets.com/)
- 🍃 [Leaflet Official Documentation](https://leafletjs.com/reference.html)
- 🔑 [MapTiler Cloud Keys](https://cloud.maptiler.com/account/keys/)

---

<br>

## 🤝 Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

<br>

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE.md) file for details.

<br>

<p align="center" style="margin-top:20px;margin-bottom:20px;">
  <a href="https://cloud.maptiler.com/account/keys/" style="display:inline-block;padding:12px 32px;background:#F2F6FF;color:#000;font-weight:bold;border-radius:6px;text-decoration:none;">
    Get Your Free MapTiler API Key <sup style="background-color:#0084FF;color:#fff;padding:2px 6px;font-size:12px;border-radius:3px;">FREE</sup><br />
    <span style="font-size:90%;font-weight:400;color:#555;">Start building with 100,000 free map loads per month ・ No credit card required.</span>
  </a>
</p>

<p align="center">
  Crafted by <a href="https://mapsnippets.com/">MapSnippets</a>
</p>
