# The-roadsters
# ⛩️ Tsukuyomi Watch (月読見張): The Samurai's Celestial Guide

> **PECathon 2026 — Problem Statement PS-002**  
> *Read the heavens like a warrior. Find the clearest skies, track cosmic events, and restore the cosmos with faithful scientific AI.*

---

## 🌙 Overview

In feudal Japan, samurai and navigators read the moon phases, falling stars, and celestial alignments to time their campaigns and journeys. Today, stargazers and astrophotographers face cloud cover, urban light pollution, scattered orbital telemetry, and sensor/atmospheric noise in deep-sky captures.

**Tsukuyomi Watch** is a full-stack astronomy and celestial intelligence platform wrapped in an Edo-period ink-wash and cyber-mystic interface:
1. **The Watchtower (`/watchtower`)** — Real-time stargazing spot finder, Night Honor Score calculator, parchment-styled Leaflet map, and interactive D3.js celestial hemisphere dome with Japanese star lore.
2. **The Celestial Scroll (`/scroll`)** — Live SGP4 ISS pass predictor, US Naval Observatory + SunCalc lunar phase tracker, meteor shower & eclipse ledger, and traditional lunar calendar filtering with horizon alerts.
3. **The Spirit Lens (`/lens`)** — Non-hallucinatory astrophotography restoration pipeline (Wavelet/NLM denoising + Richardson-Lucy deconvolution) paired with automated DAOStarFinder/SEP celestial source extraction.
4. **The Dojo Dashboard (`/dojo`)** — Interactive split-screen before/after comparator, deep zoom, and astrometric bounding/centroid overlays for detected stars and galaxies.
5. **Way of the Watcher (`/honor`)** — Gamified Honor Points progression system for logging celestial observations.

---

## ⚔️ Key Technical Architecture

### 1. The Watchtower: Topocentric Weather, AQI & Sanctuary Discovery
* **Real-Time Telemetry**: Queries Open-Meteo Forecast (`cloud_cover`, `relative_humidity_2m`, `visibility`, `temperature_2m`, `wind_speed_10m`) and Open-Meteo Air Quality (`us_aqi`, `pm2_5`, `pm10`).
* **Resilient Dark-Sky Discovery**: Queries OpenStreetMap Overpass mirrors for nearby mountain peaks, viewpoints, nature reserves, and observatories, backed by a curated Shivalik/Himalayan foothills sanctuary fallback (Morni Hills, Kasauli, Chail, Sukhna Wildlife Sanctuary, Siswan Dam) so demos never fail on rate-limited Wi-Fi.
* **Interactive Celestial Dome (`CelestialMapWidget.tsx`)**: Computes exact Local Sidereal Time (LST) and projects constellations (`Hokuto Shichisei`, `Tsuzumi-boshi`, `Subaru`, `Orihime & Hikoboshi`) onto an interactive stereographic D3 canvas with dynamic cloud-cover attenuation and AQI haze simulation.

### 2. The Celestial Scroll: Precision Ephemeris & SGP4 Orbital Mechanics
* **Multi-Source ISS TLE Engine**: Fetches Two-Line Element sets for NORAD `25544` across a 3-tier fallback (`wheretheiss.at` → `celestrak.org` → `tle.ivanstanojevic.me`) with 3-hour caching and 7-day stale resilience.
* **Topocentric Pass Prediction**: Propagates orbits via `satellite.js` (SGP4) and cross-checks observer solar depression via `SunCalc` to filter for genuinely visible naked-eye passes.
* **USNO + SunCalc Lunar Hybrid**: Combines `aa.usno.navy.mil` rise/set/transit data with real-time `SunCalc` parallactic angle, limb illumination, and topocentric distance.

### 3. The Spirit Lens & Dojo: Faithful Deconvolution (Zero Hallucination)
* **True-to-Signal Restoration**: Unlike generative diffusion models that invent fake stars, our pipeline preserves photometric integrity using multi-scale denoising and **Richardson-Lucy PSF deconvolution** powered by `sharp` and custom numerical kernels.
* **Automated Source Cataloging**: Detects stellar point sources and extended galactic/nebular structures, reporting centroid coordinates, flux/brightness, FWHM/size, and classification overlays in the Dojo Dashboard.

---

## 🛠️ Tech Stack

* **Frontend**: React 19, TypeScript, Tailwind CSS v4, D3.js (celestial dome projection), Leaflet (parchment cartography), Motion, Lucide Icons, Web Audio API (`soundEngine.ts` for Koto & Suzu synthesis).
* **Backend**: Node.js, Express, `tsx`, `sharp` (high-speed image processing), `satellite.js` (SGP4 orbital mechanics), `suncalc`, `multer`.
* **Storage**: Browser `IndexedDB` (`imageStore.ts`) for persisting high-resolution restored astro captures across sessions.

---

## 🚀 Local Setup & Execution

### Prerequisites
* **Node.js** `>= 20.19.0` (or Bun)

### Installation
```bash
# 1. Install dependencies
npm install

# 2. Configure environment variables
cp .env.example .env
# Optional: Add GEMINI_API_KEY in .env if using AI commentary features
