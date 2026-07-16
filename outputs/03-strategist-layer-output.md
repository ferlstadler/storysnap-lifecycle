# Output 03 — Strategist Layer
_Generated from skill 03-strategist-layer.md_

---

## 1. Alert Taxonomy

| Alert Type | Trigger | Priority | Channel | Frequency |
|---|---|---|---|---|
| **At Risk** | Pace Score < 0.50 after month 3 OR 2+ active flags | Alert (48h) | HubSpot task + email to Strategist | On trigger; weekly reminder if no action logged |
| **Expiration Watch** | < 60 days to expiration AND > 20% credits remaining | URGENT (same day) | HubSpot task + email to Strategist | On trigger; escalation at day 3 if no activity |
| **Lost Champion** | Departure detected + replacement < 2 opens in 30 days | URGENT if < 90 days to expiration; Alert otherwise | HubSpot task + email to Strategist | On trigger; weekly task if no engagement after 21 days |
| **Drifting, No Response** | Account in Drifting journey for 6 weeks with no engagement on automated touches | Alert (weekly queue) | HubSpot task | Weekly batch; not individual alerts |
| **Cross-sell Opportunity** | Accelerating or Healthy account with `single_service` flag | Watch (weekly digest) | HubSpot task batch | Weekly digest; not individual alerts |
| **New Account, No Kickoff** | No deliverable started by day 21 post-purchase | Watch (48h) | HubSpot task | Day 21; single alert |

**Design note:** Alert volume is the enemy of action. URGENT alerts must stay rare — if everything is URGENT, Strategists stop reading them. Drifting No Response and Cross-sell are batched weekly precisely to avoid per-account noise. The three individual, time-sensitive alerts (At Risk, Expiration Watch, Lost Champion) are the only ones that fire in real time.

---

## 2. Fully Designed Alert: Lost Champion

**Why this one:** It's the alert where timing matters most and context matters most. An Expiration Watch alert is urgent but the action is clear. A Lost Champion alert requires the Strategist to make judgment calls — is the replacement the right person to contact? Is the account worth prioritizing given credits and timing? The alert needs to give them everything to answer those questions in 60 seconds.

---

**Alert format (HubSpot task + email notification)**

**Task title:**
`[URGENT] Champion Departed — {{account.name}} | {{credits.remaining}} credits | {{days.to_expiration}} days left`

**Task body:**

```
WHAT HAPPENED
{{contact.departed_name}} ({{contact.departed_title}}) left {{account.name}} on {{contact.departure_date}}.

ACCOUNT SNAPSHOT
Segment:              {{account.segment}}
Pace Score:           {{account.pace_score}} → {{pace.status}}
Credits remaining:    {{credits.remaining}} of {{credits.total}} ({{credits.pct_unused}}% unused)
Days to expiration:   {{days.to_expiration}}
Last deliverable:     {{deliverable.last_type}} completed on {{deliverable.last_date}}
Active flags:         {{account.flags}}
Last email open:      {{engagement.last_open_date}}
Last portal login:    {{portal.last_login_date}}

REPLACEMENT CONTACT
Name:   {{contact.replacement_name}}
Title:  {{contact.replacement_title}}
Email:  {{contact.replacement_email}}
Engagement to date: {{contact.replacement_opens}} opens since identified

SUGGESTED NEXT ACTION
Send personal introduction within 48h using the talking points below.
Do NOT re-enroll this account in automated email sequences until the new contact has engaged.
If expiration is < 90 days, flag for commercial conversation (extension vs. repurchase) on first call.

[View account in HubSpot] [Open talking points]
```

---

## 3. Talking Points: Lost Champion Outreach

**For:** Strategist reaching out to replacement contact for the first time
**Context:** New contact may or may not know about the StorySnap relationship. Do not assume they do.

**Opening — acknowledge the transition directly:**
"Hi [Name], I'm [Strategist] from [brand they purchased]. I wanted to reach out directly — I know transitions like this come with a lot on your plate, and I didn't want this account to fall through the cracks."

**Orient them, don't pitch them:**
- "Here's where things stand: [Company] has [X] credits available through [expiration date]. So far the team has produced [deliverable types completed] — I can send you links to everything that's been made."
- "There's still [X] credits to work with, and enough time to produce [realistic deliverable options given timeline]."
- Do not lead with urgency. Lead with "here's what exists and here's what's still possible."

**The ask — low commitment:**
"Would you be open to a 20-minute call? I can walk you through what's been done, show you what's available, and we can figure out together whether there's anything useful to produce before the term ends."

**If expiration is < 90 days — surface it matter-of-factly, not urgently:**
"Just worth knowing: the current bundle runs through [date]. Happy to talk through options — including whether it makes sense to roll anything forward."

**If `single_service` flag is active — introduce the broader portfolio:**
"One thing worth mentioning: [Company] has only used [brand/deliverable type] so far. The credits work across everything we make — including [other brand]. Happy to show you what else is available if it's relevant to what you're working on."

**What to avoid:**
- Do not reference the departed champion negatively or frame them as a problem
- Do not open with credit urgency — it sounds like a collections call
- Do not send a link to a calendar booking tool in the first message — it presumes too much
- Do not CC or BCC the departed contact
