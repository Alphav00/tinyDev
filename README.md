# 🕹️ tinyDev: The DJC³ Cartridge
**A 16-Bit Triadic AI Reasoning Deck (Zero Dependencies)**

![Status: V1](https://img.shields.io/badge/STATUS-V1_CARTRIDGE-00d4ff?style=for-the-badge)
![Dependencies](https://img.shields.io/badge/DEPENDENCIES-ZERO-39ff4e?style=for-the-badge)
![Platform](https://img.shields.io/badge/PLATFORM-MOBILE_BROWSER-e040fb?style=for-the-badge)

> *We didn’t want a bloated cloud dashboard. We wanted a Super Nintendo that writes code.*

**tinyDev** is a self-contained, single-file HTML application that turns your mobile browser into a multi-agent AI coprocessor. It uses a "Triadic Reasoning" pipeline (DJC³) to break down prompts, synthesize logic, and generate actionable output—all wrapped in a tactile, 16-bit retro interface.

There is no Webpack. No `node_modules`. No backend server. Just math, HTML Canvas, and bare-metal API calls.

## 🌟 The Features (Mode 7)

*   **The DJC³ Pipeline:** Three distinct agent personas that run sequentially to solve complex problems:
    *   **[F1] DIMITRI (Logic):** Analyzes data with cold clarity.
    *   **[F2] JESSICA (Creative):** Synthesizes ideas with lateral thinking.
    *   **[F3] CHI (Action):** Integrates both into a final, grounded output.
*   **Procedural Pixel Art:** All agent sprites and animations are generated procedurally via HTML Canvas coordinates. Zero external image requests.
*   **Naked API Streaming:** Connects directly to Anthropic, OpenRouter, or Gemini using native browser `fetch` and Server-Sent Events (SSE).
*   **Legible Chaos:** When an API fails or an agent hallucinates, the UI triggers a localized CRT glitch and spawns "fire" sprites you must physically extinguish.
*   **Cozy Productivity:** Agents fall asleep ("Zzz") if left idle too long and drink coffee when awake.

## 🪙 INSERT COIN (Quick Start)

You don't need to install anything.

1. Clone this repository or simply download `tinyDev-mobile-v1.html`.
2. Open it in any mobile or desktop web browser.
3. Tap the **[ ☰ ]** menu icon in the top left.
4. Paste your API key (OpenRouter, Anthropic, or Gemini).
5. Type a prompt and press **[ ▶ RUN ]**.

To run the full Triadic Pipeline, select **[ ⚡ COUNCIL ]** mode and watch the data flow up the tower.

## 📖 The Architecture (Webbook)

How do you fit a multi-agent AI orchestration platform into a single HTML file without it collapsing? Read the design manual:

*   [PILLAR I: Tactile Nostalgia & Procedural Pixels](./docs/01-TACTILE-NOSTALGIA.md)
*[PILLAR II: Bare-Metal State Machines](./docs/02-STATE-MACHINES.md)
*[PILLAR III: Legible Chaos (Designing for Failure)](./docs/03-LEGIBLE-CHAOS.md)

## 🚀 The Roadmap (V2: PIXEL·OS)

Currently, tinyDev is a pure UI/Reasoning cartridge. The future roadmap involves bridging this interface directly into a local Android development environment:
- [ ] **AVF Linux Integration:** Moving API keys and logs to a local SQLite database via Termux.
- [ ] **Chrome CDP Auto-Player:** Allowing the `CHI` agent to physically click buttons and test web apps on the device via Chrome DevTools Protocol.
- [ ] **Hardware Polling:** Adding real-time CPU/RAM stats to the retro dashboard.

---
*Built in the warehouse. Forged on a phone.*
