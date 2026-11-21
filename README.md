# Birthday Gift Web Application

An interactive, single-page birthday greeting web application protected by client-side AES-256-GCM encryption. All private messages, media assets, and gift sections remain encrypted at rest and are decrypted directly in the browser only when the correct passcode is entered.

## Features

- **Passcode-Protected Vault:** Uses the browser's native Web Crypto API (`PBKDF2` + `AES-256-GCM`) to decrypt protected views and embedded assets on the client side without storing plaintext secrets in the source code.
- **Interactive Gift Hub:** Unlocks four distinct sections upon authentication:
  - **Gallery:** Polaroid-style photo showcase.
  - **Bouquet:** Interactive flower display with multilingual notes.
  - **Letter:** Envelope animation leading to a personal birthday message.
  - **Cake:** Interactive birthday cake with clickable candles and confetti effects.
- **Background Audio:** Integrated audio player that initializes upon unlocking the vault.
- **Responsive Design:** Adapts cleanly across desktop and mobile viewports with both on-screen keypad and physical keyboard input support.

## Project Architecture

To keep personal photos, audio, and messages private in a public repository, the build pipeline separates development source files from the deployed bundle:

1. **Asset Inlining:** Local images and audio files are encoded into Base64 Data URIs during the build step so no raw media folders need to be exposed publicly.
2. **Symmetric Encryption:** The protected HTML sections and inlined media are encrypted using a 256-bit key derived via PBKDF2 (100,000 iterations, SHA-256) from the 4-digit passcode.
3. **Zero-Server Static Hosting:** The final `index.html` is a self-contained static file compatible with GitHub Pages, Vercel, or Netlify.

## Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript (Web Crypto API)
- Python (`cryptography` library for build-time encryption)

## Local Build Instructions

1. Install the required Python dependency:
   ```bash
   pip install cryptography
   ```
2. Place your unencrypted template in `raw.html` and media files inside the `assets/` directory (both ignored by Git via `.gitignore`).
3. Run the encryption builder script to generate the standalone `index.html`:
   ```bash
   python encrypt_site.py
   ```
4. Deploy the generated `index.html` to any static hosting provider.