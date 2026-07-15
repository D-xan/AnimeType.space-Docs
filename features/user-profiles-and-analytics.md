# 👤 User Profiles, Analytics, & Reputation

## The Profile Dashboard Philosophy

In a community-driven social network, a user's profile is their digital identity. It must go beyond a simple list of recent posts; it needs to serve as a comprehensive dashboard that reflects their reputation, highlights their achievements, and provides deep analytical insights into how the community is interacting with their content.

To achieve this, the AnimeType Profile Dashboard is engineered to display complex aggregated data—such as total views across all posts, lifetime agreement ratios on theories, and engagement growth over specific time periods—while maintaining instantaneous load times.

## 📊 Analytics Caching Architecture & Timeframe Processing

Calculating comprehensive user analytics is a highly computationally expensive operation. If a user has authored 500 posts, dynamically querying the database to sum the views, likes, and theory votes across all 500 documents every time their profile is loaded would result in severe database strain, massive bandwidth consumption, and unacceptable UI loading spinners.

### Document-Level Pre-aggregation
To solve this, AnimeType implements a strict document-level caching architecture. Instead of querying raw collections, the backend periodically aggregates the user's metrics and writes a highly optimized, structured payload directly into the user's root document under the `analytics_history` property.

### Single-Pass Client-Side Computations
A crucial feature of the analytics dashboard is the ability to filter data by timeframe: **7 Days**, **30 Days**, **365 Days**, and **All Time**.

If the frontend had to make a new network request to the backend every time the user clicked a different timeframe button, the UI would feel sluggish and unresponsive. Instead, AnimeType relies on a **single client-side pass** architecture:

1. When the profile mounts, the frontend fetches the pre-aggregated `analytics_history` cache once.
2. The JavaScript engine executes a lightning-fast array reduction, computing the mathematical totals for the 7, 30, 365, and All-Time windows simultaneously in memory.
3. The UI buttons (7D, 30D, etc.) are then bound directly to these pre-computed, in-memory datasets. 
4. When a user clicks a button, the DOM updates instantly, swapping the chart data and stat cards without a single network request or recalculation.

### Stale-While-Revalidate Caching
To balance data freshness with database cost efficiency, the `analytics_history` cache implements a 1-hour Stale-While-Revalidate TTL (Time To Live). When a profile is viewed, the system checks the timestamp of the cache. If it is older than 60 minutes, the UI immediately renders the cached data (providing instant feedback) while silently dispatching a background worker to recalculate the metrics and update the database for the next visitor.

## 🏆 The Badge and Reputation System

Engagement shouldn't just be numerical; it should be rewarding. AnimeType features a dynamic Badge & Reputation system designed to gamify community contribution.

Users earn badges based on specific engagement milestones:
- **The Theorist**: Awarded for proposing theories that achieve a high agreement consensus.
- **The Conversationalist**: Awarded for maintaining highly active discussion threads.
- **The Architect**: Awarded for sustained, long-term contribution to the platform.

### Horizontal Scroll Optimization
Badges are displayed prominently at the top of the profile using a highly optimized horizontal scroll view. Because mobile devices often struggle with complex CSS reflows during horizontal swiping, the badge container is hardware-accelerated (`transform: translateZ(0)`), ensuring butter-smooth 60fps scrolling even on low-end Android devices.

By combining heavy backend pre-aggregation, intelligent client-side memory management, and rewarding gamification, the AnimeType profile dashboard elevates the user experience from a simple feed to a comprehensive digital identity.\n