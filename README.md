# Territorial.io — World Map Edition

![HTML5 Canvas](https://img.shields.io/badge/HTML5-Canvas_2D-E34F26?style=flat-square&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Dependencies](https://img.shields.io/badge/Dependencies-Zero-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

A browser-based single-file strategy game inspired by **Territorial.io**, engineered with native HTML5 Canvas 2D and vanilla JavaScript. Features procedural pixelated map generators, high-precision floating-point economic calculations, and multi-tiered tactical bot AI.

---

## Features Overview

* **Procedural Map Engine:** Generates $400 \times 250$ pixelated maps ($100,000$ grid nodes) with CSS scaling to $900 \times 562$. Features World Continents (Americas, Eurasia, Africa, Australia), Archipelagos, and Pangaea supercontinents.
* **Precision Floating-Point Economics:** 64-bit float math tracking troop growth, dynamic decay rates, distance deployment taxes, and exponential density penalties.
* **Strengthened Bot AI Engine:** Four difficulty modes supporting troop interest hoarding, weak-border exploitation, amphibious naval operations, and dynamic gang-up strikes on map leaders.
* **Integrated Interactive Lobby:** Custom setup card configuring player handles, bot density ($8$ to $64$ AI opponents), difficulty modes, and map generation presets.
* **Zero Dependencies:** Pure HTML, CSS, and JS contained within a single `index.html` file—no frameworks, external assets, or build pipelines required.

---

## Game Engine & Mathematical Models

### 1. High-Precision Interest Rate Curve
Economic cycles tick every $1.25\text{s}$ ($\Delta t \cdot 0.8$). Base interest decays exponentially per game cycle:

$$\text{baseRate} = 0.0815 \cdot e^{-\text{cycle} \cdot 0.0035} + 0.0125$$

If troop density ($\text{Density} = \frac{\text{Troops}}{\text{Land}}$) exceeds $100.0 \text{ troops/pixel}$, an exponential penalty is applied:

$$\text{penaltyRatio} = \left(\frac{\max(0.0,\, \text{Density} - 100.0)}{50.0}\right)^{1.4}$$

$$\text{interestRate} = \max\left(0.0,\, \text{baseRate} \cdot (1.0 - \text{penaltyRatio})\right)$$

* **Density Hard Cap:** Capped at $150.0 \text{ troops/pixel}$ ($\text{land} \times 150.0$). At $150.0$ density, interest drops to $0\%$.
* **Territory Yield:** Adds a static $+1.0 \text{ troop}$ per land pixel each cycle.
* **Starting Balance:** Every player spawns with $500.0 \text{ troops}$.

### 2. Conquest Cost & Distance Tax
Launching an expansion deducts an upfront **3.25% deployment tax** from the selected attack budget. Pixel conquest cost depends on target ownership:

$$\text{Effective Attack Pool} = \text{Attack Pool} \cdot (1.0 - 0.0325)$$

* **Neutral Land:** Costs a flat $2.0 \text{ troops/pixel}$.
* **Hostile Land:** Costs scale exponentially with defender density ($\text{defDensity} = \frac{\text{Defender Troops}}{\text{Defender Land}}$):

$$\text{Pixel Cost} = 2.0 + 1.85 \cdot (\text{defDensity})^{1.12}$$

### 3. Naval Expeditions
* **Activation Threshold:** Requires a minimum of $400.0 \text{ troops}$.
* **Payload Size:** Dispatches $15\%$ of active troop balance.
* **Traversal Speed:** Moves at $2.8 \text{ units/frame}$ across water coordinates, establishing a $3 \times 3$ pixel nucleus upon arrival.

---

## AI Difficulty Matrix

| Difficulty | Interest Hoarding Phase | Leader Targeted Strikes | Naval Invasions | Tactical Profile |
| :--- | :--- | :--- | :--- | :--- |
| **EASY** | Disabled | $0\%$ | Disabled | Expands into random adjacent neutral pixels ($15\% - 35\%$ pool). |
| **MEDIUM** | Disabled | $0\%$ | Disabled | Steady expansion with balanced troop allocations. |
| **HARD** | Density $< 85.0$ and Troops $< 5000.0$ | $0\%$ | $12\%$ Chance | Hoards balance during high-yield interest cycles; launches sea flanks. |
| **HARDCORE** | Density $< 85.0$ and Troops $< 5000.0$ | $45\%$ Chance | $12\%$ Chance | Hoards interest, launches heavy $35\%$ targeted strikes on players with $>15\%$ map area, and executes sea flanks. |

---

## Lobby Configuration Options

| Option ID | Settings | Default | Description |
| :--- | :--- | :--- | :--- |
| **Player Name** | Text String ($\le 15$ chars) | `Conqueror` | Display name rendered on HUD and match leaderboard. |
| **Bot Competitors** | `8`, `16`, `32`, `64` | `16 Bots` | Total count of AI opponents spawned across neutral land. |
| **AI Intelligence** | `EASY`, `MEDIUM`, `HARD`, `HARDCORE` | `HARDCORE` | Sets AI interest hoarding, leader targeting, and naval rules. |
| **Map Generator** | `WORLD`, `ARCHIPELAGO`, `PANGAEA` | `WORLD` | Procedural land distribution algorithm. |

---

## User Interface & Controls

### Controls
* **Left Click (Spawn Phase):** Click any grey neutral pixel to establish a $3 \times 3$ capital territory.
* **Attack Ratio Slider:** Sets total deployment budget between $1\%$ and $100\%$ (default: $20\%$).
* **ATTACK Button:** Dispatches troops from border frontier pixels.
* **NAVAL BOAT Button:** Toggles amphibious deployment mode. Click any land area across water to launch a boat.
* **LOBBY Button:** Resets active match state and returns to the main lobby setup screen.

### HUD Indicators
* **Balance:** Current floating-point troop balance (formatted with `k` or `M` suffixes).
* **Land:** Total pixel land area held and percentage of world land pixels.
* **Interest Rate:** Active cycle interest yield (displays red when density penalty triggers above $100.0$).
* **Troop Density:** Current troops-per-pixel ratio relative to the $150.0$ hard cap.

---

## Quick Start Guide

### Local Setup
1. Clone this repository:
   ```bash
   git clone [https://github.com/your-username/territorial-world-edition.git](https://github.com/your-username/territorial-world-edition.git)
