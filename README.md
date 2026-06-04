# 🚖 Taxi Compass

> Street-hailable taxis and localized weather telemetry, made visible in a high-performance microcosm.

Taxi Compass is a lightweight, zero-dependency, single-file web application built for commuter utility in Singapore. It answers two immediate questions for a street-hailer standing on a sidewalk: *Where are the closest available vehicles right now, and is it about to pour rain on my current sector?*

---

## 🎯 Design Philosophy & Constraints

This project is built under the **"Getting Real" solopreneur philosophy** pioneered by DHH and Jason Fried. It prioritizes deliberate constraints, software independence, and subtraction over feature creep:

* **The Single-File Architecture:** The entire application resides natively in a single, portable HTML file. There are no `npm install` chains, no heavy Node.js frameworks, and no complex compilation or build pipelines. It can be hosted instantly on GitHub Pages, texted directly to a user, or opened in any modern browser.
* **Extreme Scope Control:** Instead of cloning an entire ride-sharing platform with accounts and history logs, it does one single thing flawlessly: visualizes immediate proximity metrics to empower on-the-spot transit decisions.

---

## ✨ Core Features

* **Dual Real-Time Data Ingestion:** Concurrently ingests live transport coordinates (of available taxis that you can hail) and rolling localized meteorological sector data from official government API endpoints.
* **Intelligent Proximity Engine:** Uses client-side spatial mathematics to isolate available vehicles within a strict 1.5km walking radius.
* **Cross-Stitched Regional Forecasts:** Pinpoints your precise weather planning sector and reflects current meteorological conditions via context-aware visual iconography (a.k.a Emojis ... ).
* **Mobile-First Layout Overhaul:** Fully responsive UI utilizing fluid CSS media breakpoints, thumb-optimized structural navigation buttons, and compressed dashboard metrics for small viewports.

---

## ⚡ Technical Architecture & Performance Optimizations

An emphasis was placed on keeping client-side compute times low, protecting frame rates, and minimizing battery strain on mobile hardware.

### 1. GPU-Accelerated Canvas Layer Rendering
When real-time fleet data spikes to over 4,000+ active taxis, forcing a mobile browser layout engine to track, style, and draw 4,000 individual HTML DOM elements (`<div>` emoji layers) creates severe panning and pinch-zoom latency. 
Taxi Compass handles this by passing the `{ preferCanvas: true }` initialization flag to Leaflet. All vehicles are mapped into a single, flat HTML5 graphics Canvas sheet as high-performance vector micro-markers (`L.circleMarker`), bringing layout overhead to zero and guaranteeing fluid framerates across all mobile browsers (Edge, Chrome, Brave, Firefox, Safari).

### 2. Client-Side Geolocation & Mathematical Proximity
Rather than deploying heavy spatial server queries, proximity telemetry is generated completely on the client device. Upon securing a hardware GPS lock via the Web Geolocation API, the script triggers a **Haversine Distance Engine** to calculate spherical geometry paths across coordinate arrays:

$$\text{Distance} = 2 R \cdot \arcsin\left(\sqrt{\sin^2\left(\frac{\Delta\text{lat}}{2}\right) + \cos(\text{lat}_1)\cos(\text{lat}_2)\sin^2\left(\frac{\Delta\text{lon}}{2}\right)}\right)$$

This engine checks thousands of incoming vehicle nodes and identifies the shortest vector path to the nearest GovTech meteorological sector center in milliseconds.

### 3. Asynchronous Resilience & API Rate Protections
* **Network Timeout Guard:** Asynchronous fetch protocols are governed by an `AbortController` network timeout connection routine set to 5 seconds. If the user experiences severe underground packet drops or transient cell-tower drops, the application cleanly terminates the thread and alerts the UI status tray rather than freezing execution threads.
* **Throttling Cooldown:** Manual map refreshes are protected by an intentional 60-second interactive state cooldown. This protects upstream public data endpoints from server spamming, ensures cache respect, and reinforces deliberate user interaction.

---

## 🛠️ Data Sources & Tech Stack

* **Map Interface:** [Leaflet.js (v1.9.4)](https://leafletjs.com/) via CDN
* **Map Typography & Styling:** [CartoDB Light Tiles](https://carto.com/basemaps/) (`© OpenStreetMap`, `© CARTO`)
* **Vehicle Location Data:** [Land Transport Authority (LTA) Datamall Live Taxi Availability API](https://beta.data.gov.sg/)
* **Weather Forecast Data:** [GovTech Singapore v2 Real-Time Meteorological API Stream](https://beta.data.gov.sg/)

---

## 🚀 Deployment & How to Run

Because the codebase honors zero-dependency portability constraints, local execution requires no tooling installation:

1. Clone or download this repository.
2. Open the index HTML file directly in any desktop or mobile web browser.

For public testing, the app is deployed continuously on GitHub Pages at:
👉 **[https://clw411.github.io/taxi-compass/]**
