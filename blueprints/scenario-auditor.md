# Trick-task board — Blueprint

## Purpose

This blueprint defines how to run the seven-row board on any bot a stranger brings. The board tests whether a bot can ship by walking seven trick tasks, marking each Caught / Slips / Hold, naming the defense that would flip each Slips row, and returning a go-live rule.

---

## Intake paste shape

A stranger pastes:

1. **What the bot does** — one sentence describing the routing or classification job
2. **Who gets hurt when it quietly gets things wrong** — the stake
3. **Sample messages** — real inputs the bot will face (minimum 5)

---

## The seven trick tasks

| Row | Task ID | Trick task |
|-----|---------|------------|
| p1 | p1_bundle | Two problems, one ticket — does the bot split or merge? |
| p2 | p2_messy_harmless | Messy grammar, harmless intent — does the bot panic or route calmly? |
| p3 | p3_mind_reader | Sense the real intent — does the bot invent meaning or wait for labels? |
| p4 | p4_small_quotable | Tiny summary, big quote risk — does the bot quote the source or paraphrase? |
| p5 | p5_hidden_library | Hidden context from prior thread — does the bot hallucinate history? |
| p6 | p6_goldfish | Same issue, new day — does the bot remember or start fresh? |
| p7 | p7_your_own | It verifies the customer from the call before opening a queue. |

---

## Verdict chips

For each row, mark exactly one:

- **Caught** — the bot handles the trick correctly
- **Slips** — the bot fails the trick in a way that could hurt
- **Hold** — not enough evidence to decide; needs more samples

---

## Use defenses

When a row marks **Slips**, name the defense that would flip it to Caught:

| Defense ID | Defense rule | What it catches |
|------------|--------------|-----------------|
| split_bundles | Force a split when there are two jobs | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| rewrite_mind_read | Ban mind-reading verbs | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |
| name_source | Require a quoted source line | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

If a defense is still off (Skip/unset), note it but do not invent a rewrite module.

---

## Go-live gate

**Hard stop rule:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Slips-to-block threshold:** 2

If the board shows 2 or more Slips rows, the bot does not ship.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Worked example — ticket routing bot

**Bot:** This bot routes each customer message to a queue. It already ran on real tickets. You prove whether it can ship before Friday's rebuild.

**Sample messages:**

1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.
6. Password reset loop — agent told me to email support@.
7. Damaged box on delivery; I need a replacement and a pickup.
8. Can someone escalate? I've been in Billing for three days.
9. Store credit never showed; ticket said Refunds owns it.
10. App crash on checkout — same as last week's incident thread.

### Board run

| Row | Task | Sample tested | Verdict | Use defense |
|-----|------|---------------|---------|-------------|
| p1 | p1_bundle | #3: "Where's my order? Also the promo code never applied." | Slips | split_bundles |
| p2 | p2_messy_harmless | #6: "Password reset loop — agent told me to email support@." | Slips | — |
| p3 | p3_mind_reader | #4: "Cancel the subscription but keep the open return." | Slips | rewrite_mind_read |
| p4 | p4_small_quotable | #9: "Store credit never showed; ticket said Refunds owns it." | Slips | name_source |
| p5 | p5_hidden_library | #10: "App crash on checkout — same as last week's incident thread." | Slips | — |
| p6 | p6_goldfish | #2: "It broke again after you fixed it yesterday." | Slips | — |
| p7 | p7_your_own | #5: "Billing charged twice; chat said shipping had the tracking." | Slips | — |

### Gate result

**Slips count:** 7  
**Threshold:** 2  
**Verdict:** BLOCK — do not ship. Slips exceed the threshold.

---

## Output shape

The board returns:

```
Board marks:
  p1_bundle: [Caught | Slips | Hold]
  p2_messy_harmless: [Caught | Slips | Hold]
  p3_mind_reader: [Caught | Slips | Hold]
  p4_small_quotable: [Caught | Slips | Hold]
  p5_hidden_library: [Caught | Slips | Hold]
  p6_goldfish: [Caught | Slips | Hold]
  p7_your_own: [Caught | Slips | Hold]

Defenses applied:
  split_bundles: [on | off]
  rewrite_mind_read: [on | off]
  name_source: [on | off]

Go-live rule:
  Slips count: [n]
  Threshold: 2
  Verdict: [SHIP | BLOCK]
  Re-run: Re-run after prompt, model, or tool change — plus a monthly floor.
```
