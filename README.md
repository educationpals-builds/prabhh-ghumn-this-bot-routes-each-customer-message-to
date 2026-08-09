# Trick-task board

A stranger describes the bot they're about to trust—what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The board runs seven trick tasks against those messages, marks each **Caught / Slips / Hold**, names the Use defense that would flip each Slips row, and returns a go-live rule quoting the Slips-to-block number and the re-run trigger.

---

## What this board does

You paste:
1. A one-sentence description of your bot (what it routes, classifies, or answers).
2. The stakes—who gets hurt when it silently fails.
3. A handful of real messages the bot will face.

The board returns:
- Seven trick-task rows, each marked **Caught**, **Slips**, or **Hold**.
- For every Slips row, the defense that would flip it.
- A go-live rule: the slip count that blocks ship, and when to re-run.

---

## Worked example

**Bot:** This bot routes each customer message to a queue. It already ran on real tickets. You prove whether it can ship before Friday's rebuild.

**Sample messages:**

> Refund for wrong size — not a shipping question.  
> It broke again after you fixed it yesterday.  
> Where's my order? Also the promo code never applied.  
> Cancel the subscription but keep the open return.  
> Billing charged twice; chat said shipping had the tracking.  
> Password reset loop — agent told me to email support@.  
> Damaged box on delivery; I need a replacement and a pickup.  
> Can someone escalate? I've been in Billing for three days.  
> Store credit never showed; ticket said Refunds owns it.  
> App crash on checkout — same as last week's incident thread.

---

## The seven trick-task rows

| Row | Trick task | Verdict | Grounding message |
|-----|-----------|---------|-------------------|
| p1 | **Bundle trap** — two jobs hiding in one ticket | Slips | "Where's my order? Also the promo code never applied." |
| p2 | **Messy-harmless trap** — noisy phrasing, obvious queue | Slips | "It broke again after you fixed it yesterday." |
| p3 | **Mind-reader trap** — bot guesses intent without evidence | Slips | "Can someone escalate? I've been in Billing for three days." |
| p4 | **Small-quotable trap** — tiny summary drops the customer's words | Slips | "Store credit never showed; ticket said Refunds owns it." |
| p5 | **Hidden-library trap** — bot invents a policy not in its sources | Slips | "Password reset loop — agent told me to email support@." |
| p6 | **Goldfish trap** — bot forgets earlier context in the thread | Slips | "Billing charged twice; chat said shipping had the tracking." |
| p7 | **Your trick task** — It verifies the customer from the call before opening a queue. | Slips | "Damaged box on delivery; I need a replacement and a pickup." |

---

## Use defenses that catch Slips

| Defense | Rule | What it catches |
|---------|------|-----------------|
| **split_bundles** | Force a split when there are two jobs | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| **rewrite_mind_read** | Ban mind-reading verbs | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |
| **name_source** | Require a quoted source line | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Go-live rule

**Gate sentence:** Ship stops when Slips hit your count. No soft warnings, no owners.

**Slips-to-block:** 2

**Re-run trigger:** Re-run after prompt, model, or tool change — plus a monthly floor.

If the board shows 2 or more Slips rows, ship is blocked. No exceptions, no escalation path.

---

## One-paste rebuild block

Copy this block, replace the bracketed sections with your own bot, stakes, and messages, then run the board:

```
BOT: [One sentence — what does your bot route, classify, or answer?]

STAKES: [Who gets hurt when it silently fails?]

MESSAGES:
[Paste 5–10 real messages the bot will face, one per line.]
```

The board will return seven Caught/Slips/Hold marks, the defenses that flip each Slips, and your go-live rule.

---

See [charter.md](charter.md) for who this board serves and what the marks mean.  
See [METHOD.md](METHOD.md) for the underlying framework.  
See [VERIFY.md](VERIFY.md) for the stranger verification checklist.  
See [STORY.md](STORY.md) for the narrative of this board run.

<!-- educationpals-build-verified -->
