# 👤 User Profiles & Analytics

## Profile Dashboard
Every AnimeType user has access to a comprehensive profile dashboard that tracks their reputation, badges, and content performance.

### 📊 Analytics Caching Architecture
Querying raw post collections (`userPosts`) every time a profile mounts is computationally expensive and slow.
- **Document-level Caching**: Analytics are cached natively inside the user's root document (under `analytics_history`).
- **Timeframe Processing**: The system standardizes logic to compute `7`, `30`, `365`, and `all` days in a **single client-side pass**. 
- **Stale-While-Revalidate**: If the cache is older than 1 hour, a background refresh is triggered. UI buttons are bound directly to these pre-computed subsets, allowing instant time-range switching without redrawing the DOM from scratch.

### 🏆 Badge & Reputation System
Users earn badges based on their engagement metrics (likes, theories proposed, active discussions). These badges are dynamically fetched and rendered in a highly optimized horizontal scroll view.

### ❓ FAQ
**How often do my profile analytics update?**
Analytics are cached for **1 hour** to conserve database reads. You will see near real-time data, but heavy aggregations refresh hourly.\n