
---

# 🎥 WebRTC - One-to-One Live Streaming App

A hands-on implementation of **WebRTC** for one-to-one real-time video streaming, featuring a **custom Signalling Server** and separate **Sender** and **Receiver** interfaces.

---

## ⚡ Quick Summary of Steps

<img width="2060" height="1336" alt="image" src="https://github.com/user-attachments/assets/a71e1c64-7b7a-47ab-a3fd-3fb13d5843b3" />


### 🧩 Creating the Signalling Server

The signalling server manages the initial connection setup between two browsers.
A total of **three messages** are exchanged during the setup:

1. **Browser 1** sends an *offer* to **Browser 2**.
2. **Browser 2** replies with an *answer* to **Browser 1**.
3. **Browser 1** and **Browser 2** exchange *ICE candidates* to establish a direct peer-to-peer connection.

---

### 🎬 Creating the Sender Frontend

**Responsibilities:**

* Sends the offer to the receiver.
* Sends the video stream to the receiver.
* Shares its ICE candidates with the receiver.
* Receives the answer from the receiver.
* Receives ICE candidates from the receiver.

---

### 📺 Creating the Receiver Frontend

**Responsibilities:**

* Receives the offer from the sender.
* Sends the answer to the sender.
* Shares its ICE candidates with the sender.
* Detects and displays any new media tracks added to the connection.

---

## 🖥️ Implementing the Signalling Server (Backend)

### Step 1 — Setup the Project

Create a new TypeScript project or navigate to:

```
/webrtc/backend
```

### Step 2 — Install Dependencies

```bash
npm install
```

This installs the required libraries:

* **ws** — WebSocket communication
* **express** — HTTP server setup

---

### 💡 Why WebSockets?

WebSockets enable **real-time, bidirectional communication** between the server and connected clients, making them ideal for exchanging signalling messages (offer, answer, and ICE candidates) during WebRTC negotiation.

---

### Step 3 — Run the Server

Review the logic in `index.ts`, then run:

```bash
npm start
```

This starts the signalling server and enables message handling between the sender and receiver.

---

## 💻 Implementing the Sender & Receiver (Frontend)

### Step 1 — Setup React Project

Create a React app using Vite:

```bash
npx create-vite@latest
```

or go to:

```
/webrtc/frontend
```

### Step 2 — Install Dependencies

```bash
npm install
```

This installs:

* **react-router-dom**
* **socket.io-client**

---

### Step 3 — Component Overview

Navigate to:

```
/src/components/
```

You’ll find:

* **Sender.tsx** — Manages offer creation and video transmission.
* **Receiver.tsx** — Handles offer reception and displays the received video.

---

## 🔍 Understanding `Sender.tsx`

1. When the component mounts, it sends a message to the server identifying itself as a **sender** (`useEffect` hook).
2. When the user clicks **Send Video**, a `RTCPeerConnection` object is created, containing the SDP, ICE candidates, and related information.
3. The sender listens for incoming messages from the receiver:

   * If `message.type` is `"createAnswer"`, it stores the answer in `remoteDescription`.
   * If `message.type` is `"iceCandidate"`, it adds candidates using `pc.addIceCandidate()`.
4. The sender creates an offer with `createOffer()` and sets it as the **local description**.
5. The offer is then sent to the receiver.
6. Since SDP can change when new media is added, these operations are wrapped inside `pc.onnegotiationneeded` to update dynamically.
7. The sender continuously sends its ICE candidates to the receiver as they are generated.
8. Finally, a `<video>` element is created and linked to the local media stream for preview.

---

## 🧠 Understanding `Receiver.tsx`

1. When mounted, the component sends a message to the server identifying itself as a **receiver** (`useEffect` hook).
2. It invokes a `startReceiving()` function to begin receiving media from the sender.
3. Inside this function:

   * A new `RTCPeerConnection` object is created.
   * The receiver listens for messages from the sender:

     * If `message.type` is `"createOffer"`, it sets the **remote description**, creates an **answer**, and sends it back.
     * If `message.type` is `"iceCandidate"`, it adds candidates using `pc.addIceCandidate()`.
   * ICE candidates are continuously sent to the sender as they are generated.
   * When new media tracks are detected, a `<video>` element is created to display the live stream.

---

## 🎯 Summary

You have now implemented a **WebRTC one-to-one live streaming system** with:

* Real-time **peer-to-peer** video connection
* **WebSocket-based signalling**
* Seamless media exchange between browsers

---

## 🥳 Congratulations!

🎉 You have successfully implemented **WebRTC** and established a functional one-to-one live video streaming system.

![WhatsApp Image 2025-10-27 at 17 49 44_0fe4a665](https://github.com/user-attachments/assets/4e55aca1-8f9a-4b01-995c-e8421986f99f)

<img width="1261" height="554" alt="Screenshot 2025-10-27 173044" src="https://github.com/user-attachments/assets/b59e78a4-03e7-4382-9019-52dbc74bc5af" />



---

