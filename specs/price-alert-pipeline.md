# Price Alert Pipeline

**Status:** Shipped  
**Surface:** Backend (Supabase + edge functions) + Chrome extension + Email  
**Last updated:** May 27,2026

---

## Problem

Users watch items they're not ready to buy. Without a system to surface price drops, the watch list becomes a graveyard — items sit there, prices change, users don't know. We needed a reliable way to tell users when something they're watching has dropped in price, without becoming the kind of notification source they mute.

## User story

> As a shopper watching items across multiple stores, I want to be notified when a price drops so I can buy at the right moment, without manually checking each item every day.

## Success criteria

- Price checks run on a reliable cadence
- A user with an active alert receives notification within one cron cycle of the price drop being detected
- Notifications respect the channels the user opted into (email digest, browser push, or both)
- After an alert fires, it re-arms automatically — users don't manually reset anything
- Failures in the price-check job don't cascade into duplicate or missed deliveries
- Users can disable notifications globally or per-item without losing the watch itself

## Components

**1. Price-check cron**  
Runs on a schedule. Iterates over watched items, fetches current prices, compares against the baseline price stored at watch time and the user's target price if one was set. When a drop crosses the threshold, writes a row to the `price_alerts` table.

**2. Send-alerts edge function**  
Triggered off new rows in `price_alerts`. Groups alerts by user, formats a digest, sends email via Resend. For users opted into browser notifications, also surfaces the alert in a channel the extension polls via the Chrome alarms API.

**3. Notification preferences**  
Per-user config: email on/off, push on/off, digest cadence, quiet hours. Defaults are deliberately conservative — opt-in for email, opt-out for push. (Reasoning in the "What I learned" section.)

**4. Re-arm trigger**  
Database trigger that, after an alert is marked delivered, updates the watched item's baseline price so the next drop is measured from the new floor. Prevents the same alert firing repeatedly as the price oscillates around a threshold.

## Data model (high-level)

- `watched_items` — the watch list. Item reference, baseline price at watch time, optional target price, user.
- `notification_preferences` — per-user channel config and cadence.
- `price_alerts` — log of fired alerts. Used by the send function and as audit trail for debugging delivery issues.

## Edge cases handled

| Case | Behavior |
|---|---|
| Item out of stock during check | Skip, no alert fired, retry next cycle |
| Price increases instead of decreases | No-op |
| Target price set, but current is already below it at watch time | Don't fire on first check; treat current as the baseline |
| Email delivery fails | Row stays in `price_alerts` as undelivered, retried |
| User unsubscribes mid-cycle | Preferences checked at send time, not check time |
| Same item watched by multiple users | Price fetched once per item, fanned out per user |

## Out of scope (intentionally)

- **Predicting future drops** — would require pricing history we don't yet have at the volume needed
- **Cross-store "this item is cheaper here" alerts** — handled by a separate comparison feature
- **Back-in-stock alerts** — separate roadmap item, different signal entirely

## What I learned shipping this

**Cron-driven systems are harder than they look when you care about exactly-once delivery.** The re-arm trigger replaced an earlier approach where the same alert could fire multiple times during a price oscillation around the threshold. The fix is structural — the baseline has to update with the alert, in the same transaction — not a band-aid in the send function.

**Conservative notification defaults matter more than I expected.** The first version was opt-out on email. Unsubscribe rate was poor and reflected badly on domain reputation. Switching to opt-in raised the activation bar but the people who opt in actually engage with the digests. The lesson generalizes: in a product where you want users to trust your notifications, defaulting to "off" costs you reach but buys you signal quality, and signal quality is the harder thing to rebuild once lost.

**Resend made the email layer almost a non-event.** The interesting work was always the deduplication, the channel preferences, and the re-arm logic — not the email send itself. Worth knowing as a sequencing lesson: the third-party-shaped problems are usually the easy ones; the hard problems are state and consistency.

---

*This spec is reproduced from the actual working document used to scope the feature, with internal IDs, table names, and project-specific references sanitized. The structure — problem, user story, success criteria, components, data model, edge cases, out-of-scope, learnings — is what I use for every feature spec on Neuron.*
