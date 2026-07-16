# Output 05 — Analytics, Tracking & Week-One Plan
_Generated from skill 05-analytics.md_

---

## 1. North Star Metrics

**Metric 1: Credit Utilization Rate at Month 6**
What it measures: percentage of purchased credits used by the halfway point of the 12-month term, measured per cohort.

Why this one: the brief makes clear that best clients repurchase around month 6–7. That means month-6 utilization is the leading indicator for repurchase — everything else (GRR, NRR, renewal rate) is a lagging indicator that tells you what already happened. If utilization at month 6 is high, repurchase follows. If it's low, you already lost.

What good looks like in year one: establish a baseline in Q1, then move it by 15+ percentage points by end of year. If the current cohort average is ~40% at month 6 (assumption based on the underutilization pattern described), the target is 55%+ by Q4.

---

**Metric 2: Drifting-to-Healthy Recovery Rate (Automated)**
What it measures: percentage of accounts that enter the Drifting segment and return to Healthy (Pace Score ≥ 0.80) within 60 days — without requiring Strategist intervention.

Why this one: this is the metric that holds the automated journey accountable. If Strategists are rescuing every drifting account manually, the machine isn't working. If this rate is high, automation is changing behavior at scale. If it's low, the problem is either the trigger timing, the message content, or the channel mix.

What good looks like in year one: 40%+ of drifting accounts recover without Strategist involvement. Below 30% → investigate and iterate on the automated sequence. Above 50% → the system is working.

---

**Metric 3: Strategist Alert-to-Action Rate**
What it measures: percentage of Strategist alerts (At Risk, Expiration Watch, Lost Champion) that result in a logged HubSpot activity — call, email, or meeting — within 48 hours.

Why this one: alert design is only as good as the action it drives. If Strategists aren't acting on alerts within 48 hours, the alerts are either too frequent, too vague, or misprioritized. This metric holds the Strategist layer accountable as a product, not just the Strategists as individuals.

What good looks like in year one: 80%+ action rate on URGENT alerts, 65%+ on standard Alerts. If below those thresholds, audit the alert format and taxonomy before assuming it's a Strategist behavior problem.

---

## 2. Data and Tracking Gaps

**Gap 1: Airtable ↔ HubSpot are not synced**
Pace data lives in Airtable. Engagement and contact data live in HubSpot. No single system has both. Without a sync, no HubSpot workflow can enroll accounts based on pace, and no Strategist alert can include credit data automatically.

Action: Week one (see below). Map the field list that needs to flow from Airtable to HubSpot. Start with a Zapier automation as the first layer — it's fast to configure and good enough to validate the logic. Scope a proper native sync or webhook with product ops in parallel.

**Gap 2: Pace Score does not exist as a field anywhere**
The formula can be computed from Airtable data, but it is not currently a stored, queryable value. Without it, no segment enrollment criterion can be automated.

Action: Define the formula (credits_used / credits_total) / (days_elapsed / 365). Create it as a formula field in Airtable first, then sync the computed value to a HubSpot account property. This is the critical path item (see week one).

**Gap 3: Portal login data is not in HubSpot**
The `dark` flag requires portal engagement data. If login events are not firing into HubSpot today, the flag cannot be computed.

Action: Confirm with product ops whether portal logins are tracked anywhere in a structured way (analytics platform, database, or existing event system). If yes, pipe them into HubSpot as a custom property (last_portal_login date). If not, add a basic server-side event that captures user ID + timestamp on login.

**Gap 4: Champion departure is an alert, not a structured field**
The departure detection exists, but if it arrives as an email notification rather than a CRM field update, it cannot trigger automated workflows or append to account records.

Action: Confirm the format of the departure alert. If it's email-only, create a Strategist-side manual process: when they receive a departure alert, they update a HubSpot contact property (departed = true, departure_date). Then build the workflow on top of that field. Not elegant, but it works immediately with no dev work.

**Gap 5: Cross-brand credit usage is not rolled up at the account level**
To detect `single_service`, I need a count of distinct deliverable types used per account. Airtable likely has this per-deliverable record, but not as a rollup.

Action: Create a rollup field in Airtable that counts distinct deliverable_type values per account. Sync that count (and the list of types) to HubSpot as an account property. This enables `single_service` detection and cross-sell targeting.

---

## 3. Week-One Build: Pace Score Sync

The single thing to build in week one is a computed Pace Score field that exists in HubSpot as a queryable account property, populated by Airtable data.

**Why this first:** Pace Score is the foundation of every segment, every journey enrollment, every alert, and two of the three north star metrics. Nothing in the system can run without it. No other build is on the critical path in the same way — portal login sync and champion departure detection matter, but they power individual flags, not the entire engine.

**What it unlocks immediately:** the moment Pace Score exists as a HubSpot property, every segment enrollment criterion is implementable as a HubSpot workflow filter. The Drifting journey can enroll accounts. The At Risk alert can fire. The priority matrix becomes operational. The rest of the system can be built in sequence without any additional data work.

**What it does not require:** new portal development, changes to the champion departure detection system, or cross-brand rollup logic. Those come after week one.

**How to build it in week one:** create the formula field in Airtable (credits_used / credits_total / (days_elapsed / 365)), validate it against 5–10 known accounts manually, then set up a Zapier automation that pushes the computed value to a HubSpot account property every 24 hours. Imperfect — but working and auditable on day seven.

---

## 4. Conscious Scope Cuts

**Cut 1: New account onboarding journey (step-by-step content)**
The onboarding journey is defined at the segment level, but the actual content — what to say in week 1, how to introduce the portal, how to brief a client on all brands — deserves its own exercise. I deprioritized it because the core problem the brief identifies is underutilization mid-term, not early abandonment. Fixing drift and at-risk accounts recovers more revenue faster than optimizing the first 30 days.

**Cut 2: Accelerating account cross-sell journey**
Accounts with Pace Score > 1.2 are a good problem. They're self-correcting. I defined the segment and noted the cross-sell opportunity, but didn't design the journey in detail because the ROI of that work is lower than fixing the accounts that are at risk of not repurchasing at all. When the core system is running, Accelerating accounts are the right next investment.
