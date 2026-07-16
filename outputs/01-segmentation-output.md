# Output 01 — Segmentation Model
_Generated from skill 01-segmentation.md_

---

## 1. Pace Score

**Formula:**
```
Pace Score = (Credits Used / Credits Purchased) ÷ (Days Elapsed / 365)
```

A score of **1.0** means the account is consuming credits exactly proportional to time elapsed. Above 1.0 = ahead of pace. Below 1.0 = behind.

**Example:** Month 6 of 12 (50% term elapsed). Client used 35% of credits.
`Pace Score = 0.35 / 0.50 = 0.70` → Drifting.

**Computed from Airtable:** credits_purchased, credits_used, purchase_date. No external data needed.

**Design note:** I deliberately avoided a more complex formula (e.g., weighted by deliverable type or recent velocity) because the data to support it isn't reliably structured yet. The simple ratio is computable today and corrects itself as usage patterns emerge. Velocity trending (is pace improving or worsening week-over-week) is the next layer to add once the score is live.

---

## 2. Segment Definitions

| Segment | Qualifying Signals | Primary Action | Owner |
|---|---|---|---|
| **New** | 0–30 days post-purchase | Onboarding sequence — set expectations, introduce portal, introduce all brands | Automated |
| **Accelerating** | Pace Score > 1.2 | Cross-sell awareness + early renewal prep | Automated |
| **Healthy** | Pace Score 0.80–1.20, month 2+ | Light nurture, cross-brand education, social proof | Automated |
| **Drifting** | Pace Score 0.50–0.79 | Re-engagement nudges; Strategist is notified but does not act yet | Automated + Monitor |
| **At Risk** | Pace Score < 0.50 after month 3, OR any Pace Score with 2+ active flags | Automated escalation sequence + Strategist alert triggered | Strategist-led |
| **Expiration Watch** | < 60 days to term end AND > 20% credits remaining (any segment) | Immediate Strategist escalation; automated urgency message | Urgent / Strategist |

**Design note:** Six segments, mutually exclusive at the account level. Expiration Watch functions as an override — it surfaces any account regardless of current segment when the time window becomes critical. I considered splitting Drifting into Early/Late Drift, but that added a seventh segment without changing the action meaningfully. The line between Drifting and At Risk (0.50) is the threshold where data shows drift starts to compound — accounts below this rarely self-correct without intervention.

---

## 3. Flag System

Flags are account-level indicators of specific failure modes. They stack freely on top of any segment. An account can hold multiple flags simultaneously.

| Flag | Detection Logic | Data Source |
|---|---|---|
| `behind_pace` | Pace Score < 0.80 for 2 consecutive weekly checks | Airtable |
| `single_service` | Only 1 unique deliverable type across all completed deliverables; no cross-brand history | Airtable |
| `testimonial_blocked` | Testimonial-type deliverable included in purchase + no testimonial completed in 60+ days + Pace Score < 0.80 | Airtable |
| `lost_champion` | Primary contact departure detected AND replacement contact has < 2 email opens in last 30 days | HubSpot + departure alert |
| `dark` | No portal login in 30 days AND average open rate on last 4 sends < 20% | Portal logs + HubSpot |

**Design note:** Flags are not segments — they modify the urgency and action of whichever segment an account is in. `testimonial_blocked` is narrow by design: it only fires when there is evidence the client has testimonial credits and isn't using them, not just any slow account. `dark` requires two signals (portal + email) to avoid false positives from clients who simply don't open marketing email but are still active in the portal.

---

## 4. Priority Matrix

How segment + flag count determines escalation level:

| | 0 flags | 1 flag | 2+ flags |
|---|---|---|---|
| **New** | Automated | Automated + monitor | Strategist watch |
| **Accelerating** | Automated | Automated | Monitor |
| **Healthy** | Automated | Monitor | Strategist watch |
| **Drifting** | Automated | Strategist watch | Strategist alert |
| **At Risk** | Strategist alert | Strategist alert | **URGENT** |
| **Expiration Watch** | **URGENT** | **URGENT** | **URGENT** |

**Escalation definitions:**
- **Automated** — machine handles entirely, no Strategist action needed
- **Monitor** — Strategist sees account on a watchlist; no action required yet
- **Strategist watch** — Strategist receives a low-priority task to check in within the week
- **Strategist alert** — Strategist receives a priority task with full account context; action expected within 48h
- **URGENT** — Strategist receives a same-day alert; account escalated to top of their queue

**Design note:** Accelerating + flags is intentionally low urgency — a fast-spending account with a lost champion is concerning, but the primary problem is the departure, not the spend rate. At Risk overrides flag count and always becomes a Strategist alert because the compounding effect means waiting for a second flag is too late.
