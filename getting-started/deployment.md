# 🚀 Deployment Strategy

## Web Deployment (Vercel)
AnimeType's web client is heavily optimized for edge deployment on **Vercel**.

### CI/CD Pipeline
1. Any push to the `main` branch automatically triggers a Vercel build.
2. Vercel executes `npm run build` (`tsc && vite build`).
3. The Vite build generates static HTML, CSS, JavaScript chunks, and the PWA Service Worker.
4. The artifacts are globally distributed across the Vercel Edge Network.

*Important Note*: Because Vercel defaults to stripping `devDependencies` during production builds, critical build tools (`vite`, `tsc`) must reside in the standard `dependencies` block in `package.json`.

## Android Deployment (Capacitor)
AnimeType compiles natively to Android using **Ionic Capacitor**.

### Build Process
1. Build the web assets:
   ```bash
   npm run build
   ```
2. Sync assets to the Android project:
   ```bash
   npx cap sync android
   ```
3. Open Android Studio to build the Signed APK / AAB:
   ```bash
   npx cap open android
   ```

### ❓ FAQ
**How do push notifications work on the deployed Android app?**
The native Capacitor Android app is bundled with `google-services.json`. The web codebase uses `@capacitor/push-notifications` to interface directly with FCM on the device level.\n