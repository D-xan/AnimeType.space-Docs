<div align="center">
  <img src="https://img.shields.io/badge/AnimeType-Official_Documentation-a855f7?style=for-the-badge&logo=appveyor" alt="AnimeType Documentation" />
  <h1>📚 AnimeType Documentation Hub</h1>
  <p><strong>The definitive guide to the architecture, features, and algorithms powering AnimeType space.</strong></p>
</div>

---

## 🌟 Introduction to AnimeType

Welcome to the official documentation for **AnimeType**, a next-generation social network and community platform exclusively designed for anime, manga, and light novel enthusiasts. Unlike generic social media platforms, AnimeType is custom-built from the ground up to cater specifically to the deep, lore-heavy, and highly interactive nature of the anime community. 

Our mission is to foster a space where passionate fans can share their artwork, debate complex plot theories, connect with like-minded individuals in real-time, and build a lasting reputation based on the quality of their contributions. To achieve this, AnimeType relies on a highly sophisticated, hybrid technology stack that prioritizes performance, real-time interactivity, and offline accessibility.

## 🏗️ The Technology Stack

AnimeType is built using a modern, scalable, and highly responsive architecture designed to handle thousands of concurrent connections while delivering a seamless user experience across both web and mobile platforms.

- **Frontend Architecture (Vite & TypeScript)**: The web client is built using Vite, offering lightning-fast Hot Module Replacement (HMR) during development and highly optimized Rollup-based bundling for production. The entire codebase is strictly typed with TypeScript to ensure maintainability and reduce runtime errors.
- **Mobile Integration (Capacitor)**: AnimeType is a true Progressive Web App (PWA) that compiles into a native Android application using Ionic Capacitor. This allows the platform to access native device features—such as the filesystem for image caching and the native notification tray—while maintaining a single, unified codebase.
- **Hybrid Backend (Supabase & Firebase)**: We utilize a dual-backend strategy to get the best of both relational and NoSQL worlds. Supabase (powered by PostgreSQL) handles our core relational data, user authentication identities, and complex search indexing. Meanwhile, Firebase (Firestore and Realtime Database) is leveraged for its unbeatable low-latency WebSocket connections, which power our real-time chat and push notification delivery systems.

## 📖 How to Use This Documentation

This documentation hub is structured to provide both high-level overviews for product managers and deep, technical deep-dives for software engineers. Whether you are looking to set up the project locally, understand the mathematics behind our algorithmic engagement systems, or learn how we handle native Android notification deduplication, you will find it here.

### 🚀 Quick Start Guides
- **[Local Installation Guide](./getting-started/installation.md)**: A comprehensive walkthrough for cloning the repository, installing dependencies, configuring environment variables, and starting the local Vite development server.
- **[Deployment Strategy](./getting-started/deployment.md)**: Detailed instructions on our CI/CD pipeline, Vercel edge deployment optimizations, and the Capacitor build process for compiling Android APKs and App Bundles (AAB).

### ✨ Core Features & Systems
- **[Theory Crafting & Voting](./features/theories.md)**: Dive into the mechanics of our unique tri-state voting system (Agree, Disagree, Interesting) and the mathematics behind the Agreement Percentage Ring.
- **[Algorithmic Engagement System](./features/engagement-system.md)**: Understand how AnimeType solves the "cold start" problem for new content creators using dynamically scaled, algorithmically generated engagement metrics that mirror real user behavior.
- **[Real-time Chat & Messaging](./features/chat-and-messaging.md)**: Explore the architecture of our millisecond-latency chat system, including offline persistence, optimistic UI updates, and our highly efficient batch-processing strategy for read receipts.
- **[Push Notifications](./features/push-notifications.md)**: Learn how we integrate Firebase Cloud Messaging (FCM) with Capacitor, utilize Android's `MessagingStyle` for premium native UI, and solve the infamous FCM duplicate payload issue using memory-based HashSets.
- **[User Profiles & Analytics](./features/user-profiles-and-analytics.md)**: Discover how we provide users with deep insights into their content performance using single-pass client-side computations and stale-while-revalidate document caching.
- **[Search & Discovery](./features/search-and-discovery.md)**: An overview of our dual-layered search infrastructure, combining client-side fuzzy matching via Fuse.js with powerful PostgreSQL Full Text Search (FTS) for scalable discovery.

### 🏗️ Architectural Deep Dives
- **[Database Architecture](./architecture/supabase-firebase.md)**: A detailed breakdown of why we chose a hybrid Supabase + Firebase approach, the specific use cases for each database, and how we maintain state synchronization across both platforms via webhooks and edge functions.
- **[Performance & Caching](./architecture/performance-optimization.md)**: Our masterclass on frontend optimization, detailing our use of dynamic imports for code splitting, Blurhash for skeleton media loading, and Service Worker precaching for offline PWA support.

## 🤝 Community & Contribution

AnimeType thrives on community feedback and open collaboration. We encourage developers, designers, and anime fans to contribute to the platform's ongoing evolution. Please review our issue tracker for planned features, and ensure that any pull requests strictly adhere to our TypeScript guidelines and architectural patterns outlined in this documentation.

Thank you for being a part of the AnimeType journey. Together, we are building the ultimate digital home for the anime community.\n