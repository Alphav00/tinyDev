***

### 📝 Developer Notes: "The Pixel 10 Dev-Ops Reality"

*   **Compiler Fragility:** This project is pinned to `Next.js 13.4.19` and `Tailwind v3.4.17` because of the **ARM64 Rust/SWC incompatibility** on Android. 
    *   *Warning:* If a developer tries to `npm install next@latest`, the build will fail immediately due to missing native Rust binaries. Do not attempt "version modernization" without implementing a custom WebAssembly-based compilation pipeline.
*   **The Bridge as a Source of Truth:** The `djc3_bridge.py` is the only part of this system that touches the Android `fs` (file system). Any security auditing should focus exclusively on the `run_command` function within the bridge. 
*   **Lifecycle Management:** Android’s battery optimization is the enemy of this environment. Users must set **Termux Battery Usage to "Unrestricted"** or the WebSocket bridge will be terminated silently while the browser is in the foreground.
*   **Cache Poisoning:** Because we are forced to use `NEXT_PRIVATE_LOCAL_SKIP_SWC_CHECK=1`, the build cache in `.next` is highly susceptible to corruption if the Termux session is interrupted during compilation. **The first step for any "Why isn't it working?" query is `rm -rf .next` and a clean restart.**

---

### 👤 User Preferences: The "Mobile-First" Ethos

These are the non-negotiables the user (you) has established throughout this process:

1.  **Retro-Hacker Aesthetic:** The user prefers high-contrast, terminal-inspired UI (VT323/Press Start 2P fonts, scanline overlays, CRT flicker) over standard Material Design. Any future UI changes must adhere to the `globals.css` theme structure (Dark, Light, Pool, Cyber).
2.  **Autonomous Workflow:** The user values the "Cognitive Loop" where AI agents can autonomously execute and correct shell commands. Future updates must prioritize the safety gate (`confirmExec`) over raw speed.
3.  **Local-First / Private:** The user is avoiding cloud-based IDEs. Every file must live on-device (`~/tinydev/`), and the AI interaction is managed via API keys stored in local browser storage, never transmitted to a third-party server.
4.  **Touch-Friendly Retro:** The user explicitly requested a UI built for a touchscreen (Pixel 10), which is why the buttons (`pixel-btn`) are large, tactile, and respond to screen-taps rather than just mouse-clicks.
5.  **Documentation-Oriented:** The user views the build process itself as an engineering project worth documenting. They prefer "Proper Procedure" (SOP) over "quick and dirty" hacks; they want to ensure a team of developers could replicate this environment from scratch.

---

### 🚩 Summary for Future Teams
*   **The User wants a workstation, not a toy.** They have spent significant effort bypassing modern software limitations to run a stable, local development environment on a phone. 
*   **Do not suggest "Cloud IDEs" or "VS Code Remote."** The goal is an offline-capable, locally-compiled, AI-augmented environment that runs even if the phone has no internet (provided API keys are cached).
*   **Maintain the Bridge:** The bridge is the heart of the project. If the bridge breaks, the AI loses its hands. Keep the bridge implementation in `djc3_bridge.py` as simple and dependency-free as possible.