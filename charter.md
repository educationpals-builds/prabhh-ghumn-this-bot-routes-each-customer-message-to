# Trick-task board — Charter

## Who this serves

Teams shipping a bot that routes customer messages to queues. Before Friday's rebuild, you need proof the router handles edge cases — not just the easy tickets.

This board is for anyone who must answer: *Can this bot ship, or will it quietly mis-route messages until a human catches the damage?*

---

## The specimen under audit

> This bot routes each customer message to a queue. It already ran on real tickets. You prove whether it can ship before Friday's rebuild.

Sample messages the bot will face:

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

---

## What Caught / Slips / Hold mean

Each of the seven trick tasks gets one mark:

| Mark | Meaning |
|------|---------|
| **Caught** | The bot handled this trick correctly on the sample messages. |
| **Slips** | The bot failed this trick — it would mis-route or lose information. A defense must flip this row before ship. |
| **Hold** | Not yet tested, or evidence is inconclusive. Do not ship until resolved. |

---

## The seven board rows

1. **p1 — Bundle split**: Does the bot open separate tickets when one message contains two problems?
2. **p2 — Messy harmless**: Does the bot route correctly when the message is sloppy but straightforward?
3. **p3 — Mind reader**: Does the bot infer intent without explicit labels, risking wrong-queue routing?
4. **p4 — Small quotable**: Does the bot preserve the customer's exact words, or does it summarize away evidence?
5. **p5 — Hidden library**: Does the bot recognize references to prior tickets or threads?
6. **p6 — Goldfish**: Does the bot remember context from earlier in the conversation?
7. **p7 — Your trick task**: It verifies the customer from the call before opening a queue.

---

## Go-live commitment

**Gate sentence:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Block threshold:** Ship stops at **2** Slips rows. If two or more rows show Slips, the bot does not ship.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Defenses that flip Slips rows

When a row shows Slips, turn on the defense that catches it:

| Defense | What it catches |
|---------|-----------------|
| **Force a split when there are two jobs** | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| **Ban mind-reading verbs** | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| **Require a quoted source line** | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Charter commitment

A stranger can point this board at their own routing bot, paste their own customer messages, and get the same seven trick tasks applied to their case. The board returns Caught / Slips / Hold marks, names the defense for each Slips row, and enforces the go-live rule: no ship while Slips ≥ 2.
