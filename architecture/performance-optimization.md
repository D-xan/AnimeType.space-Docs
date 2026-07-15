# 🏎️ Performance & Caching

## Frontend Optimizations

### 1. Vite & Code Splitting
AnimeType is built with **Vite**, utilizing aggressive code-splitting. Components like `searchUI`, `feedUI`, and heavy chart libraries are dynamically imported only when required, keeping the initial JavaScript bundle incredibly small.

### 2. Progressive Web App (PWA)
The app is fully configured as a PWA using `vite-plugin-pwa`:
- **Service Worker Caching**: Static assets (JS, CSS, SVGs) are precached. 
- **Offline Mode**: The UI gracefully handles network drops, displaying a "Ready for offline use" state while queuing mutations.

### 3. DOM & State Management
- **Pre-computed Subsets**: UI components are bound to pre-computed datasets (like the 7/30/365 analytics subsets) to avoid heavy JavaScript filtering on the main thread during render.
- **Lazy Loading Media**: Avatars and post images utilize `blurhash` for skeleton loading and native browser `loading="lazy"` attributes.

### ❓ FAQ
**Why do I see a blank screen after a deployment update?**
If a service worker caches the old `index.html`, it may try to request outdated JS chunks. Simply hard-refreshing (`Ctrl+Shift+R`) or clearing the mobile cache will prompt the service worker to fetch the latest Vercel build.\n