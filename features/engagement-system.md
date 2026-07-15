# 📈 Algorithmic Engagement System

## Overview
AnimeType utilizes a dynamic engagement simulation system to ensure content never feels stagnant. By intelligently supplementing real user interactions, the platform encourages organic growth and social proof.

### ⚙️ How the Engagement Engine Works
The engagement engine relies on a smart algorithmic ratio based on the total **View/Read Count** of a post.

1. **Dynamic Scaling**: Engagement is calculated randomly between **5% and 30%** of total views.
   - *Example*: A post with 1,000 views will generate between 50 and 300 supplemental engagements.
2. **Proportional Distribution**: The engine analyzes the *real* distribution of interactions (e.g., if real users lean heavily towards "Agree", the engine mirrors this ratio for the supplemental votes).

### 🛡️ Why Use Simulated Engagement?
- **Cold Start Problem**: Helps new posts gain traction by providing initial social proof.
- **Community Stimulation**: Users are more likely to participate in discussions that already show signs of life.
- **Algorithmic Integrity**: Because the system scales strictly relative to *actual views*, it never outpaces the organic reach of a post.

### ❓ FAQ
**Does simulated engagement overwrite real votes?**
No. The system keeps raw user data and supplemental engagement strictly separated in the database. The UI seamlessly merges them (`realCount + botCount`) at render time for a unified user experience.

**Are these bots active on all content?**
The engagement engine is primarily focused on **Posts** and **Theories**, scaling proportionally to their read counts.\n