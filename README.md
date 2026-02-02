# AIR Wallpaper - Starstream Edition

**Official Internal Tool of AI Raadgivning ApS**

A high-performance, adaptive animated wallpaper designed for macOS and Plash. It features a "starstream" particle effect with a central "Air Logo" that reacts to your system activity.

## ✨ Features

-   **Adaptive Speed**: The animation speed reacts to your typing and clicking activity in real-time.
-   **Cyber Retrowave Aesthetics**: The logo features wave ripples, chromatic aberration, and scanlines.
-   **Dual Modes**:
    -   **Default**: Constant, smooth animation.
    -   **Adaptive**: Reacts to your workflow intensity.
-   **Native Performance**: Powered by a lightweight Swift background monitor (negligible CPU usage).
-   **Offline Capable**: Runs entirely locally on your machine.
-   **Zero-Friction**: Single-script installation with auto-starting background services.

## 🚀 Installation

We have streamlined the setup into a single "one-click" installer.

1.  **Prerequisite**: Install **Plash** from the Mac App Store.

2.  **Open Terminal** and navigate to the folder where you pulled this repository:
    ```bash
    cd /path/to/your/downloaded/repo
    ```

3.  **Run the Installer**:
    ```bash
    ./install.sh
    ```
    *This script will compile the native monitor, install background agents, and clean up temporary files.*

3.  **Grant Permissions**:
    -   You will likely see a popup asking for **Accessibility Permissions**.
    -   Go to **System Settings > Privacy & Security > Accessibility**.
    -   Toggle the switch for `monitor` to **ON**.
    -   *If the animation doesn't react immediately, run `./install.sh` one more time to restart the service.*

4.  **Configure Plash**:
    -   Open **Plash** (available on the Mac App Store).
    -   Add a new website with one of the following URLs:
        -   **Adaptive Mode** (Recommended): `http://localhost:8000?mode=adaptive`
        -   **Default Mode** (Constant): `http://localhost:8000`

## ❓ Troubleshooting

**Problem: Animation freezes after a while.**
-   **Fix**: Ensure you are using the `?mode=adaptive` URL. The latest update includes a "minimum speed" floor to prevent freezing when idle.
-   **Fix**: Check if the `monitor` process is running: `pgrep -l monitor`. If not, run `./install.sh` again.

**Problem: "Air Logo" is missing.**
-   **Fix**: Ensure `airlogo.svg` is present in the project folder.
-   **Fix**: Check the browser console (Right-click > Inspect Element in Plash "Browsing Mode") for errors.

**Problem: Speed doesn't change when I type.**
-   **Fix**: Verify Accessibility Permissions are granted for `monitor`.
-   **Fix**: Ensure you are using the `?mode=adaptive` URL.

## 🛠 Technical Details

-   **Frontend**: Three.js (WebGL) with custom GLSL shaders.
-   **Backend**: Native Swift (`monitor`) for system-wide event tracking.
-   **Communication**: The monitor writes to `speed.json`, which the frontend polls.
-   **Persistence**: `launchd` agents keep both the web server and the monitor running in the background.

---
## FPS Counter Overlay

If you are merging changes from the prior feature branch or reintroducing the FPS overlay manually, follow the same three parts that ship in this repo:

1. **CSS** – copy the `.fps-counter`, `.fps-label`, and `.fps-value` rules into your `index.html` (or equivalent stylesheet). The overlay sits fixed in the bottom-right corner, uses a translucent capsule background, and has `pointer-events: none` so it never blocks desktop interactions.
2. **DOM + helper** – create the container and two spans inside `animation.js` (or your module entrypoint), append them to `document.body`, and call `updateFpsDisplay(0)` once so the counter shows zero before the loop runs.
3. **Loop logic** – inside `animate()` compute the delta from `performance.now()`, accumulate frames over a 0.5 s window, clamp and round the computed FPS to the [1, 999] range, and call `updateFpsDisplay` right before the render step. This preserves the legacy telemetry math and keeps the overlay tightly coupled with the animation loop.

Including all three parts ensures the overlay looks, behaves, and reports the same metrics as before the feature branch broke. The latest styling reinterprets the capsule as a green CRT/Nixie-style counter (scanlines, glow, and radial tint) so it matches the desired retro console aesthetic.

*Property of AI Raadgivning ApS*
