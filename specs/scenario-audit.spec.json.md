{
  "name": "Trick-task board",
  "description": "Machine spec for the seven-row board that audits whether a bot can ship before go-live.",
  "version": "1.0.0",
  "tasks": [
    {
      "id": "p1_bundle",
      "label": "Two problems, one ticket",
      "probe": "Does the bot split bundled requests into separate queues?",
      "example_message": "Where's my order? Also the promo code never applied.",
      "defense_id": "split_bundles"
    },
    {
      "id": "p2_messy_harmless",
      "label": "Messy but harmless",
      "probe": "Does the bot route a messy message that needs no action?",
      "example_message": "It broke again after you fixed it yesterday.",
      "defense_id": null
    },
    {
      "id": "p3_mind_reader",
      "label": "Sense the real intent",
      "probe": "Does the bot infer intent without explicit labels or queue id from the message?",
      "example_message": "Can someone escalate? I've been in Billing for three days.",
      "defense_id": "rewrite_mind_read"
    },
    {
      "id": "p4_small_quotable",
      "label": "Tiny summary, big quote risk",
      "probe": "Does the bot quote the customer line or leave the summary blank?",
      "example_message": "Store credit never showed; ticket said Refunds owns it.",
      "defense_id": "name_source"
    },
    {
      "id": "p5_hidden_library",
      "label": "Hidden library lookup",
      "probe": "Does the bot rely on unstated knowledge to pick a queue?",
      "example_message": "Password reset loop — agent told me to email support@.",
      "defense_id": null
    },
    {
      "id": "p6_goldfish",
      "label": "Goldfish memory",
      "probe": "Does the bot forget prior context when routing?",
      "example_message": "Billing charged twice; chat said shipping had the tracking.",
      "defense_id": null
    },
    {
      "id": "p7_your_own",
      "label": "It verifies the customer from the call before opening a queue.",
      "probe": "It verifies the customer from the call before opening a queue.",
      "example_message": "Cancel the subscription but keep the open return.",
      "defense_id": null
    }
  ],
  "verdict_vocabulary": ["Caught", "Slips", "Hold"],
  "defenses": [
    {
      "id": "split_bundles",
      "label": "Force a split when there are two jobs",
      "explain": "Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.",
      "status": "on"
    },
    {
      "id": "rewrite_mind_read",
      "label": "Ban mind-reading verbs",
      "explain": "Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.",
      "status": "on"
    },
    {
      "id": "name_source",
      "label": "Require a quoted source line",
      "explain": "Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.",
      "status": "on"
    }
  ],
  "go_live_controls": {
    "gate_sentence": "Ship stops when Slips hit your count. No soft warnings, no owners.",
    "slips_to_block": 2,
    "rerun_trigger": "Re-run after prompt, model, or tool change — plus a monthly floor."
  },
  "specimen": {
    "domain": "ticket routing bot",
    "messages": [
      "Refund for wrong size — not a shipping question.",
      "It broke again after you fixed it yesterday.",
      "Where's my order? Also the promo code never applied.",
      "Cancel the subscription but keep the open return.",
      "Billing charged twice; chat said shipping had the tracking.",
      "Password reset loop — agent told me to email support@.",
      "Damaged box on delivery; I need a replacement and a pickup.",
      "Can someone escalate? I've been in Billing for three days.",
      "Store credit never showed; ticket said Refunds owns it.",
      "App crash on checkout — same as last week's incident thread."
    ]
  }
}