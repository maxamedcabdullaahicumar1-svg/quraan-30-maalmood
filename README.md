# Qur'aan 30 Maalmood — PWA

A Somali-first installable Progressive Web App (PWA) for tracking a 30-day Quran reading plan, prayer times, and reading-session completion. Arabic interface is available using the language toggle. Reading progress and settings are stored in `localStorage` on the device.

## What works
- Responsive dashboard and 30-day calendar.
- Sequential allocation of pages 1–604 with no gaps or duplicates: four 21-page days and twenty-six 20-page days. Every day is divided into five prayer sessions; 21-page days have one 5-page session and four 4-page sessions.
- Check/uncheck sessions, progress counts, current page, bookmarks, reset confirmation.
- Prayer times fetched by coordinates from the AlAdhan timings API; GPS permission or manual city selection; calculation method and Asr school settings.
- Browser notification permission and foreground reminders while the app page is running.
- PWA manifest, SVG app icon, service-worker cache for offline shell.
- Somali and Arabic UI, RTL direction, light/dark mode.

## Accuracy and limitations
1. The reading plan guarantees sequential coverage of all 604 page numbers. It does **not** claim that each day corresponds exactly to one printed Juz. A verified page-to-Juz metadata file for a named 604-page Madinah Mushaf edition must be added before presenting Juz boundaries as verified.
2. The Quran text is intentionally not bundled. Do not insert generated or unverified Quran text. Add a legally distributable, verified Uthmani dataset with verified page/ayah boundaries before claiming this PWA contains the complete Mushaf.
3. Prayer times require internet access for refresh from AlAdhan. A previously loaded page shell works offline, but prayer times may become stale until a refresh succeeds. Calculations can differ from the local mosque timetable.
4. Browser notifications are not equivalent to Android native alarms. This PWA's reminders are checked while the page is running and visible/alive. Browsers may suspend timers or block delivery after the app is closed, device restarts, or battery saver activates. Reliable background push requires a push server and subscription storage; exact local alarms generally require a native Android app or wrapper. Do not promise exact delivery.
5. This is a web app, not a signed Android APK. It can be installed from HTTPS on supported Android browsers using the browser menu or the install button.

## Run locally
A service worker and installable PWA require HTTPS or `localhost`; opening `index.html` directly as a `file://` URL is not sufficient.

With Node.js installed:

```bash
npm test
npx --yes http-server . -p 8080
```

Then open `http://localhost:8080` on the same computer. On an Android phone on the same network, use your computer's LAN IP and port 8080 for functional testing, but service-worker installation generally requires HTTPS on the phone. For real install testing, deploy to HTTPS hosting.

## Deploy to free HTTPS hosting
1. Create a repository containing the contents of this folder.
2. Import it into a static hosting provider such as GitHub Pages, Cloudflare Pages, or Netlify.
3. Publish the repository root as the site directory; no build command is needed.
4. Open the resulting HTTPS URL on Android Chrome.
5. Use the browser menu → **Install app** or **Add to Home screen**.

## Android install notes
- The SVG icon is used as the app icon in the PWA manifest. Some platforms prefer 192×192 and 512×512 PNG icons; for broader install prompt compatibility, generate and add those PNGs and reference them in `manifest.webmanifest` before release.
- Geolocation requires user permission and a secure context (HTTPS or localhost).
- Notification permission is also user-controlled and browser-dependent.

## Tests
```bash
npm test
```
Tests cover the 604-page sequential allocation and session prayer order. They do not verify a Quran text dataset, Juz metadata, AlAdhan availability, or background notification delivery.
