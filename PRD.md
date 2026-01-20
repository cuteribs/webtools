# Product Requirements Document (PRD) - WebTools Collection

## 1. Introduction

### 1.1 Overview
The **WebTools Collection** is a suite of essential, browser-based utilities designed for developers and technical professionals. It provides a convenient, privacy-focused, and offline-capable environment for common tasks such as JWT analysis, string manipulation, and network device identification.

### 1.2 Goals
-   **Privacy First:** Ensure all data processing occurs locally within the user's browser, eliminating the need to send sensitive data (like JWTs or internal MAC addresses) to external servers.
-   **Accessibility:** Provide a responsive, mobile-friendly interface that works across various devices.
-   **Performance:** Deliver lightweight, fast-loading tools with minimal dependencies.
-   **Usability:** Offer an intuitive user experience with clear visual feedback and helpful features like clipboard integration.

### 1.3 Target Audience
-   Software Developers (Frontend, Backend, Full-stack)
-   Network Engineers
-   Security Analysts
-   System Administrators

## 2. Technical Architecture

### 2.1 Technology Stack
-   **Core Logic:** JavaScript (ES6+)
-   **Framework:** [Alpine.js](https://alpinejs.dev/) (v3.x) - Lightweight reactive framework.
-   **Styling:** [Bulma](https://bulma.io/) (v0.9.4) - Modern CSS framework based on Flexbox.
-   **Icons:** Font Awesome (v6.4.0).
-   **Storage:** Dexie.js (v4.x) - Wrapper for IndexedDB (used in MAC Vendor Lookup).
-   **Libraries:** 
    -   `jose` (v4.14.4) for JWT operations.
    -   `manuf.json` (Wireshark database) for MAC lookup data.

### 2.2 Design Principles
-   **Client-Side Execution:** No backend processing for tool logic. The server only serves static assets.
-   **Modular Design:** Each tool operates independently, although they share a common design language.
-   **Responsive Layout:** UI adapts seamlessly to mobile, tablet, and desktop viewports.

## 3. Product Features

### 3.1 Portal (Landing Page)
The entry point to the application, providing navigation and project information.
-   **Tool Catalog:** Grid view of available tools with descriptions, categories, and feature lists.
-   **Quick Access:** Large, touch-friendly buttons for rapid navigation to common tools.
-   **Information:** "About" section detailing privacy policies and technology stack.

### 3.2 JWT Decoder & Signature Verifier
A security tool for inspecting and verifying JSON Web Tokens (JWT).

**Key Features:**
-   **Decoding:** Instantly decodes Header and Payload components of a JWT.
-   **Pretty Printing:** Formats JSON output for readability.
-   **Token Analysis:** Extracts and displays critical fields:
    -   Algorithm (`alg`), Type (`typ`), Key ID (`kid`).
    -   Issuer (`iss`), Subject (`sub`), Audience (`aud`).
    -   Timestamps (`exp`, `iat`, `nbf`) converted to human-readable dates.
-   **Signature Verification:** 
    -   Supports verification against remote JWKS (JSON Web Key Sets).
    -   **Auto-Discovery:** Automatically attempts to resolve the JWKS URL based on the `iss` (Issuer) claim (e.g., appending `/.well-known/openid-configuration`).
-   **UX Utilities:** "Copy to Clipboard" for individual sections or the entire token.

### 3.3 String Encoder/Decoder
A versatile utility for converting text between different formats.

**Supported Formats:**
1.  **Base64:** Standard binary-to-text encoding.
2.  **URI Component:** Percent-encoding for URLs.
3.  **Binary to Hex:** Hexadecimal representation of text.
4.  **HTML Entities:** Safe encoding for HTML special characters.
5.  **Unicode Escape:** JavaScript/JSON compatible escape sequences (e.g., `\u0041`).
6.  **Emoji:** Conversion between Emoji characters and Shortcodes/Unicode.
7.  **JSON Escape:** Escaping strings for inclusion in JSON.

**Key Features:**
-   **Bi-directional:** One-click Encode and Decode functions.
-   **Chaining:** "Use as Input" feature allows output to be immediately reused for multi-step transformations.
-   **Error Handling:** Visual feedback for invalid inputs (e.g., malformed Base64).

### 3.4 MAC Vendor Lookup
A network utility for identifying device manufacturers via Media Access Control (MAC) addresses.

**Key Features:**
-   **Database:** Integrates with the Wireshark OUI database (`manuf.json`).
-   **Smart Caching:** Uses IndexedDB (via Dexie.js) to cache the database locally, enabling fast, offline-capable lookups after the initial download.
-   **Flexible Input:**
    -   Supports multiple formats: `00:50:56:12:34:56`, `00-50-56...`, `005056...`.
    -   Accepts full MAC addresses or just the OUI (first 6 hex digits/3 bytes).
    -   Auto-search triggers on valid input length.
-   **Database Management:** User-initiated "Refresh Database" to fetch the latest manufacturer data.

### 3.5 QR Code Generator & Scanner
A utility for generating and reading QR codes directly in the browser.

**Key Features:**
-   **Generation:** Create QR codes from text, URLs, Wi-Fi credentials, or VCards.
-   **Customization:** Adjust size, error correction level (L, M, Q, H), and foreground/background colors.
-   **Download:** Export generated QR codes as PNG or SVG images.
-   **Scanning:** 
     -   Scan QR codes from an uploaded image file.
     -   (Optional) Decode from webcam video stream.

### 3.6 UUID Generator
A developer tool for generating Universally Unique Identifiers (UUIDs).

**Key Features:**
-   **Versions:** Support for standard UUID versions:
    -   **v1:** Time-based.
    -   **v3:** Namespace-based (MD5).
    -   **v4:** Random (most common).
    -   **v5:** Namespace-based (SHA-1).
-   **Bulk Generation:** Create multiple UUIDs in a single batch (e.g., 1-1000).
-   **Formatting:** Toggle options for:
    -   Hyphens (standard vs. compact).
    -   Case (uppercase vs. lowercase).
    -   Braces (optional wrapping `{}`).
-   **Copy:** One-click copy for single or bulk results.

## 4. User Interface & Experience

### 4.1 Design System
-   **Theme Modes:** Support for both **Light** and **Dark** themes.
    -   Toggle switch in the navigation bar.
    -   Auto-detect system preference (`prefers-color-scheme`).
    -   Persist user choice in `localStorage`.
-   **Styling:** Clean, modern interface using Bulma with custom variables for theming.
-   **Color Palette:**
    -   Primary: Blue/Indigo (Navigation, Primary Actions).
    -   Accents: Color-coded by tool category (Security: Warning/Yellow, Encoding: Info/Blue, Network: Success/Green).
-   **Typography:** Sans-serif for UI elements; Monospace (`Courier New`) for data inputs/outputs (code, tokens, hex).

### 4.2 Error Handling
-   Non-intrusive warning banners for manageable errors (e.g., "Invalid Base64 string").
-   Clear loading states (spinners) during async operations (e.g., fetching JWKS or MAC database).

### 4.3 Internationalization (i18n)
-   **Languages:** Full support for **English** (default) and **Chinese (Simplified)**.
-   **Implementation:**
    -   Language toggle in the header.
    -   All UI text strings abstracted into resource dictionaries/JSON files.
    -   Persist language preference in `localStorage`.

## 5. Non-Functional Requirements

### 5.1 Privacy & Security
-   **Zero-Trust:** No user input is ever transmitted to a backend server controlled by the application owner.
-   **External Calls:** Limited to specific, user-initiated actions:
    -   Fetching `manuf.json` (from Wireshark or local mirror).
    -   Fetching JWKS keys (from the token's specified issuer).

### 5.2 Performance
-   **Lazy Loading:** Scripts (Alpine.js) deferred to prevent render blocking.
-   **Debouncing:** Input fields (like JWT and search) invoke expensive operations only after the user stops typing (e.g., 250ms delay).
-   **Caching:** Extensive use of browser storage to minimize network requests.

### 5.3 Compatibility
-   Support for modern browsers (Chrome, Firefox, Edge, Safari).
-   Requires JavaScript enabled.
