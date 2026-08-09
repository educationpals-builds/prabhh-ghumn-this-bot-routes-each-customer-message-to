# The Five-Split Check

**Method acronym: PRISM**

The Five-Split Check is a structured audit method for attention-based systems. Each letter names a failure mode that quiet automation can hide:

- **P** — Packing: bundling distinct jobs into one output when they need separate handling
- **R** — Reading minds: inferring intent without explicit evidence in the input
- **I** — Invisible sources: summarizing without quoting the line that justifies the summary
- **S** — Short memory: forgetting context from earlier in the same thread or session
- **M** — Missing verification: acting on identity or state claims without confirming them

---

## How the Trick-task board applies PRISM

The advanced instrument is a seven-row board. Each row is a trick task — a message designed to surface one PRISM failure. The board marks every row:

| Mark | Meaning |
|------|---------|
| **Caught** | The bot handled the trick correctly |
| **Slips** | The bot failed the trick — a defense must flip this before ship |
| **Hold** | The trick could not be tested yet (missing data, ambiguous setup) |

### The seven board rows

1. **p1_bundle** — Two problems, one ticket  
   *Sample:* "Where's my order? Also the promo code never applied."  
   Tests whether the router opens two queues or merges them.

2. **p2_messy_harmless** — Noise that shouldn't change the queue  
   *Sample:* "Refund for wrong size — not a shipping question."  
   Tests whether extra commentary derails the route.

3. **p3_mind_reader** — Intent that isn't spelled out  
   *Sample:* "It broke again after you fixed it yesterday."  
   Tests whether the router guesses a queue without explicit labels.

4. **p4_small_quotable** — Tiny summary, big quote risk  
   *Sample:* "Store credit never showed; ticket said Refunds owns it."  
   Tests whether the router quotes the customer line or invents a paraphrase.

5. **p5_hidden_library** — Reference to prior context  
   *Sample:* "App crash on checkout — same as last week's incident thread."  
   Tests whether the router retrieves or ignores the linked thread.

6. **p6_goldfish** — Forgotten earlier detail  
   *Sample:* "Can someone escalate? I've been in Billing for three days."  
   Tests whether the router remembers the queue history or starts fresh.

7. **p7_your_own** — Builder's custom trick  
   *Trick:* It verifies the customer from the call before opening a queue.  
   Tests whether the router confirms identity before acting on the message.

---

## Use / Skip defenses

When a row marks **Slips**, the board names a defense that would flip it to **Caught**. The builder toggles each defense **Use** or **Skip**:

| Defense | What it catches |
|---------|-----------------|
| **Force a split when there are two jobs** | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| **Ban mind-reading verbs** | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |
| **Require a quoted source line** | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## The go-live gate

The board enforces a hard gate before ship:

> Ship stops when Slips hit your count. No soft warnings, no owners.

- **Slips-to-block threshold:** 2  
- **Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

If the board shows 2 or more **Slips** rows, the bot cannot ship. The gate does not escalate to an owner or issue a soft warning — it blocks.

---

## Summary

The Five-Split Check (PRISM) structures the audit. The seven-row board is the instrument that applies it. Defenses flip Slips rows. The go-live gate enforces the threshold. A stranger running this board against their own bot gets the same discipline: seven trick tasks, three defenses, and a hard stop at 2 slips.
