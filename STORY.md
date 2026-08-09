# Trick-task board — Story of This Run

## The bot under audit

A customer-message routing bot that assigns each incoming ticket to a queue. It already ran on real tickets. The audit determines whether it can ship before Friday's rebuild.

## Sample messages tested

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

## The seven trick tasks — all slipped

| Row | Trick task | Verdict |
|-----|------------|---------|
| p1 | Bundle detector — does the router split when two jobs arrive in one message? | Slips |
| p2 | Messy-but-harmless — does it route correctly when grammar is rough but intent is clear? | Slips |
| p3 | Mind-reader — does it infer queue without explicit labels? | Slips |
| p4 | Small quotable — does it preserve the customer's exact words or summarize them away? | Slips |
| p5 | Hidden library — does it rely on context the message never stated? | Slips |
| p6 | Goldfish — does it forget prior thread context mid-conversation? | Slips |
| p7 | It verifies the customer from the call before opening a queue. | Slips |

All seven rows returned **Slips**.

## Defenses turned on

Three defenses marked **Use**:

1. **Force a split when there are two jobs**  
   Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.

2. **Ban mind-reading verbs**  
   Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

3. **Require a quoted source line**  
   Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

## Go-live rule

**Gate sentence:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Block at:** 2 slips

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

## Outcome

With all seven rows showing Slips and the block threshold set at 2, this bot cannot ship. The board blocks release until at least five of the seven trick tasks flip to Caught or Hold.
