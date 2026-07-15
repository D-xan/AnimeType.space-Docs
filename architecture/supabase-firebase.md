# 🏛️ Hybrid Database Architecture

## Why Supabase + Firebase?
AnimeType utilizes a hybrid backend approach to leverage the best of both relational and NoSQL paradigms.

### 🐘 Supabase (PostgreSQL)
**Use Cases:** Core application state, user identities, post indexing, and complex relational queries.
- **Search & Discovery**: PostgreSQL's powerful indexing allows for rapid full-text search across posts, theories, and users.
- **Relational Integrity**: Ensures strict foreign-key constraints between users, posts, and their respective interactions.

### 🔥 Firebase (Firestore / Realtime)
**Use Cases:** Ephemeral state, chat messaging, and native push notifications.
- **Real-time Chat**: Firestore's snapshot listeners provide unbeatable latency for chat interfaces.
- **FCM (Firebase Cloud Messaging)**: Deep integration for Android push notifications and background data payloads.

### ❓ FAQ
**How do you sync state between Supabase and Firebase?**
AnimeType uses lightweight Cloud Functions and Webhooks. For example, when a user updates their avatar in Supabase, a webhook pushes the updated URL to Firebase to ensure chat avatars remain in sync.\n