# Trick-task board — Run Local

How to replay this board on your own bot and messages.

---

## What you need

1. **Your bot description** — what it does, who gets hurt when it quietly gets things wrong.
2. **Sample messages** — real inputs the bot will face.

---

## The seven trick tasks

This board runs exactly seven probes against your bot:

| Row | Task |
|-----|------|
| p1 | Bundle — two problems in one message |
| p2 | Messy harmless — noise that shouldn't trigger action |
| p3 | Mind reader — inferring intent without evidence |
| p4 | Small quotable — tiny summary that loses the original |
| p5 | Hidden library — unstated knowledge the bot assumes |
| p6 | Goldfish — forgetting earlier context |
| p7 | It verifies the customer from the call before opening a queue. |

---

## How to run

### Step 1 — Paste your bot

Describe the bot you're about to trust. Example from this build:

> This bot routes each customer message to a queue. It already ran on real tickets. You prove whether it can ship before Friday's rebuild.

### Step 2 — Paste your messages

Provide real messages the bot will handle. Example from this build:

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

### Step 3 — Read seven marks

For each of the seven tasks, mark:

- **Caught** — the bot handles this correctly
- **Slips** — the bot fails this task
- **Hold** — you need more evidence before deciding

### Step 4 — Apply the go-live rule

After marking all seven rows, apply the ship gate:

- **Hold style:** Ship stops when Slips hit your count. No soft warnings, no owners.
- **Block threshold:** 2 slips
- **Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

If your board shows 2 or more Slips rows, ship stops. No exceptions.

---

## Defenses that flip Slips

When a row marks Slips, check whether one of these defenses would flip it to Caught:

| Defense | What it catches |
|---------|-----------------|
| Force a split when there are two jobs | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Replay checklist

- [ ] Bot description pasted
- [ ] Sample messages pasted
- [ ] All seven rows marked (Caught / Slips / Hold)
- [ ] Slips count tallied
- [ ] Go-live rule applied (block at 2 slips)
- [ ] Re-run scheduled per trigger
