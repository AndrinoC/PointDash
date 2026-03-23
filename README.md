# Dots & Boxes Online — setup notes

This HTML file works in two modes:

- **Local**: no backend required.
- **Online**: uses **GitHub Pages** for static hosting and **Firebase Realtime Database** + **Firebase Anonymous Auth** for the room code system.

## Quick deploy

1. Rename `point_grid_lines_v4.html` to `index.html`.
2. Upload it to a GitHub repository.
3. Turn on **GitHub Pages** for that repo.
4. Create a Firebase project.
5. In Firebase:
   - add a **Web app**
   - enable **Anonymous Authentication**
   - create a **Realtime Database**
6. Copy your Firebase web config JSON.
7. Either:
   - paste the config into the app's setup box once per browser, or
   - replace the placeholder `DEFAULT_FIREBASE_CONFIG` object inside the HTML before publishing.

## Recommended Firebase Realtime Database rules

These rules are intentionally simple for a small casual game. They require users to be authenticated, which works with anonymous auth.

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "rooms": {
      "$code": {
        ".read": "auth != null",
        ".write": "auth != null"
      }
    }
  }
}
```

## How the invite flow works

- Host clicks **Create room**.
- The app generates a short room code.
- Host shares:
  - the website URL
  - the room code
- Guest opens the same website and joins with that code.

The host is **Blue** and the guest is **Red**.

## Gameplay rules included

- sizes: **8×8**, **12×12**, **16×16**, **20×20**
- only **adjacent horizontal/vertical** segments can be placed
- hovering previews a faded line
- clicking locks the line in
- line colors alternate **blue / red**
- if a move closes a box:
  - the box fills with that color
  - the same player goes again
- already-filled lines are removed from the hit layer, so they cannot be clicked again

## Notes

- **Undo** is available only in local play.
- **Restart** resets the current board. In online play, both players see the reset.
- This is designed to be static-host friendly, so it does not need a custom server.
