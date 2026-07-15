# 🚀 Global Deployment Strategy

## Architecting for Scale

When an application is tasked with serving real-time chat, heavy image assets, and complex interactive DOM elements to a global audience of anime fans, standard shared-hosting deployment strategies are entirely insufficient. 

To guarantee low latency, maximum uptime, and rapid iteration cycles, AnimeType employs a highly automated, dual-pronged deployment strategy. The web client is distributed globally via the **Vercel Edge Network**, while the native mobile application is compiled and distributed using **Ionic Capacitor** and Android Studio.

## 🌐 Web Deployment via Vercel CI/CD

AnimeType's web infrastructure is deeply integrated with Vercel to provide a zero-configuration, heavily optimized Continuous Integration and Continuous Deployment (CI/CD) pipeline.

### The Deployment Pipeline
1. **Trigger**: Any commit pushed to the `main` branch on the GitHub repository acts as a webhook trigger, instantly initiating a new Vercel build process.
2. **Execution**: Vercel spins up an isolated build container, installs the `dependencies` defined in the `package.json`, and executes the build script: `npm run build` (which maps to `tsc && vite build`).
3. **Compilation**: The TypeScript compiler (`tsc`) performs strict type-checking across the entire codebase. If it passes, Vite takes over, utilizing Rollup to aggressively tree-shake unused code, minify JavaScript chunks, optimize CSS, and generate the Progressive Web App (PWA) Service Worker via `vite-plugin-pwa`.
4. **Distribution**: Once the static assets are generated in the `dist/` directory, Vercel globally distributes them across its massive Edge CDN. This ensures that a user in Tokyo downloads the assets from a Japanese server, while a user in New York downloads from a US server, guaranteeing millisecond load times globally.

### Dependency Safety Protocol
Because Vercel automatically detects the production environment and strips out `devDependencies` prior to the build execution, critical tools like `vite` and `typescript` must reside in the standard `dependencies` block. Failing to adhere to this protocol will result in Vercel throwing an `exit code: 127 (command not found)`, crashing the deployment pipeline.

## 📱 Native Android Deployment via Capacitor

AnimeType is not just a website; it is a fully functioning Android application. We achieve this without maintaining a separate Java/Kotlin codebase by utilizing Ionic Capacitor, which bridges our optimized web assets to native Android APIs.

### The Mobile Build Process
To deploy a new version of the Android application to the Google Play Store, developers must follow a strict compilation workflow:

1. **Build Web Assets**: First, compile the latest production-ready web assets.
   ```bash
   npm run build
   ```
2. **Capacitor Synchronization**: Sync the newly generated `dist/` folder into the native Android project structure. This command also automatically updates any native dependencies and plugins (such as Firebase Cloud Messaging or local filesystem access).
   ```bash
   npx cap sync android
   ```
3. **Native Compilation**: Open the synchronized project in Android Studio to perform the final native build.
   ```bash
   npx cap open android
   ```
From within Android Studio, the engineering team can generate the digitally signed APK (Android Package) or AAB (Android App Bundle). This package includes the `google-services.json` file, securely linking the native application to our Firebase infrastructure, which powers our hardware-level Push Notifications and real-time data syncs. 

By utilizing Vercel for instant web distribution and Capacitor for native compilation, AnimeType ensures that feature updates are delivered to all users, regardless of their device, with unparalleled speed and reliability.\n