# Trick-task board — Ship Gate

## Go-live rule

**Hold style:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Slips-to-block threshold:** 2

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

---

## How the gate works

This ticket-routing bot must pass the seven trick tasks before Friday's rebuild. The board currently shows **7 Slips rows** — every task failed.

Because the hold style is hard-stop at 2 Slips (no soft warnings, no owners), the bot **cannot ship**.

### Current board status

| Row | Task | Verdict |
|-----|------|---------|
| p1 | Bundle — two problems, one ticket | Slips |
| p2 | Messy harmless — noise that shouldn't block | Slips |
| p3 | Mind reader — guessing intent without evidence | Slips |
| p4 | Small quotable — tiny summary loses the quote | Slips |
| p5 | Hidden library — assumes knowledge not in message | Slips |
| p6 | Goldfish — forgets prior context | Slips |
| p7 | It verifies the customer from the call before opening a queue. | Slips |

**Total Slips:** 7  
**Threshold:** 2  
**Result:** Ship blocked

---

## Defenses marked "Use"

These defenses are active and should flip Slips rows when applied:

1. **Force a split when there are two jobs**  
   Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.

2. **Ban mind-reading verbs**  
   Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

3. **Require a quoted source line**  
   Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

---

## What must happen before ship

With 7 Slips and a threshold of 2, the bot is blocked. To ship:

1. Apply the three active defenses to the bot's routing logic
2. Re-run the board against the same specimen messages
3. Confirm Slips count drops to 1 or fewer
4. Document which tasks now show Caught

Because the hold style specifies "No soft warnings, no owners," there is no escalation path — the count must drop below threshold or the bot stays blocked.

---

## Re-run schedule

Re-run after prompt, model, or tool change — plus a monthly floor.

This means the board must re-run:
- Any time the routing prompt changes
- Any time the underlying model changes
- Any time connected tools (queue system, CRM lookup) change
- At minimum once per month even if nothing else changed
