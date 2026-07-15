# 🔔 Push Notifications & Native Android Integration

## The Importance of Native Notifications

While Progressive Web Apps (PWAs) have made incredible strides in bridging the gap between web and mobile, relying solely on Web Push Notifications is often insufficient for a social network. Web push support is heavily fragmented across different mobile browsers, often delayed by battery optimization layers, and lacks access to the rich native APIs required for premium UI features like inline replies or grouped messaging.

To provide a truly world-class mobile experience, AnimeType utilizes **Ionic Capacitor** to compile its web codebase into a native Android APK/AAB, integrating deeply with **Firebase Cloud Messaging (FCM)** at the native Java/Kotlin level.

## 🛡️ Solving the FCM Duplicate Payload Architecture Flaw

One of the most notorious and deeply frustrating issues with native Firebase Cloud Messaging on Android is payload duplication. Due to the nature of distributed backend systems and network retries, the FCM infrastructure will frequently deliver the exact same data payload to an Android device multiple times. 

If a chat app blindly processes every incoming `RemoteMessage`, the user will receive multiple notification chimes, and the notification tray will render the exact same message two, three, or even four times. This creates a confusing and unprofessional user experience.

### The HashSet Deduplication Strategy
To completely eradicate this issue, AnimeType implements a custom `FirebaseMessagingService` (`ChatMessagingService.java`) that acts as an intelligent gatekeeper before any UI rendering occurs.

Every incoming FCM payload sent by the AnimeType backend includes a unique, UUID-v4 formatted `messageId`. 

When `onMessageReceived(RemoteMessage remoteMessage)` is triggered by the Android OS, the service executes the following logic:
1. It extracts the `messageId` from the payload data map.
2. It checks against a static, memory-resident `HashSet<String>` containing recently processed IDs.
3. If the `messageId` exists in the HashSet, the service silently drops the payload and terminates execution, completely preventing the duplicate render.
4. If the `messageId` is new, it is added to the HashSet, and the service proceeds to build and display the native notification.

To prevent the HashSet from growing infinitely and causing an OutOfMemory (OOM) exception, the service implements a lightweight cleanup routine that periodically clears the set or utilizes a fixed-size Circular Buffer array for ID tracking.

## 🎨 Crafting a Premium Notification UI (MessagingStyle)

Standard text notifications are inadequate for a chat-heavy application. Users expect to see the sender's avatar, read previous context from the conversation, and have the ability to reply immediately without opening the application.

AnimeType heavily utilizes Android's `NotificationCompat.MessagingStyle` API.

### 1. Instant Avatar Hydration
When a push notification arrives, the native service immediately checks the Android `getCacheDir()` for the sender's avatar. If the bitmap is found, it is instantly attached to the `Person` object in the `MessagingStyle` builder. This ensures that the notification tray looks rich and personalized the moment it drops down from the status bar, without any network blocking.

### 2. Contextual Thread Grouping
Rather than overwriting the previous notification or cluttering the tray with dozens of individual alerts, `MessagingStyle` automatically groups incoming messages by their respective chat threads. The user can expand the notification to read the last 5-10 messages of the conversation, providing crucial context before they decide to reply.

### 3. Native Intent Actions (Mark as Read)
AnimeType integrates custom `PendingIntent` actions directly into the notification layout. If a user swipes the notification away or explicitly taps a "Mark As Read" action button, the native Java service fires an HTTP request to the AnimeType Cloud Functions. This silently updates the Firestore database in the background, ensuring that the sender receives their read receipt without the receiver ever having to open the AnimeType app.

By mastering the intricacies of Android's native APIs and solving the inherent flaws of FCM delivery, AnimeType guarantees a notification experience that is fast, reliable, and exceptionally user-friendly.\n