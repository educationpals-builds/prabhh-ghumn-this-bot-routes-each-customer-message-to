# Trick-task board

Seven trick tasks for the ticket-routing bot. Each row shows the test message, what the bot did, the verdict for this run, and the defense that would flip a Slips mark.

---

## Board

| # | Task | Test message | Bot behavior | Verdict | Defense to flip Slips |
|---|------|--------------|--------------|---------|----------------------|
| p1 | Bundle trap | "Where's my order? Also the promo code never applied." | Routed to one queue instead of splitting two jobs | **Slips** | Force a split when there are two jobs |
| p2 | Messy harmless | "It broke again after you fixed it yesterday." | Routed without flagging the prior-ticket reference | **Slips** | — |
| p3 | Mind reader | "Can someone escalate? I've been in Billing for three days." | Inferred intent without explicit queue labels | **Slips** | Ban mind-reading verbs |
| p4 | Small quotable | "Store credit never showed; ticket said Refunds owns it." | Summarized without quoting the customer line | **Slips** | Require a quoted source line |
| p5 | Hidden library | "App crash on checkout — same as last week's incident thread." | Routed without referencing prior incident data | **Slips** | — |
| p6 | Goldfish | "Billing charged twice; chat said shipping had the tracking." | Lost context of the cross-queue handoff | **Slips** | — |
| p7 | Your trick task | "Password reset loop — agent told me to email support@." | It verifies the customer from the call before opening a queue. | **Slips** | — |

---

## Specimen messages used

```
Refund for wrong size — not a shipping question.
It broke again after you fixed it yesterday.
Where's my order? Also the promo code never applied.
Cancel the subscription but keep the open return.
Billing charged twice; chat said shipping had the tracking.
Password reset loop — agent told me to email support@.
Damaged box on delivery; I need a replacement and a pickup.
Can someone escalate? I've been in Billing for three days.
Store credit never showed; ticket said Refunds owns it.
App crash on checkout — same as last week's incident thread.
```

---

## Defenses marked Use

| Defense | Explanation |
|---------|-------------|
| Force a split when there are two jobs | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Summary

- **Total tasks:** 7
- **Caught:** 0
- **Slips:** 7
- **Hold:** 0

All seven tasks slipped on this run. The defenses above would flip p1, p3, and p4 if enforced before the next run.
