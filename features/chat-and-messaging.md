# 💬 Real-time Chat & Messaging Architecture

## The Foundation of Modern Communication

In the highly connected landscape of modern social networks, asynchronous communication (like email or traditional forums) is no longer sufficient. Users expect lightning-fast, real-time messaging with instant feedback, read receipts, typing indicators, and rich media support. 

To deliver this world-class messaging experience, AnimeType's chat infrastructure bypasses traditional REST APIs and relational database polling entirely, opting instead for a highly optimized WebSocket architecture built on top of **Firebase Realtime Database** and **Firestore**.

## ⚡ Performance Optimizations and Avatar Caching

One of the most common performance bottlenecks in modern chat applications is the handling of user avatars (profile pictures). In a busy chat thread, dozens of messages can stream in per second. If the application attempts to perform a blocking network call to download the avatar image for every incoming message, the main thread will inevitably stall, resulting in severe UI stuttering and a degraded user experience.

### Local Filesystem Caching Strategy
To ensure 60fps scrolling and instant UI hydration, AnimeType implements an aggressive local caching strategy.
1. **Network Interception**: When a message arrives, the system checks if the sender's avatar URL is already cached in the device's local filesystem (using `getCacheDir()` on Android).
2. **Instant Rendering**: If the image exists locally, the Bitmap is decoded instantly from disk and rendered to the UI, completely bypassing the network stack.
3. **Asynchronous Fallback**: If the image is missing, the UI immediately renders a lightweight, color-coded placeholder initials avatar. A background thread is then dispatched to fetch the image via `HttpURLConnection`, write it to the local cache, and gracefully swap out the placeholder once the download completes.

This separation of network fetching from UI rendering guarantees that the chat thread remains buttery smooth, regardless of the user's internet connection speed.

## 👁️ Advanced Read Receipts & State Synchronization

A modern chat application is defined by its ability to accurately and instantly report message states (Sent, Delivered, Read). Implementing reliable read receipts in a distributed, multi-device environment is a complex engineering challenge.

### The Problem with Client-Side Updates
If User A reads a message from User B, User A's client could directly update the database to mark the message as `isRead: true`. However, this approach is inherently flawed. If User A reads the message via a native Android Push Notification without actually opening the app, the client-side JavaScript never executes, and User B never receives their read receipt.

### The Cloud Function Batch Processing Solution
To solve this, AnimeType centralizes the "Mark As Read" logic using **Firebase Cloud Functions**.

When a user triggers a read event (either by opening the chat window or clicking "Mark As Read" directly on a push notification), the client sends a lightweight ping to a dedicated Cloud Function, passing the `chatId` and the `userId`.

The Cloud Function executes a highly efficient batch operation directly on the Firestore database:
1. It queries the `chat_messages` subcollection for the specific thread.
2. It filters the query: `where("receiverId", "==", currentUserId)` and `where("isRead", "==", false)`.
3. It iterates over the results and appends them to a Firestore `WriteBatch`.
4. It executes the batch update, atomically changing `isRead: true` across all unread messages.

Because this logic runs securely on the backend, it ensures that read state is perfectly synchronized across all of the user's linked devices instantaneously. Furthermore, because Firebase utilizes snapshot listeners on the frontend, User B's UI updates in real-time, instantly shifting the message status icons from "Delivered" to "Read."

## 📡 Offline Persistence and Optimistic UI

Network reliability is unpredictable, especially on mobile devices. AnimeType's chat architecture is designed to be fully "Offline-First."

- **Optimistic Updates**: When a user types a message and hits send, the message is immediately appended to the local DOM, giving the illusion of zero-latency transmission.
- **Queueing Mutations**: Behind the scenes, Firebase queues the database mutation. If the device is offline, the message remains safely in the local queue. 
- **Automatic Dispatch**: The moment network connectivity is restored (even if the app has been backgrounded), the queued mutations are automatically flushed to the server, ensuring that no message is ever lost.

By combining offline persistence, server-side batch processing for read receipts, and aggressive local filesystem caching for media, AnimeType delivers a messaging experience that rivals industry leaders.\n