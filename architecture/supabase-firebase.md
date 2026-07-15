# 🏛️ Hybrid Database Architecture: Supabase & Firebase

## The Dilemma of Database Selection

When architecting a modern, highly interactive social network, engineering teams are almost always forced to choose between two fundamentally different database paradigms:
1. **Relational Databases (SQL)**: Excellent for strict data integrity, complex table joins, foreign key constraints, and powerful indexing (e.g., PostgreSQL). However, they traditionally struggle with pushing real-time, low-latency updates to thousands of connected clients.
2. **NoSQL / Realtime Databases**: Unbeatable for millisecond-latency state synchronization, document-based flexibility, and offline mobile support (e.g., Firebase, MongoDB). However, they lack strict relational integrity and make complex querying and full-text search exceedingly difficult.

Rather than compromising on features, AnimeType utilizes a **Hybrid Backend Architecture**, leveraging both Supabase (PostgreSQL) and Firebase to utilize the absolute best of both worlds.

## 🐘 Supabase (PostgreSQL) - The Relational Core

Supabase serves as the absolute source of truth for AnimeType's core application state. It manages all data that requires strict relational integrity and complex querying.

### Key Use Cases
- **User Identity & Authentication**: Supabase Auth handles secure user registration, session management, and JWT generation, backed by strict Row Level Security (RLS) policies in PostgreSQL to guarantee data privacy.
- **Post & Theory Indexing**: The vast majority of user-generated content (Posts, Theories, Comments) is stored in Supabase. This allows the backend to enforce strict foreign key constraints (e.g., a Comment cannot exist if the parent Post is deleted).
- **Search & Discovery**: AnimeType heavily utilizes PostgreSQL's highly advanced Full Text Search (FTS) capabilities. By indexing content vectors in Supabase, the platform can perform lightning-fast, highly accurate searches across thousands of text records—a feat that is notoriously difficult and expensive to achieve purely in Firebase.

## 🔥 Firebase - The Real-Time Ephemeral Engine

While Supabase handles the heavy lifting of persistent state, Firebase (Firestore and Realtime Database) is deployed specifically to handle highly ephemeral, high-velocity data streams that require instantaneous client synchronization.

### Key Use Cases
- **Real-time Chat Infrastructure**: Firestore's snapshot listeners are unparalleled in the industry. By storing chat threads in Firebase, AnimeType clients maintain active WebSockets that instantly push new messages, typing indicators, and read receipts to the UI with zero polling overhead.
- **FCM (Firebase Cloud Messaging)**: Because AnimeType compiles to a native Android app via Capacitor, integrating deeply with FCM is a strict requirement for reliable, OS-level push notifications. Firebase serves as the central nervous system for dispatching these payloads to devices.

## 🔄 Maintaining State Synchronization

The primary challenge of a hybrid architecture is ensuring that data does not become desynchronized between the two platforms. AnimeType solves this using lightweight, event-driven Webhooks and Cloud Functions.

For example, when a user changes their display name or updates their avatar image, they submit the mutation to Supabase. PostgreSQL successfully updates the relational record. Immediately upon success, a Supabase Database Webhook fires, triggering a serverless edge function. This function securely patches the user's cached profile data inside Firebase Firestore. 

This ensures that the real-time chat UI (powered by Firebase) instantly reflects the updated avatar, while the core relational database (Supabase) remains the uncorrupted source of truth. By strictly defining the boundaries and responsibilities of each database, AnimeType achieves a level of performance and feature richness that a monolithic database architecture could never support.\n