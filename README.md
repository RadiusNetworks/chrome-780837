# chrome-pwa-sad-face-test

[CRBUG#780837](https://bugs.chromium.org/p/chromium/issues/detail?id=780837)

This is a test application to demonstrate problems with crashing behavior when loading a progressive web app (PWA) inside of a Chrome Kiosk App.

The Chrome App is a single HTML page that loads a PWA in a `webview`:
```
  <body>
    <main>
      <webview src="https://s3.amazonaws.com/displaykit-assets-development/testing/swtest3/index.html" partition="persist:displaykit">
    </main>
  </body>
```

The [PWA source code](pwa_webapp_code) is contained in this repository, and it can be loaded from this remote URL: https://s3.amazonaws.com/displaykit-assets-development/testing/swtest3/index.html

The PWA is a single HTML page that loads three different video templates inside `iframe` tags:
```
  <html>
    <main>
      <iframe src="turbines.html" scrolling="no" sandbox="allow-scripts allow-same-origin"></iframe>
    </main>
  </html>
```

turbines.html (inside the `iframe`):
```
  <html>
    <body>
      <video id="video" autoplay>
        <source src="turbines.webm" autoplay loop>
      </video>
    </body>
  </html>
```

## Expected behavior

The app should run indefinitely, playing three simple videos repeatedly for an extended period of time.

## Observed Behavior

After running for an extended period of time (8+ hours) the webview appears to crash, showing a "sad face" error screen (see image below).  Inspecting the app with dev tools reveals that the webview is still present in the DOM, but the process associated with the webview is no longer present in the "Inspectable pages" list.

The Chrome app is still running but the PWA inside the `webview` tag is no longer running and appears to have crashed.

![sad-face](https://i.imgur.com/mkNoIwA.png)
