# Project Bonfire: Decentralized Mesh Architecture

## 1. Overview
Project Bonfire utilizes the **Google Nearby Connections API** to create a peer-to-peer (P2P) mesh network. Unlike the Server-Client model, this does not require a single "Host" to manage a Hotspot. Devices negotiate connections over Bluetooth/BLE and upgrade to high-speed Wi-Fi Direct automatically.

**Primary Use Case:** Hiking, Protests, Crowded Festivals where infrastructure is absent and users are moving.

## 2. Architecture

### Topology Strategy: P2P_CLUSTER
We use the `P2P_CLUSTER` strategy (M-to-N). This allows the network to be more flexible than a star topology. However, for simplicity in V1, we will implement a `P2P_STAR` (1 advertiser, N discoverers) logic where the "First User" becomes the advertiser.

### Data Flow
1.  **Discovery:**
    * User A clicks "Create Room" -> Starts **Advertising** (`com.google.android.gms.nearby`).
    * User B clicks "Join Room" -> Starts **Discovery**.
2.  **Handshake:**
    * User B sees User A. Requests connection.
    * Both screens show a generated Token (e.g., "1234"). Users confirm match.
    * Connection established (Payload channel open).
3.  **Communication:**
    * Messages are sent as `Payload.fromBytes(utf8_bytes)`.
    * To send a file/image, we use `Payload.fromFile()`.

## 3. Tech Stack
* **Language:** Kotlin
* **API:** Google Play Services (Nearby Connections).
* **Permissions:** `BLUETOOTH_ADVERTISE`, `BLUETOOTH_SCAN`, `ACCESS_FINE_LOCATION`, `NEARBY_WIFI_DEVICES`.

## 4. Key Challenges & Solutions
* **Connection Stability:** Wi-Fi Direct connections can be fragile if users move apart. *Solution:* The API handles fallback to Bluetooth automatically, but bandwidth drops significantly. The UI must indicate "Low Bandwidth Mode".
* **iOS Incompatibility:** This solution is **strictly Android-only**. Apple devices cannot join this mesh.

## 5. Security
* **Encryption:** The API uses robust encryption (DTLS) by default.
* **Authentication:** The visual token comparison (SAS - Short Authentication String) prevents Man-in-the-Middle attacks.