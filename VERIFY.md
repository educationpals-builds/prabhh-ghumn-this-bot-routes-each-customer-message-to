# Trick-task board — Verification Checklist

Use this checklist to confirm the board works correctly when a stranger runs `/play`.

---

## 1. Exactly seven Caught / Slips / Hold marks

The kit must return exactly **7 rows**, each marked Caught, Slips, or Hold:

| Row | Trick task |
|-----|------------|
| p1 | Bundle trap (two jobs, one ticket) |
| p2 | Messy-harmless trap |
| p3 | Mind-reader trap |
| p4 | Small-quotable trap |
| p5 | Hidden-library trap |
| p6 | Goldfish trap |
| p7 | It verifies the customer from the call before opening a queue. |

If the kit returns fewer than 7 or more than 7 rows, verification fails.

---

## 2. Every Slips row names a Use defense

When a row is marked **Slips**, the board must name one of these Use defenses:

- **Force a split when there are two jobs** — Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.
- **Ban mind-reading verbs** — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.
- **Require a quoted source line** — Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

If a Slips row does not name a Use defense, verification fails.

---

## 3. Hostile ask p7 quotes the learner's trick verbatim

Row p7 must quote this exact ask:

> It verifies the customer from the call before opening a queue.

If p7 shows a different ask (e.g., "churn sensing" or "sentiment detection"), verification fails.

---

## 4. Go-live rule quotes slips_to_block verbatim

The go-live rule must state the hold number as **2**.

> Ship stops when Slips hit your count. No soft warnings, no owners.

If the board shows a different threshold (e.g., 1 or 3), verification fails.

---

## 5. Refuses green ship while Slips ≥ 2

When the board counts **2 or more Slips rows**, it must refuse to show a green ship status.

Test: If all seven rows show Slips (as in the worked example), the board must block ship. A green ship while Slips ≥ 2 is a verification failure.

---

## 6. Domain matches the selected situation only

All examples must stay inside this domain:

> This bot routes each customer message to a queue. It already ran on real tickets. You prove whether it can ship before Friday's rebuild.

Sample messages from the worked example:

- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.
- Password reset loop — agent told me to email support@.
- Damaged box on delivery; I need a replacement and a pickup.
- Can someone escalate? I've been in Billing for three days.
- Store credit never showed; ticket said Refunds owns it.
- App crash on checkout — same as last week's incident thread.

If the board shows examples from a different domain (clause splitting, FAQ answering, refund classification, inbox triage), verification fails.

---

## Re-run trigger

The board must re-run under this condition:

> Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Summary

| Check | Pass condition |
|-------|----------------|
| Row count | Exactly 7 |
| Slips defense | Every Slips row names a Use defense |
| p7 ask | Quotes "It verifies the customer from the call before opening a queue." |
| Hold number | 2 |
| Ship gate | Refuses green when Slips ≥ 2 |
| Domain | Customer-message routing bot only |
