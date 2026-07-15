# 🔔 Push Notifications

## Android FCM Integration
AnimeType leverages **Capacitor** and **Firebase Cloud Messaging (FCM)** to deliver rich, native push notifications to Android devices.

### 🛡️ Solving Notification Duplication
A common pitfall with native FCM is the delivery of identical data payloads multiple times, resulting in duplicate UI renders in the `NotificationCompat.MessagingStyle` tray.
- **The Solution**: The custom `FirebaseMessagingService` (`ChatMessagingService.java`) maintains a strict `HashSet<String>` of processed `messageId`s in memory. If a payload arrives with an already-processed ID, it is immediately discarded.

### 🎨 Native Notification UI
AnimeType utilizes Android's `MessagingStyle` to provide a premium chat notification experience:
- Inline replies directly from the notification tray.
- Instant avatar rendering using locally cached Bitmaps.
- Automatic grouping of messages by chat thread.

### ❓ FAQ
**Why do some avatars not load immediately in notifications?**
If an avatar is not yet cached in the local filesystem, the service gracefully falls back to a default placeholder to ensure the notification is posted instantly without blocking the thread. The image is then fetched asynchronously for future use.\n