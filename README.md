# Proxi: Local Network Chat Application

Proxi is a suite of local network chat applications designed for various scenarios where traditional internet connectivity may be limited or unavailable. It explores different architectural approaches to establish peer-to-peer or server-client communication over local networks.

## Projects

Proxi is comprised of three distinct projects, each tailored for specific use cases and technical considerations:

### 1. Project Bonfire: Decentralized Mesh Architecture

*   **Overview:** Utilizes the Google Nearby Connections API to create a peer-to-peer (P2P) mesh network. It does not require a central host or a single hotspot, allowing devices to negotiate connections over Bluetooth/BLE and upgrade to high-speed Wi-Fi Direct.
*   **Primary Use Case:** Ideal for scenarios without internet infrastructure or in crowded environments where users are mobile, such as hiking, protests, or festivals.
*   **Key Features:** Decentralized P2P_CLUSTER topology (M-to-N), robust encryption (DTLS), and automatic fallback to Bluetooth for connection stability.
*   **Limitations:** Strictly Android-only; incompatible with iOS devices.
*   **Tech Stack:** Kotlin, Google Play Services (Nearby Connections API).

### 2. Project Lighthouse: Universal Web-Based Chat

*   **Overview:** Designed for maximum cross-platform compatibility. An Android device acts as a host, running a local web server (Ktor) and creating a Wi-Fi hotspot. Any device with a web browser (iOS, laptop, other Androids) can connect to the host's hotspot and access the chat via a local URL.
*   **Primary Use Case:** Perfect for mixed device groups (e.g., iPhone/Android) or impromptu meetings where users prefer not to install an application.
*   **Key Features:** No client-side app installation required, simple HTML/CSS/JavaScript client, Host app displays a QR code for easy WiFi connection on iOS.
*   **Limitations:** Traffic is unencrypted (HTTP); requires a ForegroundService on the host for continuous operation.
*   **Tech Stack:** Kotlin + Ktor (Host), HTML5, CSS3, Vanilla JavaScript (Client).

### 3. Project Tether: Android Hotspot Server Architecture

*   **Overview:** Operates in a server-client mode where one Android device acts as a host, creating a local Wi-Fi hotspot and running a TCP socket server. Other Android devices connect to this hotspot and establish socket connections for real-time chat.
*   **Primary Use Case:** Suitable for organized groups in situations like classrooms, buses, or emergency scenarios where a designated "leader" device can host the chat.
*   **Key Features:** WPA2 secured network, Network Service Discovery (NsdManager) for host discovery, standard TCP socket communication.
*   **Limitations:** Significant battery drain on the host device; Android 10+ restrictions require user permission for Wi-Fi connection.
*   **Tech Stack:** Kotlin, `java.net.ServerSocket` (or Ktor), Kotlin Coroutines, Hilt.

## Choosing the Right Project

The choice between Bonfire, Lighthouse, and Tether depends on your specific needs:

*   **Bonfire:** For Android-only, highly mobile, and infrastructure-less environments with a focus on decentralized mesh networking and security.
*   **Lighthouse:** For maximum cross-platform compatibility, ease of client access (browser-based), and situations where quick setup without app installation is paramount.
*   **Tether:** For Android-only, server-client setups with a clear host, suitable for more controlled environments and reliable communication over a dedicated hotspot.
