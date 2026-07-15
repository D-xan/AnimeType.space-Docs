# 💬 Real-time Chat & Messaging

## System Architecture
AnimeType's messaging infrastructure is built on top of **Firebase Realtime Database / Firestore**, providing millisecond-latency message delivery.

### ⚡ Performance Optimizations
- **Avatar Caching**: To prevent UI stuttering, high-priority chat notifications instantly render avatars by utilizing local `File` caching (`getCacheDir()`) on Android, completely avoiding blocking network calls (`HttpURLConnection`) on the main thread.
- **Display Name Syncing**: Display names are cached via `SharedPreferences` to ensure instantaneous UI hydration upon opening a chat thread.

### 👁️ Read Receipts & Syncing
Read receipts are critical for a modern messaging experience.
- **Mark As Read (Native Sync)**: When a user triggers a "Mark As Read" intent via a native Android notification, a dedicated Cloud Function is pinged.
- **Batch Processing**: The Cloud Function iterates over subcollections (e.g., `chat_messages`), querying `where("isRead", "==", false)` and executes a `batch.update` to set `isRead: true`. This ensures frontend UI state remains perfectly synced on the next load.

### ❓ FAQ
**How does AnimeType handle offline messaging?**
Firebase's offline persistence is enabled by default. Messages sent while offline are queued locally and automatically dispatch the moment network connectivity is restored.\n