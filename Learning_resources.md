# Engineering Learning Roadmap: Local Networking on Android

This document outlines the requisite knowledge to build local offline communication apps.

## Phase 1: The Fundamentals (Required for all approaches)
* **Kotlin Coroutines:** Networking must happen off the main thread.
    * *Concept:* `suspend` functions, `Dispatchers.IO`, `lifecycleScope`.
    * *Resource:* [Kotlin Coroutines Guide](https://kotlinlang.org/docs/coroutines-guide.html)
* **Android Services:** Keeping the connection alive.
    * *Concept:* Foreground Services, Notification Channels.
    * *Resource:* [Android Services Documentation](https://developer.android.com/guide/components/services)

## Phase 2: Socket Networking (For "Tether" & "Lighthouse")
* **Raw Sockets (The Hard Way):**
    * Start by building a simple Java/Kotlin console app that sends "Hello" from one terminal window to another using `java.net.Socket`.
* **Ktor (The Modern Way):**
    * *Concept:* Using Ktor as a Server *inside* an Android app.
    * *Resource:* [Ktor Server Documentation](https://ktor.io/docs/server-create-a-new-project.html)
    * *Key Topic:* WebSockets in Ktor.

## Phase 3: Android WiFi APIs (For "Tether" & "Lighthouse")
* **LocalOnlyHotspot:**
    * *Concept:* Programmatically creating an Access Point.
    * *Docs:* `WifiManager.startLocalOnlyHotspot`.
* **Network Service Discovery (NSD):**
    * *Concept:* How devices find each other without hardcoding IPs.
    * *Docs:* `NsdManager` (Registering and Discovering services).

## Phase 4: Google Nearby Connections (For "Bonfire")
* **The API:**
    * *Resource:* [Nearby Connections Overview](https://developers.google.com/nearby/connections/overview)
    * *Key Concepts:* `Advertising`, `Discovery`, `Payloads`, `Strategies (P2P_STAR vs P2P_CLUSTER)`.

## Phase 5: Web Technologies (For "Lighthouse")
* If building the web-host version, you need basic frontend skills.
    * *Concepts:* HTML5, CSS Flexbox (for chat layout), JavaScript `WebSocket` API.

## Recommended Project Order
1.  **Build a CLI Chat:** Two terminal windows on your laptop talking via Sockets.
2.  **Build "Lighthouse" (Web Host):** Easiest to debug. Run Ktor on Android, connect via Chrome on your laptop.
3.  **Build "Tether" (Android-to-Android):** Add the complexity of programmatic WiFi connection.