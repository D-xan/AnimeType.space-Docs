# 🧠 Theory Crafting & Voting System

## The Philosophy of the Theory System

In traditional social networks, the primary method of engagement is a simple binary action: a "Like" or a "Heart." While this mechanism works exceptionally well for sharing artwork, general status updates, or memes, it fundamentally fails when applied to analytical, lore-heavy discussions. 

The anime community thrives on speculation. Fans spend countless hours analyzing foreshadowing, dissecting character motivations, and predicting future plot twists in their favorite manga and anime series. When a user posts a complex hypothesis, a simple "Like" is ambiguous. Does a "Like" mean the reader agrees with the theory? Does it mean they think the theory is completely wrong but appreciate the effort? Or does it mean they simply found the concept entertaining?

To solve this ambiguity and provide true consensus metrics, AnimeType introduced the **Theory Crafting System**, a specialized content type featuring a custom tri-state voting mechanism designed specifically for hypothesis evaluation.

## 📊 The Tri-State Voting Mechanism

When evaluating a theory post on AnimeType, users are presented with three distinct actions, replacing the standard "Like" button:

1. **Agree (✅)**: The user has read the theory, analyzed the provided evidence, and believes the hypothesis is highly plausible or factually correct based on the current lore.
2. **Disagree (❌)**: The user believes the theory is inaccurate, contradicts established canon, or lacks sufficient evidence. 
3. **Interesting (💡)**: The user finds the concept fascinating, entertaining, or thought-provoking, regardless of its actual probability of being true. This serves as a vital engagement metric that rewards creativity without skewing the factual consensus.

Users are permitted to cast multiple types of votes simultaneously. For example, a user might find a theory highly *Interesting* but ultimately vote *Disagree* because it contradicts a recent manga chapter.

## 🧮 Calculating the Agreement Percentage

The core visual component of a Theory Post is the **Agreement Percentage**, displayed prominently on both the Feed and Search pages as a dynamic, color-coded Conic Gradient ring. 

To ensure this metric provides an accurate representation of the community's consensus, the calculation relies *only* on polarizing votes.

### The Mathematical Formula
```javascript
const activeVotesTotal = agreeCount + disagreeCount;
const agreementPercent = activeVotesTotal > 0 
    ? Math.round((agreeCount / activeVotesTotal) * 100) 
    : 0;
```

**Why exclude the "Interesting" vote?**
The "Interesting" vote acts as a neutral variable. If 100 people found a theory interesting, 10 people agreed with it, and 0 people disagreed, the theory technically has a 100% agreement rate among those who took a definitive stance on its accuracy. If we included the "Interesting" votes in the denominator, the agreement rate would incorrectly display as ~9%, falsely implying that the community rejected the hypothesis. By isolating the calculation to `Agree` and `Disagree`, we protect the integrity of the consensus metric.

## 🎨 UI/UX Implementation and Rendering

The frontend rendering of Theory stats is highly specialized. When a `post.type === 'theory'` is detected in the feed or search results, the standard engagement pill (Likes/Comments) is seamlessly swapped out for the Theory Dashboard UI.

### The Conic Gradient Ring
To provide immediate visual feedback, the agreement percentage is rendered using a modern CSS `conic-gradient`. 
```css
background: conic-gradient(#a855f7 ${agreePercent}%, #27272a 0);
```
This creates a beautiful, native-feeling circular progress bar that fills proportionally to the agreement rate. The ring utilizes a vibrant purple (`#a855f7`) to signify agreement, contrasting sharply against the dark mode background (`#27272a`).

### Hashtag Extraction
Theories often rely on extensive tagging (e.g., `#OnePiece`, `#Gear5`, `#Lore`). The UI engine automatically extracts these tags from the post body, strips out any internal system tags (e.g., tags starting with `##`), and limits the display to a maximum of three primary tags to maintain a clean, uncluttered interface on mobile devices.

## 🛡️ Moderation and Data Integrity

Because theories are highly debated, maintaining data integrity is paramount. 
- **Vote Toggling**: The backend strictly enforces vote toggling. A user cannot infinitely inflate the "Agree" count. Clicking "Agree" twice simply removes the vote.
- **Algorithmic Simulation**: To prevent new theories from suffering from the "cold start" problem, AnimeType employs a sophisticated algorithmic engagement system that supplements the raw user counts with simulated data based on real view metrics. (For a deep dive into this mechanism, refer to the [Algorithmic Engagement System](./engagement-system.md) documentation).

By combining specialized UX design, strict mathematical formulas, and community-tailored engagement options, the AnimeType Theory System provides the most robust platform available for anime lore discussion.\n