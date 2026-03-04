This manual outlines the standard operating procedure (SOP) for deploying the **tinyDev Mobile v3 (DJC³ Pipeline)** development environment on an Android/Termux system. 

Follow these steps in exact order to ensure a stable, production-ready build.

---

# Technical Manual: tinyDev Mobile Deployment (v3.0.0)

## Phase 1: Environment Preparation
*Do not skip dependency validation.*

1.  **Termux Initialization:**
    ```bash
    pkg update && pkg upgrade -y
    pkg install python nodejs git -y
    pip install websockets
    termux-setup-storage
    ```
2.  **Directory Setup:**
    ```bash
    mkdir -p ~/tinydev/app/api/agent
    cd ~/tinydev
    ```

## Phase 2: Configuration & Version Locking
*This environment relies on the "Sweet Spot" stable stack (Next.js 13.4.19).*

1.  **Version Enforcement:**
    ```bash
    npm install next@13.4.19 react@18.2.0 react-dom@18.2.0
    npm install -D tailwindcss@3.4.17 postcss@8 autoprefixer@10 @babel/core
    ```
2.  **Compiler Configuration:**
    Create the following files in the project root (`~/tinydev/`):
    *   **`.babelrc`**: `{ "presets": ["next/babel"] }`
    *   **`next.config.js`**: `module.exports = { swcMinify: false };`
    *   **`postcss.config.js`**: `module.exports = { plugins: { tailwindcss: {}, autoprefixer: {} } };`

## Phase 3: Asset Synchronization
Ensure all source files (`page.tsx`, `route.ts`, `globals.css`) are placed in their respective directories. 

**Required Directory Structure:**
*   `~/tinydev/app/page.tsx` (UI Logic)
*   `~/tinydev/app/globals.css` (Style Rules)
*   `~/tinydev/app/api/agent/route.ts` (API Proxy)
*   `~/tinydev/djc3_bridge.py` (Shell Execution Portal)

*Verification:* Run `ls -R ~/tinydev/app` to confirm all files are present.

## Phase 4: Launch Procedure (Standard SOP)
*This system requires a dual-process bridge.*

### Step 1: Initialize the Bridge (Session 1)
This process grants the UI access to the Termux shell.
```bash
cd ~/tinydev
python djc3_bridge.py
```
*Note the generated **TOKEN**; it is required for frontend authorization.*

### Step 2: Initialize the Interface (Session 2)
Use the environment flag to bypass the incompatible native Android Rust compiler.
```bash
cd ~/tinydev
rm -rf .next
NEXT_PRIVATE_LOCAL_SKIP_SWC_CHECK=1 npm run dev -- -H 0.0.0.0
```

## Phase 5: Interface Handshake
1.  Open Chrome and navigate to `http://127.0.0.1:3000`.
2.  **First Launch:** Allow ~60 seconds for Babel to compile the application.
3.  **Bridge Authentication:**
    *   Tap the **Settings (☰)** icon.
    *   Navigate to **BRIDGE**.
    *   Input: `ws://localhost:8765` and your **TOKEN**.
    *   Tap **CONNECT**.

---

## Troubleshooting & Maintenance
*   **Failed Build:** If the server crashes or reports "Failed to compile," always run `rm -rf .next` before restarting.
*   **UI Mismatch:** If the interface appears incorrect, clear the browser's cache. You are likely viewing a cached build from an incompatible version.
*   **Permission Denied:** Ensure your `djc3_bridge.py` has execution permissions (`chmod +x djc3_bridge.py`).

## Security Protocol
*   The `djc3_bridge.py` generates a random security token on every launch. **Never share this token.** 
*   The system binds to `0.0.0.0` for localhost accessibility; ensure your Android device firewall/privacy settings allow local loopback traffic.

***

*This manual is the property of the DJC³ Development Team. Modifications to the dependency stack are strictly prohibited without verification against the ARM64 environment.*