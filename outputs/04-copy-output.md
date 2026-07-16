# Output 04 — Automated Message Copy
_Generated from skill 04-copy.md_

---

## Message 1: Automated Email

**Trigger:** Account is in Drifting journey (week 4), `testimonial_blocked` flag active — no testimonial completed in 60+ days
**Segment:** Drifting + `testimonial_blocked`
**Channel:** Automated email via HubSpot
**Dynamic variables:**
- `{{contact.first_name}}` — from HubSpot contact
- `{{account.name}}` — from HubSpot account
- `{{account.testimonial_credits}}` — from Airtable (testimonial-type credits remaining)
- `{{strategist.name}}` — from HubSpot Strategist owner field

---

**Subject:** Getting customers on camera (what actually works)

**Body:**

Hi {{contact.first_name}},

Getting a customer to agree to a video testimonial is usually the hardest part — not the shoot, not the edit. The ask is where most teams stall.

What moves it: make the ask dead simple. A one-line message like "Would you be up for a 5-minute call about your experience with us?" converts better than a formal request with a lot of context.

We put together a short outreach guide for teams in {{account.name}}'s space — it's what we send clients when things get stuck. Happy to drop it in your inbox.

Want me to send it over?

{{strategist.name}}

P.S. You still have {{account.testimonial_credits}} testimonial credits available. Remote shoots can be scheduled within a week once your customer says yes.

---

**Copy notes:**
The email is sent from the Strategist's name, not a generic sender — it's a warm nudge, not a newsletter. The guide offer gives the contact a reason to reply without committing to anything. The P.S. surfaces the credit availability without making it feel like a pressure tactic. Single CTA: reply yes to get the guide.

---

## Message 2: In-App / Portal Message

**Trigger:** Account logs into portal, is in Healthy or Drifting segment, `single_service` flag active
**Segment:** Healthy or Drifting + `single_service`
**Channel:** In-app banner, shown on portal login (dismiss after 1 view, do not repeat for 14 days)
**Dynamic variables:**
- `{{account.deliverable_type_used}}` — from Airtable (most-used deliverable type)
- `{{account.brand_not_used}}` — from Airtable (brand with zero deliverables completed)
- `{{account.credits_remaining}}` — from Airtable

---

**Message:**

Your {{account.credits_remaining}} credits work across everything we make — not just {{account.deliverable_type_used}}.

Teams like yours are using {{account.brand_not_used}} to make their product look as good as it performs. Same credits, different story.

[See what's possible →]

---

**Copy notes:**
Under 40 words. The key phrase is "same credits, different story" — it removes the friction of "do I need to buy something new?" which is the main cross-sell objection in a B2B procurement context. The CTA links to the brand page for `{{account.brand_not_used}}` so it's contextual, not generic. Banner is dismissed after 1 view to avoid becoming wallpaper.
