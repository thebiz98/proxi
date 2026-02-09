# Project Tether: Android Hotspot Server Architecture

## 1. Overview
Project Tether (Server-Client Mode) allows one Android device to act as a **Host**, creating a local Wi-Fi hotspot and running a TCP socket server. Other Android devices act as **Clients**, connecting to this hotspot programmatically and establishing a socket connection for real-time chat.

**Primary Use Case:** Classroom, Bus, Emergency situations where one device is stationary or designated as the "Leader".

## 2. Architecture

### High-Level Diagram
[Client Device] --(WiFi)--> [Host Device (Hotspot AP)]
[Client App]    --(Socket)--> [Host App (Server Port 8080)]

### Components
1.  **Host (Server):**
    * **Network Layer:** Uses `LocalOnlyHotspot` (Android O+) to create a WPA2 secured network.
    * **Discovery Layer:** Uses `NsdManager` (Network Service Discovery) to broadcast its service name (`_Tetherchat._tcp`).
    * **Application Layer:** Runs a `ServerSocket` on port 8080. It maintains a `List<Socket>` of connected clients. When a message is received from Client A, it iterates through the list and writes the message to Client B, C, etc.

2.  **Client:**
    * **Network Layer:** Uses `WifiNetworkSpecifier` to request a connection to the specific Hotspot SSID provided by the user (or scanned via QR code).
    * **Discovery Layer:** Uses `NsdManager` to resolve the IP address of the Host.
    * **Application Layer:** Initiates a standard TCP `Socket` connection to the resolved IP.

## 3. Tech Stack
* **Language:** Kotlin
* **Networking:** `java.net.ServerSocket` (Standard Java Sockets) or Ktor (Recommended for easier concurrency).
* **Concurrency:** Kotlin Coroutines (`Dispatchers.IO`).
* **Dependency Injection:** Hilt (for managing the Server Singleton).

## 4. Key Challenges & Solutions
* **Battery Drain:** The Host device will consume significant battery. *Mitigation:* Use a Foreground Service with a persistent notification to keep the process alive and warn the user.
* **Android 10+ Restrictions:** Apps cannot enable/disable Wi-Fi silently. *Solution:* We must use the `ConnectivityManager.requestNetwork` API which prompts the user for permission to connect to the specific unmetered network.

## 5. Future Roadmap
* **Phase 1:** Echo Server (One client sends, Server echoes back).
* **Phase 2:** Broadcast Server (One sends, Server sends to all).
* **Phase 3:** UI Polish (Jetpack Compose chat interface).