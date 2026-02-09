# Project Lighthouse: Universal Web-Based Chat

## 1. Overview
Project Lighthouse is designed for maximum compatibility. The Host Android device runs a **Local Web Server**. Any device with a browser (iPhone, Laptop, Android) connects to the Host's Hotspot and navigates to a local URL (e.g., `http://192.168.49.1:8080`) to enter the chat room. No app installation is required for clients.

**Primary Use Case:** Cross-platform scenarios (e.g., mixed iPhone/Android groups), impromptu meetings where users don't want to install an APK.

## 2. Architecture

### Diagram
[Host Device (Android)]
   |-- Runs `LocalOnlyHotspot` (WiFi AP)
   |-- Runs `Ktor Embedded Server` (Port 8080)
         |-- / (GET) -> Serves `index.html`, `style.css`, `chat.js`
         |-- /ws (WebSocket) -> Handles real-time message exchange

[Client Device (iOS/Laptop)]
   |-- Connects to WiFi
   |-- Browser opens `http://192.168.49.1:8080`
   |-- JS Client opens WebSocket connection

### Components
1.  **The Host App (Android):**
    * **Ktor Server:** A lightweight web server embedded in the Android app.
    * **Static Content:** The HTML/JS/CSS files are stored in the Android `assets/web` folder. Ktor is configured to serve these files.
    * **WebSocket Handler:** Manages the chat logic. Stores active sessions and broadcasts incoming messages to all connected sockets.
2.  **The Web Client (HTML/JS):**
    * A Single Page Application (SPA).
    * Uses vanilla JavaScript `WebSocket` API to connect to the host.
    * Simple UI rendering message bubbles.

## 3. Tech Stack
* **Host Logic:** Kotlin + Ktor (Server Engine: Netty).
* **Client Logic:** HTML5, CSS3, Vanilla JavaScript.
* **Host UI:** Jetpack Compose (To manage the server status: Start/Stop/Show IP).

## 4. Key Challenges & Solutions
* **User Friction (iOS):** iOS users must manually connect to the WiFi in settings. *Solution:* The Host app displays a large **QR Code**. iOS users scan it with their camera to instantly join the WiFi network.
* **Background Execution:** The server dies if the Host locks the screen. *Solution:* Use an Android `ForegroundService` with `WAKE_LOCK` to keep the CPU and WiFi radio active.

## 5. Security Note
* Traffic is unencrypted (HTTP). This is acceptable for a local, ephemeral chat, but users should be warned not to share sensitive passwords.