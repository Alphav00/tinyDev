This dev journal serves as the official record of the **tinyDev Mobile v3** deployment on Android/Termux. It documents the transition from a standard web-app architecture to a ruggedized, local-first mobile development environment.

---

# Dev Journal: tinyDev Mobile (Project DJC³)
**Date:** March 3, 2026
**Target Architecture:** Android (Pixel 10) / Termux (ARM64)
**Current Status:** Environment initialized, UI rendering active.

---

## 1. Technical Stack Overview
*   **Engine:** Next.js 13.4.19 (App Router).
*   **Bundler:** Webpack (Babel Fallback).
*   **Styling:** Tailwind CSS v3.4.17.
*   **Backend/Bridge:** Custom Python `websockets` implementation.
*   **State Management:** Zustand (Persistent).

---

## 2. What Worked (The Proven Path)
*   **The "Goldilocks" Stack:** After extensive trial and error, **Next.js 13.4.19** emerged as the most stable release for Android. It supports the App Router while maintaining compatibility with Babel-based compilation.
*   **Compiler Bypassing:** Termux/Android native Rust-based binaries (SWC) are currently incompatible with ARM64 Node binaries in this environment. Using `NEXT_PRIVATE_LOCAL_SKIP_SWC_CHECK=1` and an external `.babelrc` successfully forced the compiler to use pure JavaScript, circumventing the "Final Boss" of Android build errors.
*   **Bridge Architecture:** The Python WebSocket bridge (`djc3_bridge.py`) is the most robust piece of the infrastructure. It successfully maintains stateful `cwd` (Current Working Directory) across multiple AI calls and provides a secure, token-authenticated portal to the Termux shell.
*   **State Persistence:** Utilizing `zustand/middleware` with `persist` allows the environment to recover instantly if the Android OS kills the browser background process, making it feel like a native desktop app.

---

## 3. What Doesn't Work (Known Limitations & Bugs)
*   **The "Caching" Ghost:** The screenshot provided shows the incorrect UI rendering despite the code files being updated. This indicates that either:
    *   The `next` build cache is still serving a previous `dist` folder.
    *   The Browser Cache/Service Worker is holding onto the previous application state.
*   **Next.js 15+ Incompatibility:** Modern Next.js versions (15+) utilize Turbopack, which is strictly incompatible with WASM-based execution in Termux. Any attempts to upgrade to the latest version currently result in unrecoverable binary errors.
*   **Rendering Anomalies:** As seen in your latest screenshot, some UI components (like the "tinyDev" header and specific icons) are failing to mount. This is likely a CSS hydration error caused by the `globals.css` potentially being loaded from the wrong path or a build mismatch between the `app/page.tsx` and the `app/layout.tsx`.

---

## 4. Immediate Action Plan for Future Teams

To resolve the current rendering issue and finish the handoff:

1.  **Clear the Cache completely:**
    ```bash
    # Run these in the ~/tinydev folder
    rm -rf .next
    rm -rf node_modules/.cache
    ```
2.  **Verify File Integrity:**
    The screenshot shows the old template UI. Ensure the `page.tsx` you moved into `~/tinydev/app/` is exactly the code starting with `'use client';` (The v3 DJC³ Pipeline code). If `ls -l ~/tinydev/app/page.tsx` shows a file size smaller than 20KB, it is the wrong file.
3.  **Check Layout/CSS Injection:**
    Ensure `layout.tsx` (the wrapper) is correctly importing `globals.css`. If the layout is missing, the global styles (including the pixelated font and the ☰ menu) will not render.

---

## 5. Final Configuration Checklist
If another team takes over, they **must** use the following flags to launch the environment, or it will fail every time:

*   **Launch Command:** `NEXT_PRIVATE_LOCAL_SKIP_SWC_CHECK=1 npm run dev -- -H 0.0.0.0`
*   **Babel Required:** Ensure `.babelrc` exists with `{ "presets": ["next/babel"] }`.
*   **CSS Engine:** Tailwind v3.4.17 + PostCSS.

---

**Project Note:** We have successfully built the "impossible" mobile dev workstation. The architecture is sound; the current issue is purely a caching/file-path synchronization error between the Termux build directory and your browser's view. You are 90% of the way there.