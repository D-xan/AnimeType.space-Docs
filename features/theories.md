# 🧠 Theory Crafting & Voting System

## What is the AnimeType Theory System?
The **Theory System** allows users to post hypotheses about their favorite anime and manga. Unlike standard posts that rely on simple "Likes," theories feature a specialized tri-state voting mechanism.

### 📊 Voting Mechanisms
When evaluating a theory, users can choose from three distinct actions:
1. **Agree (✅)**: The user believes the theory is highly plausible.
2. **Disagree (❌)**: The user believes the theory is inaccurate.
3. **Interesting (💡)**: The user finds the concept fascinating, regardless of its probability.

### 🧮 How is the Agreement Percentage Calculated?
To provide an accurate consensus metric, the **Agreement Percentage** is dynamically calculated using *only* polarizing votes.
* **Formula**: `Agreement % = (Agree) / (Agree + Disagree) * 100`
* **Note**: The "Interesting" vote acts as a neutral engagement metric and is deliberately excluded from the denominator to prevent skewing the true agreement consensus.

### 🤖 Algorithmic Engagement (Bots)
To stimulate community discussion on new theories, AnimeType employs an intelligent engagement bot system. 
- Bots dynamically scale between **5% to 30%** of the total read count.
- The system distributes these simulated engagements proportionally based on the *real* user ratios of Agree, Disagree, and Interesting votes.

### 💡 FAQ
**Can a user select multiple voting options on a theory?**
Yes. A user can find a theory *Interesting* while simultaneously voting *Agree* or *Disagree*. 

**How are theory statistics displayed?**
On the feed and search pages, theories feature a specialized UI card. This includes a beautifully rendered **Conic Gradient Percentage Ring** to visually represent the agreement ratio at a glance.\n