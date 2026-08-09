# Trick-task board — Scenario Analyzer

How the analyzer reads a stranger's paste into the seven board rows and defenses.

---

## Input shape

A stranger pastes:

1. **Bot description** — what the bot does, who gets hurt when it quietly gets things wrong
2. **Sample messages** — real messages the bot will face

The analyzer parses this paste and runs each of the seven trick tasks against the sample messages.

---

## Board rows

The analyzer populates exactly seven rows:

| Row ID | Trick task | What the analyzer looks for |
|--------|------------|----------------------------|
| p1_bundle | Two problems, one ticket | Messages containing multiple distinct jobs (e.g., "Where's my order? Also the promo code never applied.") |
| p2_messy_harmless | Messy but harmless | Messages with noise that doesn't change the routing decision |
| p3_mind_reader | Sense the real intent | Messages where the bot infers intent without explicit labels or queue IDs |
| p4_small_quotable | Tiny summary, big quote risk | Messages where a one-liner summary loses critical customer language |
| p5_hidden_library | Hidden library dependency | Messages requiring lookup or context the bot may not have |
| p6_goldfish | Forgets prior context | Messages referencing earlier interactions the bot should remember |
| p7_your_own | It verifies the customer from the call before opening a queue. | Messages where identity verification is required before queue assignment |

---

## Parsing logic

For each stranger message, the analyzer:

1. **Scans for bundle signals** — conjunctions ("also," "and," "but"), multiple complaint types, or separate action requests
2. **Checks for mind-reading** — does the bot need to infer intent, or are there explicit labels/queue IDs?
3. **Measures quote fidelity** — would a summary lose the customer's exact words?
4. **Detects context references** — phrases like "again," "yesterday," "last week," "same as before"
5. **Flags verification gaps** — does the message require customer identity confirmation?

### Worked example from the builder's bot

The builder's ticket routing bot received these sample messages:

| Message | Flagged rows |
|---------|--------------|
| "Refund for wrong size — not a shipping question." | p3_mind_reader (infers "Refunds" queue without explicit label) |
| "It broke again after you fixed it yesterday." | p6_goldfish (references prior interaction) |
| "Where's my order? Also the promo code never applied." | p1_bundle (two jobs: order status + promo code) |
| "Cancel the subscription but keep the open return." | p1_bundle (two jobs: cancel + return) |
| "Billing charged twice; chat said shipping had the tracking." | p1_bundle (billing + shipping), p6_goldfish (references prior chat) |
| "Password reset loop — agent told me to email support@." | p6_goldfish (references prior agent interaction) |
| "Damaged box on delivery; I need a replacement and a pickup." | p1_bundle (replacement + pickup) |
| "Can someone escalate? I've been in Billing for three days." | p6_goldfish (references three-day history) |
| "Store credit never showed; ticket said Refunds owns it." | p4_small_quotable (one-liner must quote customer line), p6_goldfish (references prior ticket) |
| "App crash on checkout — same as last week's incident thread." | p6_goldfish (references last week's thread) |

---

## Verdict assignment

For each row, the analyzer assigns one verdict:

- **Caught** — the bot handles this trick task correctly
- **Slips** — the bot fails this trick task
- **Hold** — insufficient evidence to decide; needs more sample messages

---

## Defense matching

When a row marks **Slips**, the analyzer checks which defense would flip it:

| Defense ID | Defense rule | Catches which Slips |
|------------|--------------|---------------------|
| split_bundles | Force a split when there are two jobs | p1_bundle — sample #3 must open two tickets before this router ships |
| rewrite_mind_read | Ban mind-reading verbs | p3_mind_reader — no queue without five labels (or a queue id) from the message |
| name_source | Require a quoted source line | p4_small_quotable — sample #9's one-liner must quote the customer line or stay blank |

All three defenses are currently **on** for the builder's bot.

---

## Go-live gate evaluation

After all seven rows are marked, the analyzer counts **Slips** rows and applies the gate:

- **slips_to_block**: 2
- **Gate rule**: Ship stops when Slips hit your count. No soft warnings, no owners.

If Slips count ≥ 2, the analyzer returns **BLOCK SHIP**.

If Slips count < 2, the analyzer returns **CLEAR TO SHIP**.

---

## Re-run trigger

The board must re-run when:

> Re-run after prompt, model, or tool change — plus a monthly floor.

The analyzer flags the board as stale if any of these conditions are met since the last run.

---

## Output shape

The analyzer returns:

```
Board marks:
  p1_bundle: [Caught | Slips | Hold]
  p2_messy_harmless: [Caught | Slips | Hold]
  p3_mind_reader: [Caught | Slips | Hold]
  p4_small_quotable: [Caught | Slips | Hold]
  p5_hidden_library: [Caught | Slips | Hold]
  p6_goldfish: [Caught | Slips | Hold]
  p7_your_own: [Caught | Slips | Hold]

Slips count: [n]

Defenses to flip Slips:
  [row_id]: Use [defense_id]

Go-live rule:
  slips_to_block = 2
  Current Slips = [n]
  Verdict: [BLOCK SHIP | CLEAR TO SHIP]

Re-run trigger:
  Re-run after prompt, model, or tool change — plus a monthly floor.
```
