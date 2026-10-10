# Wooden Chess — free installable PWA

A responsive wooden-themed chess app with legal move validation, check/checkmate/draw detection, promotion, move history, undo for local play, board flip, and room-code online play.

## Run locally

PWA service workers require localhost or HTTPS. You can run a local server on a computer:

1. Install Python 3 if needed.
2. Open a terminal in this folder and run `python -m http.server 8000`.
3. Open `http://localhost:8000` in your browser.

The chess rules library is loaded from `esm.sh` and Firebase's web SDK from Google's CDN, so first launch requires internet. The app shell is cached for offline reopening, but online play requires internet.

## Publish free so you can install it on Android

Use a free static host such as GitHub Pages or Firebase Hosting. Upload the contents of this folder (not the folder itself if the host asks for the site root). HTTPS is required for PWA installation. Open the hosted URL in Chrome on Android, then browser menu → **Install app** or **Add to Home screen**.

## Enable online rooms (Firebase free Spark plan)

Online play needs a Firebase project; it is not pre-connected because each project has its own config.

1. Create a project at https://console.firebase.google.com/ and stay on the Spark (no-cost) plan.
2. Add a Web app in Project settings and copy its config into `firebase-config.js` (edit `firebase-config.js` and paste the config values).
3. Enable **Realtime Database** and create the database.
4. For a quick private test only, use the rules below. They allow public reads/writes, so anyone who knows/guesses room IDs could alter games. Do not use these rules for a public production app; use Firebase Authentication and restrictive rules before sharing widely.

Temporary testing rules:

```json
{
  "rules": {
    "rooms": {
      "$room": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

5. Deploy the full folder to an HTTPS static host. Both players open the same site. White taps **Create room** and shares the displayed code; Black enters the code and taps **Join room**.

## Important limitations

- The local game works without Firebase configuration.
- Online play requires Firebase setup, internet, and a hosted HTTPS URL. This starter is a prototype: room-code access is not secure, online undo is disabled, and creating/joining rooms is intentionally simple. Add Firebase Authentication and validated server-side move rules before treating it as a public competitive service.
- Firebase Spark has no-cost quotas, not an unlimited guarantee; check current pricing and usage in Firebase Console.
- The chess rules library is the open-source `chess.js` package, loaded from its browser ESM CDN. It handles legal moves, castling, en passant, promotion, checkmate, and draw rules.
