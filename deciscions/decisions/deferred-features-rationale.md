# Deferred & Removed Features

The hardest operating skill on a small team is deciding what *not* to build. The next hardest is removing what's already been built when it stops earning its keep.

Each item below was either originally planned in Neuron's roadmap and didn't ship, or did ship and was removed. The pattern that emerged: features that pulled focus away from the actual value proposition (cart consolidation and shipping optimization) underperformed features that doubled down on it.

---

## Removed after building

### Gamification — points, levels, medals

**Originally:** Profile levels driven by total dollars spent. Carbon savings as a points multiplier. Medals for milestones. Unlockable features at higher tiers. This was central to the engagement story in the original roadmap — the layer that was supposed to give users a reason to keep coming back.

**What changed:** Built a version of it. Watched users. The gamification layer didn't create engagement; it competed with engagement. Users who got value from the actual product — saving money on shipping — didn't need points to come back. Users who didn't get value couldn't be rescued by them. The feature was creating maintenance burden and product complexity without producing the loop it had been pitched on.

**Replaced with:** The Savings Dashboard — a running ledger of real dollar savings the user has captured, broken down by source (bundling, price drops, card rewards). Real numbers beat synthetic points. Engagement is higher and the feature reinforces the actual value proposition rather than substituting for it.

### Elaborate CO2 tracking infrastructure

**Originally:** Per-transaction carbon savings, lifetime CO2 counter, social-feed sharing of impact, gamification tie-in. Sustainability was supposed to be a differentiator.

**What changed:** Calculating accurate CO2 savings per bundled order is genuinely hard. It depends on shipping origin, last-mile carrier, packaging weight, and return rate, most of which Neuron doesn't have reliable signal on. The numbers we could produce honestly were rough at best, and marketing rough numbers as precise is a credibility cost we couldn't afford on a brand still being built.

**Replaced with:** Nothing yet. If sustainability becomes a stronger signal in customer discovery, it can return as lightweight messaging — but not as a tracked metric infrastructure that promises a precision we can't deliver.

### Later / Favorites item categories

**Originally:** Multiple item states beyond Cart and Watching — Later, Favorites — for fine-grained organization.

**What changed:** Two states is already at the edge of what users hold in working memory while shopping. Adding more multiplied UI decisions (where does this item live? where do I find it later?) without adding clarity.

**Replaced with:** Just Cart and Watching, plus Shared Lists for multi-person use cases. Three concepts maximum.

---

## Cut before building

### Social feed

**Originally:** A newsfeed of users' purchases, savings, and earned medals. Users could share to friends, see what friends bought, buy directly from the feed. Pitched as the viral engagement loop.

**What changed:** Social features have a brutal cold-start problem. They require density to be useful, and density requires either a pre-existing network (Pinterest had moodboards, Instagram had photos) or paid acquisition that doesn't economically support a shopping product. The viral mechanic was theoretical. More fundamentally, shopping isn't something most people want to socialize with their full network. The narrow case where it makes sense — coordinating with specific people — is served better by a utility than a feed.

**Replaced with:** Shared Lists. A utility for households, gift coordination, and group purchases. No newsfeed, no follow graph, no public profiles.

### Universal vendor login management

**Originally:** Manage all retailer logins and rewards programs from inside Neuron. Convert vendor-specific rewards points into a unified Neuron currency usable across the platform.

**What changed:** This is a credential-management product. That's a different product. Building it would require security infrastructure (vault architecture, encryption at rest, breach response) that's expensive to do right, catastrophic to do wrong, and competes with tools users already trust like 1Password and Apple Keychain. The points-conversion idea additionally required brand partnerships at a scale Neuron can't yet command.

**Replaced with:** Nothing. Users keep their existing logins where they already are. Smart Card Matching addresses the rewards-optimization side of the original promise without ever holding a credential.

### Debit card integration / Neuron-branded card

**Originally:** Offer a Neuron debit card with cross-merchant rewards. Build into Klarna/Affirm-style fintech territory.

**What changed:** Becoming a fintech requires a sponsor bank relationship, KYC/AML compliance, fraud infrastructure, and reserves. Fully loaded, that's millions of dollars and years of regulatory work before the first transaction. The underlying use case — getting users optimal rewards on purchases — is solvable without any of that.

**Replaced with:** Smart Card Matching — manual card entry, per-store recommendations on which existing card to use at checkout, tracking of card benefits and credits already in the user's wallet. Same outcome for the user, none of the bank-charter regulatory drag.

---

## Deferred with explicit triggers

### Native iOS app

**Originally:** Phase 4 roadmap item, treated as a near-term platform expansion.

**What changed:** Native apps cost real money to build and maintain across two platforms, and the Chrome extension model meant most shopping was happening on desktop already. Building iOS before knowing whether mobile demand existed would have been expensive validation.

**Trigger to revisit:** 50K+ MAU on the web and extension product, with mobile demand validated by waitlist signups and direct user requests. Interim path is a PWA, which captures the mobile use case without committing to native development. iOS waitlist is live on the support page to measure demand directly rather than guess at it.

### Brand analytics / vendor dashboard

**Originally:** Sell brands a dashboard of Neuron user behavior — abandonment rates, shipping-threshold sensitivity, comparative pricing data. Tiered pricing for brand customers as a second revenue line.

**What changed:** Brand analytics products require scale to be actionable. Saying "100 of your shoppers used Neuron last month" isn't a real product to a major retailer. The Honey and Rakuten outcomes validated affiliate-volume monetization, not data-licensing monetization. Building this prematurely is selling something not yet worth buying.

**Trigger to revisit:** 500K–1M+ MAU. Below that, focus stays on affiliate revenue (CJ Affiliate, with ShareASale and Impact planned) and Pro subscription monetization. Brand product becomes possible at scale; it doesn't define the path to it.

---

## Operating principles that came out of this

A few patterns worth naming explicitly:

**Cut proxy metrics in favor of real ones.** Points and CO2 tallies are proxies. Dollar savings are real. When a feature has to translate value into a synthetic currency to be motivating, the underlying value probably isn't strong enough yet.

**Don't take custody of credentials or money that isn't core to the product.** Vendor login management and debit cards both required Neuron to hold things (passwords, deposits) whose mishandling would end the company. The thing actually being solved — rewards optimization — didn't require taking custody of either.

**Cold-start mechanics need pre-existing density to work.** Social feeds, peer-to-peer marketplaces, and any feature whose value depends on other users being there should be cut on a new product until the user base is dense enough to seed it organically. Otherwise you're building an empty room.

**Defer with a trigger, not a "someday."** Each deferred feature here has a specific signal that would move it into scope. Without that, "deferred" becomes a permanent backlog that haunts every roadmap meeting and slowly re-litigates itself every quarter.

**Three concepts is usually the ceiling for a consumer surface.** Cart, Watching, Shared Lists. When a fourth concept proposes itself, the right question is usually which existing one it should collapse into.

---

*Each cut above represents real engineering work that was either saved or recovered. That savings compounds — every quarter spent not maintaining a feature that wouldn't have earned its keep is a quarter of focus available for one that does.*
