# 💻 Local Installation & Developer Setup Guide

## Welcome to the AnimeType Codebase

AnimeType is built using a modern, highly optimized TypeScript ecosystem. Setting up the project locally is straightforward, provided your development environment meets the necessary prerequisites. This guide will walk you through cloning the repository, installing dependencies, configuring your local environment variables, and booting up the Vite development server.

## 🛠️ System Prerequisites

Before beginning, ensure your local development machine is equipped with the following tools:
- **Node.js**: Version 18.x or higher is strictly required for compatibility with Vite and the latest Capacitor core packages.
- **Package Manager**: While `npm` works perfectly, `pnpm` is highly recommended for faster installation times and strict dependency resolution.
- **Git**: Required for cloning the repository and managing version control.
- *(Optional but Recommended)* **Android Studio**: If you plan to compile and test the native Android Capacitor application, you must have Android Studio installed along with the latest Android SDKs and build tools.

## 🚀 Step-by-Step Setup

### Step 1: Clone the Repository
Begin by cloning the main application repository from GitHub to your local workspace. Navigate into the `agy` directory, which serves as the root for the Vite application.
```bash
git clone https://github.com/D-xan/AnimeType-Vite.git
cd AnimeType-Vite/agy
```

### Step 2: Install Dependencies
AnimeType relies on a vast array of high-performance libraries (such as Supabase JS, Firebase Admin, Chart.js, and Capacitor). Run the installation command to fetch the dependency tree.
```bash
npm install
```
*Crucial Architecture Note*: Build tools such as `vite`, `typescript`, and `vite-plugin-pwa` have been intentionally moved from `devDependencies` into the standard `dependencies` block in the `package.json`. This is required to prevent Vercel from aggressively stripping them during edge deployments, which would cause the build pipeline to fail with a `127` exit code.

### Step 3: Environment Configuration
The application requires API keys and endpoint URLs to communicate with the hybrid Supabase and Firebase backend. You must configure your local environment securely.
1. Copy the provided example environment file to create your local config.
```bash
cp .env.example .env
```
2. Open the `.env` file in your preferred code editor and populate the necessary variables (e.g., `VITE_SUPABASE_URL`, `VITE_FIREBASE_API_KEY`). Ensure you never commit your active `.env` file to version control.

### Step 4: Booting the Development Server
Once the dependencies are installed and the environment is configured, you are ready to launch the application.

AnimeType utilizes Vite for unparalleled Hot Module Replacement (HMR) speeds. Boot the server using:
```bash
npm run dev
```
The application will instantly compile and become available at `http://localhost:5173`. Any changes you make to the TypeScript or CSS files will reflect in the browser instantly without requiring a full page reload.

## 🧪 Testing the Production Build Locally

Occasionally, you may need to verify that your code survives the minification, tree-shaking, and Rollup chunking processes associated with a production build. You can simulate the Vercel deployment environment locally:

1. Execute the build command to run the TypeScript compiler and Vite bundler.
```bash
npm run build
```
2. Once the static `dist/` directory is successfully generated, launch the production preview server.
```bash
npm run preview
```
This will serve the fully optimized, production-ready bundle locally on `http://localhost:4173`, allowing you to audit performance metrics and ensure that dynamic imports are functioning correctly before pushing your code to the live server.\n