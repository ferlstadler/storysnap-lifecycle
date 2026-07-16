# Output 02 — Journey System
_Generated from skill 02-journey-system.md_

---

## Journey 1: New Account Onboarding (Segment: New, days 0–30)

**Entry trigger:** SOW signed / purchase confirmed in Airtable (day 0)

| Step | Timing | Channel | Intent | Branch |
|---|---|---|---|---|
| 1 | Day 1 | Email | Welcome + portal access. Introduce all brands, not just what they bought. Set expectation: a Strategist will be in touch within the week | — |
| 2 | Day 7 | In-app | "Ready to kick off your first project?" Prompt to schedule first creative brief | If project briefing scheduled → monitor |
| 3 | Day 14 | Email | "Here's what your credits can do" — brand-agnostic menu of deliverable types with examples from their industry (SaaS/cybersecurity) | If no deliverable started → continue |
| 4 | Day 21 | Strategist task | "New account, no project kicked off in 21 days. Recommend outreach." Includes: account name, contacts, credits, flags | Strategist acts → exit |
| 5 | Day 30 | System | Re-evaluate into segment. If deliverable started → Healthy. If not → At Risk | — |

**Machine / Human split:** Machine owns days 1–14. Strategist is flagged at day 21 — early enough to act before the account is already drifting.

**Exit conditions:** First deliverable kicked off OR day 30 re-segmentation

---

## Journey 2: Drifting Re-engagement (Segment: Drifting, Pace Score 0.50–0.79)

**Entry trigger:** Pace Score drops below 0.80 for 2 consecutive weekly Airtable syncs

| Step | Timing | Channel | Intent | Branch |
|---|---|---|---|---|
| 1 | Week 0 | Email | Specific nudge with account data: "You've used {{credits_used}} of {{credits_total}}. Here's a deliverable type you haven't tried yet — here's what it looks like for companies like yours." | If pace improves → exit |
| 2 | Week 2 | In-app | On next portal login: "Pick up where you left off" + direct link to start a new project brief | If project started → exit |
| 3 | Week 4 | Email | Social proof: "[Similar SaaS company] used their remaining credits on [deliverable type]. Here's what they made." CTA: schedule a 20-min review call | If call booked → exit |
| 4 | Week 6 | Strategist task | "Account drifting 6 weeks, no response to 3 automated touchpoints. Recommend personal outreach." Includes: pace score, credits remaining, days to expiration, active flags, last engagement date | Strategist acts → exit |
| 4b | Week 6 | Strategist task modifier | If `testimonial_blocked` flag active → append testimonial blocker talking points to the task | — |

**Machine / Human split:** Weeks 0–4 fully automated. Strategist enters at week 6. The line is drawn there because after 3 touches with no response, another automated email is noise. A human call is the only thing that changes behavior at this stage in a B2B relationship.

**Exit conditions:** Pace Score returns above 0.80 → re-enroll in Healthy journey. Strategist logs outreach in HubSpot → exit automated sequence.

---

## Journey 3: At Risk Escalation (Segment: At Risk)

**Entry trigger:** Pace Score < 0.50 after month 3 OR any account with 2+ active flags

| Step | Timing | Channel | Intent | Branch |
|---|---|---|---|---|
| 1 | Day 0 (immediate) | Strategist alert | Full account snapshot: pace score, credits remaining, days to expiration, active flags, last deliverable, last email open, last portal login. Priority: 48h action | Strategist acts → monitor |
| 2 | Day 0 | Email | Warm check-in to all contacts: "Quick note on your credits — we want to make sure you get the most out of your bundle." Non-alarming. CTA: schedule a review call | If engaged → Strategist follows up |
| 3 | Day 3 | In-app | On next portal login: "Your account team has ideas for your remaining credits." CTA: book a call | If no engagement → continue |
| 4 | Day 7 | Strategist task | Follow-up reminder: "No client response in 7 days. Escalate?" | Strategist escalates or marks as contacted |
| 5 | Day 14 | System | If no movement → override into Expiration Watch protocol regardless of days remaining | — |

**Machine / Human split:** Alert fires immediately — Strategist is in the loop from day 0. Machine sends one client-facing email on day 0 to keep the account warm while the Strategist prepares their outreach. Machine does not send additional automated messages without Strategist clearance.

**Exit conditions:** Strategist marks account as engaged in HubSpot OR Pace Score improves above 0.50 OR account enters Expiration Watch

---

## Journey 4: Expiration Watch (< 60 days to expiration, > 20% credits remaining)

**Entry trigger:** System detects < 60 days to term end AND > 20% credits remaining (any segment)

| Step | Timing | Channel | Intent | Branch |
|---|---|---|---|---|
| 1 | Day 0 (immediate) | Strategist alert URGENT | "X credits expiring in Y days. Same-day action required." Full account snapshot + repurchase history | Strategist calls client → exit auto |
| 2 | Day 0 | Email | All contacts: "Your credits are available through [expiration date]." Lists specific deliverable options not yet used, with production time estimates — what's realistically achievable in the time left | If response → Strategist follows up |
| 3 | Day 3 | Strategist task | If no HubSpot call or meeting logged: "No activity recorded in 3 days. Escalate to manager?" | — |
| 4 | Day 14 (if < 46 days left) | Email | "Here's what you can still produce before [date]" — shorter list based on production timelines | — |
| 5 | Day 30 (if < 30 days left) | In-app | On portal login: "You have [X] credits and [N] days. Here's what's still possible." Direct CTA | — |

**Machine / Human split:** Strategist owns the commercial conversation (extension vs. rollover vs. repurchase). Machine handles client-facing communication to create helpful urgency without pressure tactics. Strategist never gets blindsided because the alert fires 60 days out, not 7.

**Exit conditions:** Credits drop below 20% remaining OR term expires OR account repurchases

---

## Journey 5: Lost Champion Recovery (Flag: `lost_champion`)

**Entry trigger:** Champion departure alert fires AND replacement contact has < 2 email opens in last 30 days

This journey overlays the account's primary segment journey. It does not replace it — it pauses automated client-facing messages and routes everything through the Strategist.

| Step | Timing | Channel | Intent | Branch |
|---|---|---|---|---|
| 1 | Day 0 (immediate) | Strategist alert URGENT | "[Name] left [Company]. Replacement: [Name/role if known]. Account is [segment] with [X] credits and [Y] days to expiration. Do NOT re-enroll in automated sequences." | — |
| 2 | Day 0 | System | Pause all automated emails and in-app messages for this account | — |
| 3 | Day 3 | Strategist sends manual intro | Uses talking points from Skill 03. Framed as orientation, not sales | If response → continue |
| 4 | Day 14 | Strategist task reminder | If no HubSpot activity logged: "No outreach recorded. Account paused. Action?" | — |
| 5 | Day 21 | System | If replacement contact has 2+ email opens OR portal login → re-enroll in segment journey. If not → flag remains, weekly Strategist task created | — |

**Machine / Human split:** Machine does nothing client-facing here. Every touchpoint with a new champion must be human. The cost of an automated message landing with someone who has no context for the account is too high to risk in a $15–20K relationship.

**Exit conditions:** New contact engages (2+ opens or portal login) → re-enroll in primary segment journey. Flag cleared by Strategist manually if situation resolves differently.
