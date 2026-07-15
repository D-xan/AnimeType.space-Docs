# 🔍 Search & Discovery

## Intelligent Search
AnimeType features a highly responsive search engine that allows users to instantly discover Posts, Theories, and other Users.

### 🎨 Specialized UI Cards
The search feed dynamically adapts its rendering logic based on the `post.type`.
- **Standard Posts**: Displays standard engagement metrics (Likes, Comments).
- **Theory Posts**: Replaces the standard "Like" UI with a customized Theory layout. This includes:
  - 📖 Total Reads
  - 💬 Total Discussions
  - 🔄 Shares & Saves
  - 📈 A visual **Percentage Ring** indicating the Agreement Ratio.
  - ✅ Raw counts for Agree and Interesting votes.

### 🏷️ Tag Filtering
Search results automatically extract and highlight relevant hashtags, stripping system-level tags (like `##internal`) to present a clean UI to the user.

### ❓ FAQ
**Does the search bar support fuzzy matching?**
Yes. Local client-side filtering utilizes `fuse.js` to provide typo-tolerant fuzzy searching for usernames and tags before falling back to full-text database queries.\n