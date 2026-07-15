# 💻 Local Installation Guide

## Prerequisites
- Node.js (v18 or higher)
- npm or pnpm
- Git

## Step-by-step Setup
1. **Clone the Repository**
   ```bash
   git clone https://github.com/D-xan/AnimeType-Vite.git
   cd AnimeType-Vite/agy
   ```

2. **Install Dependencies**
   AnimeType uses a heavily optimized dependency tree. Ensure you install both standard and dev dependencies.
   ```bash
   npm install
   ```
   *(Note: Build tools like `vite`, `typescript`, and `vite-plugin-pwa` are listed under standard dependencies to ensure Vercel compatibility).*

3. **Environment Configuration**
   Copy the example environment file and populate it with your Supabase and Firebase keys.
   ```bash
   cp .env.example .env
   ```

4. **Start the Development Server**
   ```bash
   npm run dev
   ```
   The application will be available at `http://localhost:5173`.

### ❓ FAQ
**Can I run the production build locally?**
Yes. Run `npm run build` followed by `npm run preview`. This will serve the minified, production-ready bundle on `http://localhost:4173`.\n