# Crypton: Offline Secure Communication Suite



## Overview

Crypton is a privacy-focused, serverless toolkit for secure offline communication between devices. Built with modern web technologies, it enables peer-to-peer (P2P) messaging and file sharing without internet, servers, or third-party dependencies. Connections are bootstrapped via QR code scanning, using cryptographic handshakes to ensure end-to-end encryption.

### Core Components
- **Crypton** (`index.html`): A secure chat app with RSA/ECDH key exchange, AES-GCM encryption, and visual fingerprint verification. Supports multi-device QR-based message transfer.
- **Wave-Share** (`wave.html`): A kinetic file transfer bridge using WebRTC data channels, initiated via animated QR streams for large payloads (unlimited size).

Ideal for air-gapped environments, protests, or scenarios where connectivity is unreliable/untrusted.

**Key Principles**:
- **Zero-Knowledge**: No data is stored or transmitted to external services.
- **Man-in-the-Middle (MITM) Resistance**: Visual fingerprint checks and tamper simulation mode.
- **Cross-Platform**: Runs in any modern browser (Chrome, Firefox, Safari) on desktop/mobile.

## Features

### Crypton (Secure Chat)
- **QR Bootstrapping**: Scan partner identities to establish encrypted sessions.
- **End-to-End Encryption**: RSA-OAEP for key exchange, ECDH for shared secrets, AES-GCM-256 for messages.
- **Visual Verification**: Emoji/hex fingerprints to detect MITM attacks.
- **Darth Mode**: Simulate interception/tampering for security education.
- **Animated QR Transfer**: Chunked, looping QR animations for reliable multi-message delivery.
- **Tabs**: Connect → Verify → Chat → Files (unlocks post-session).

### Wave-Share (File Transfer)
- **Unlimited Payloads**: Transfer files of any size via WebRTC data channels.
- **Kinetic QR Sync**: Animated QR loops for SDP offer/answer exchange.
- **Progress Tracking**: Real-time chunk capture and transfer visualization.
- **Sender/Receiver Modes**: Simple toggle; no pairing required.
- **Auto-Download**: Seamless file reconstruction on receiver.

| Feature          | Crypton                  | Wave-Share              |
|------------------|--------------------------|-------------------------|
| **Encryption**   | AES-GCM-256              | WebRTC DTLS/SRTP        |
| **Max Size**     | Messages (chunked QR)    | Unlimited (file-based)  |
| **Bootstrap**    | Single QR scan           | Animated QR loop        |
| **Verification** | Fingerprint check        | SDP integrity (implicit)|
| **Modes**        | Normal/Darth             | Sender/Receiver         |

## Quick Start

### Prerequisites
- Modern web browser with camera access (HTTPS or localhost).
- No installation required—open files directly.

### Running Crypton (`index.html`)
1. Save the first HTML file as `index.html`.
2. Open in browser: `file://path/to/index.html` (or serve via `npx serve` for camera access).
3. **Connect**:
   - Generate your identity QR (Step 1).
   - Scan partner's QR to exchange ECDH keys.
   - Verify shared fingerprint (emojis must match).
4. **Chat**: Send encrypted messages via QR scans.
5. **Files**: Click "Launch" to open Wave-Share for transfers.
6. **Darth Mode**: Toggle bug icon to simulate attacks.

### Running Wave-Share (`wave.html`)
1. Save the second HTML file as `wave.html`.
2. From Crypton, click "Launch" in Files tab (or open directly).
3. **Sender**:
   - Select file → Generate QR animation.
   - Receiver scans to establish WebRTC peer.
4. **Receiver**:
   - Scan sender's QR → Capture chunks → Auto-transfer.
5. Download completed file.

**Demo Flow**:
- Alice (Sender): Opens Crypton, scans Bob's ID QR → Chats → Launches Wave-Share → Selects photo.
- Bob (Receiver): Scans Alice's handshake → Verifies fingerprint → Scans Wave-Share QR → Receives file.

## How It Works

### Crypton Protocol (v2.4)
1. **Identity Exchange**: RSA public keys via QR (SPKI format).
2. **Handshake**: Encrypt ECDH public key with partner's RSA → Derive AES session key via HKDF.
3. **Messaging**: Encrypt plaintext with AES-GCM → Chunk JSON into QR frames (350 chars max, 200 FPS default).
4. **Verification**: SHA-256 fingerprint of shared secret (4 emojis + 8-hex prefix).

**Security Notes**:
- QR chunks prevent single-scan attacks.
- Darth Mode tampers packets (fails GCM tags on receiver).
- No persistent storage—sessions ephemeral.

### Wave-Share Protocol
1. **SDP Exchange**: WebRTC offer/answer serialized to base64, chunked into looping QR (150 chars, 400ms delay).
2. **Peer Connection**: STUN for ICE candidates (Google STUN server used).
3. **Data Channel**: Binary file chunks (16KB) over "visualwave" channel.
4. **Reassembly**: Receiver buffers → Blob → Download.

**Limitations**:
- Camera must face QR clearly (environment mode preferred).
- Large files: ~1-5 MB/min depending on scan speed.
- Single-direction transfer (restart for bidirectional).

## Technologies
- **Frontend**: React 18 (UMD), Tailwind CSS, Babel Standalone.
- **Crypto**: Web Crypto API (RSA/ECDH/AES/HKDF).
- **QR**: QRious (gen), jsQR (scan).
- **WebRTC**: Native RTCPeerConnection/DataChannel.
- **Icons**: Custom SVG + Lucide.
- **Fonts**: Inter, JetBrains Mono, Space Grotesk.

## Screenshots
*(Add actual images here in your repo)*

1. **Crypton Connect Tab**: Identity QR generation.
2. **Verification**: Fingerprint emoji display.
3. **Chat**: Encrypted message bubbles.
4. **Wave-Share Sender**: Animated QR loop.
5. **Receiver Scan**: Chunk progress grid.

## Development
- Edit HTML/JS inline (Babel transpiles JSX).
- Test: Use two devices/browsers; emulate camera with QR generators.
- Extend: Add voice (WebRTC MediaStream) or multi-hop relays.

## License
MIT License. Free for personal/commercial use. No warranties—use at your own risk.

## Credits
- Inspired by air-gapped tools like and offline messengers.
- Built with ❤️ for privacy advocates.

**Star/Fork on GitHub?** Contribute QR optimizations or new protocols! 🚀
