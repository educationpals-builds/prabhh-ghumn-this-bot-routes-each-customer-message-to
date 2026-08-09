# Trick-task board — Observable Measurements

What counts as observable evidence for each of the seven trick tasks. Use these measurements to mark Caught / Slips / Hold.

---

## p1_bundle — Two problems, one ticket

**Observable:** Count of distinct queues opened for a single inbound message.

**Pass condition:** When a message contains two separate jobs, the router opens two tickets (or flags for manual split).

**Sample message:**
> Where's my order? Also the promo code never applied.

**Measurement:** If the router produces exactly one queue assignment for this message, mark **Slips**. If it opens two queues (Order Status + Promotions) or flags for split, mark **Caught**.

---

## p2_messy_harmless — Noise that doesn't change the route

**Observable:** Same queue assignment before and after removing filler phrases.

**Pass condition:** Adding conversational noise ("you know," "like I said") does not change the queue.

**Sample message:**
> It broke again after you fixed it yesterday.

**Measurement:** Run the message with and without "after you fixed it yesterday." If the queue changes, mark **Slips**. If the queue stays the same (Technical Support), mark **Caught**.

---

## p3_mind_reader — Sense the real intent

**Observable:** Presence of explicit labels or queue identifiers in the routing decision.

**Pass condition:** The router cites at least five labels from the message text (or a queue id) — never infers intent from unstated context.

**Sample message:**
> Can someone escalate? I've been in Billing for three days.

**Measurement:** Check if the routing decision quotes "escalate" and "Billing" from the message. If the router assigns a queue based on inferred frustration without quoting source text, mark **Slips**. If it quotes the explicit labels, mark **Caught**.

---

## p4_small_quotable — Tiny summary, big quote risk

**Observable:** Quoted customer line present in the routing output.

**Pass condition:** The router's summary includes a verbatim quote from the customer message, or leaves the summary blank.

**Sample message:**
> Store credit never showed; ticket said Refunds owns it.

**Measurement:** If the router produces a summary without quoting "Store credit never showed" or "ticket said Refunds owns it," mark **Slips**. If it quotes the customer line or produces no summary, mark **Caught**.

---

## p5_hidden_library — Reference to prior ticket or thread

**Observable:** Router behavior when message references external context.

**Pass condition:** The router does not fabricate details from a referenced ticket or thread it cannot see.

**Sample message:**
> App crash on checkout — same as last week's incident thread.

**Measurement:** If the router invents details about "last week's incident thread" or assumes queue based on unseen history, mark **Slips**. If it routes only on visible text ("App crash on checkout") or flags for context lookup, mark **Caught**.

---

## p6_goldfish — Contradictory instructions in one message

**Observable:** Router handling of conflicting requests.

**Pass condition:** The router flags the contradiction or opens separate queues — never silently picks one.

**Sample message:**
> Cancel the subscription but keep the open return.

**Measurement:** If the router assigns a single queue (Cancellations or Returns) without flagging the conflict, mark **Slips**. If it flags the contradiction or opens both queues, mark **Caught**.

---

## p7_your_own — It verifies the customer from the call before opening a queue.

**Observable:** Customer verification step before queue assignment.

**Pass condition:** The router confirms customer identity from the call record before routing to any queue.

**Sample message:**
> Billing charged twice; chat said shipping had the tracking.

**Measurement:** If the router assigns a queue (Billing or Shipping) without a verification step referencing the customer's call record, mark **Slips**. If it requests or confirms customer identity before opening the queue, mark **Caught**.

---

## Measurement summary table

| Task ID | What to count or quote | Slips when… |
|---------|------------------------|-------------|
| p1_bundle | Queue count per message | One queue for two jobs |
| p2_messy_harmless | Queue before/after noise | Queue changes with filler |
| p3_mind_reader | Quoted labels in decision | Routes without quoting source |
| p4_small_quotable | Verbatim quote in summary | Summary without customer quote |
| p5_hidden_library | Fabricated external details | Invents unseen thread content |
| p6_goldfish | Contradiction flag or split | Silent single-queue pick |
| p7_your_own | Verification step present | Opens queue without verifying customer |

---

## Go-live gate

**slips_to_block:** 2

If 2 or more tasks mark **Slips**, ship stops. No soft warnings, no owners.

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.
