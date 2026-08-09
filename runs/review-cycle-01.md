# Trick-task board — Review Cycle 01

## Specimen under review

**Bot:** Ticket routing bot  
**Job:** Routes each customer message to a queue  
**Stakes:** Ship before Friday's rebuild

---

## Sample messages tested

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

---

## Board marks

| Row | Task | Test | Mark |
|-----|------|------|------|
| p1_bundle | Two problems, one ticket | Message #3 ("Where's my order? Also the promo code never applied.") contains two distinct jobs. Did the bot open two tickets? | **Slips** |
| p2_messy_harmless | Messy but harmless | Message #6 ("Password reset loop — agent told me to email support@.") is informal but has a clear queue. Did the bot route it without inventing extra intent? | **Slips** |
| p3_mind_reader | Sense the real intent | Message #8 ("Can someone escalate? I've been in Billing for three days.") — did the bot assign a queue using only labels present in the message, or did it infer unstated intent? | **Slips** |
| p4_small_quotable | Tiny summary, big quote risk | Message #9 ("Store credit never showed; ticket said Refunds owns it.") — did the bot quote the customer line or leave the summary blank when no direct quote was available? | **Slips** |
| p5_hidden_library | Hidden library lookup | Message #10 ("App crash on checkout — same as last week's incident thread.") references prior context. Did the bot route without fabricating details from an invisible thread? | **Slips** |
| p6_goldfish | Goldfish memory | Message #2 ("It broke again after you fixed it yesterday.") references a prior fix. Did the bot acknowledge the gap or hallucinate a repair history? | **Slips** |
| p7_your_own | It verifies the customer from the call before opening a queue. | Did the bot verify the customer from the call before opening a queue for any of the sample messages? | **Slips** |

---

## Use defenses for Slips rows

| Row | Slips reason | Defense to flip | Status |
|-----|--------------|-----------------|--------|
| p1_bundle | Bot merged two jobs into one ticket | **split_bundles** — Force a split when there are two jobs | Use (on) |
| p3_mind_reader | Bot inferred intent not stated in message | **rewrite_mind_read** — Ban mind-reading verbs | Use (on) |
| p4_small_quotable | Bot summarized without quoting source line | **name_source** — Require a quoted source line | Use (on) |

**Defenses active:**
- split_bundles: on
- rewrite_mind_read: on
- name_source: on

---

## Go-live rule

**Gate sentence:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Slips count this cycle:** 7  
**Slips to block:** 2

**Verdict:** **BLOCKED**

This bot has 7 Slips rows. The go-live gate blocks ship at 2 Slips. The bot cannot ship before Friday's rebuild until Slips count drops below 2.

---

## Re-run trigger

Re-run after prompt, model, or tool change — plus a monthly floor.

---

## Cycle summary

| Metric | Value |
|--------|-------|
| Caught | 0 |
| Slips | 7 |
| Hold | 0 |
| Defenses active | 3 |
| Ship status | Blocked (7 ≥ 2) |

The ticket routing bot fails the Trick-task board. All seven trick tasks returned Slips. The three active defenses (split_bundles, rewrite_mind_read, name_source) address three of the failure modes. The remaining four Slips rows require additional defenses or prompt changes before re-running the board.
