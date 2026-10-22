# MindMate AI — Demo Storyboard

Eight scenes. Each scene has a description of what is on screen, the on-screen text to
highlight, the narration cue, and the transition to the next scene.

---

## Scene 1 — Title / Application Load
**Duration:** 0:00 – 0:15

```
┌─────────────────────────────────────────────────────────────────────┐
│  ● MindMate AI — Daily Support Agent          LLM: Nova Pro · …    │
├───────────────────────────────┬─────────────────────────────────────┤
│  Conversation with Margaret   │  Strands Decider 2B — Support       │
│                               │  Action                             │
│  [🧩][🔔][💙][👁][🚨]        │                                     │
│                               │  Decisions appear here as the       │
│                               │  agent processes each message.      │
│  ┌────────────────────────┐   │                                     │
│  │ Type a message…   Send │   │                                     │
│  └────────────────────────┘   │                                     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- Header: `MindMate AI — Daily Support Agent`
- Metadata bar: model name, Decider mode
- Right panel label: `Strands Decider 2B — Support Action`

**Narration cue:** *"This is MindMate AI — an adaptive daily-support agent…"*

**Transition:** hold → slow mouse sweep across the interface

---

## Scene 2 — Interface Walkthrough
**Duration:** 0:15 – 0:35

```
┌─────────────────────────────────────────────────────────────────────┐
│  ● MindMate AI — Daily Support Agent                                │
├───────────────────────────────┬─────────────────────────────────────┤
│  [🧩 Memory game] ←mouse      │  Strands Decider 2B — Support       │
│  [🔔 Daily reminder]          │  Action                 ←mouse      │
│  [💙 Reassurance]             │                                     │
│  [👁 Caregiver review]        │  "Decisions appear here…"           │
│  [🚨 Emergency]               │                                     │
│                               │                                     │
│  ┌────────────────────────┐   │                                     │
│  │ Type a message…  ←mouse│   │                                     │
│  └────────────────────────┘   │                                     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- Five scenario buttons (mouse hovers each)
- Message input placeholder text
- Right panel heading

**Narration cue:** *"The interface has two panels…"*

**Transition:** mouse settles on 🧩 button → click

---

## Scene 3 — Cognitive Game (Memory Game)
**Duration:** 0:35 – 0:55

```
┌───────────────────────────────┬─────────────────────────────────────┐
│  Conversation with Margaret   │  Strands Decider 2B — Support       │
│                               │  Action                             │
│  ┌──────────────────────────┐ │ ┌───────────────────────────────┐   │
│  │ User                     │ │ │ support-action                │   │
│  │ I feel like playing a    │ │ │ 🧩 Memory Game  [82%] [conf.] │   │
│  │ word game today!         │ │ │ ▓▓▓▓▓▓▓▓▓░ MEMORY_GAME       │   │
│  └──────────────────────────┘ │ │ ░░░░░░░░░░ DAILY_REMINDER     │   │
│                               │ │ ░░░░░░░░░░ REASSURANCE        │   │
│  ┌──────────────────────────┐ │ │ ░░░░░░░░░░ CAREGIVER_REVIEW   │   │
│  │ 🧩 Memory Game           │ │ │ ░░░░░░░░░░ EMERGENCY_GUIDANCE │   │
│  │ Give four words that     │ │ └───────────────────────────────┘   │
│  │ rhyme with 'light'…      │ │                                     │
│  │ 342 ms · 1 decision      │ │                                     │
│  └──────────────────────────┘ │                                     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- Teal `🧩 Memory Game` action badge in the chat bubble
- Decision card: `support-action`, teal `🧩 Memory Game` tag, confidence % pill (green)
- Probability bars — MEMORY_GAME bar filled green, others short and red
- `confident` green tag (bottom right of card)

**Narration cue:** *"Watch the decision panel. Strands Decider 2B runs one forward pass…"*

**Transition:** hold → click 🔔 button

---

## Scene 4 — Daily Reminder
**Duration:** 0:55 – 1:15

```
┌───────────────────────────────┬─────────────────────────────────────┐
│  Conversation with Margaret   │  Strands Decider 2B — Support       │
│                               │  Action                             │
│  [previous turn above]        │ ┌───────────────────────────────┐   │
│                               │ │ support-action                │   │
│  ┌──────────────────────────┐ │ │ 🔔 Daily Reminder  [78%] [conf│   │
│  │ User                     │ │ │ ░░░░░░░░░░ MEMORY_GAME        │   │
│  │ Have I taken my morning  │ │ │ ▓▓▓▓▓▓▓▓▓░ DAILY_REMINDER     │   │
│  │ tablet yet?              │ │ │ ░░░░░░░░░░ REASSURANCE        │   │
│  └──────────────────────────┘ │ │ ░░░░░░░░░░ CAREGIVER_REVIEW   │   │
│                               │ │ ░░░░░░░░░░ EMERGENCY_GUIDANCE │   │
│  ┌──────────────────────────┐ │ └───────────────────────────────┘   │
│  │ 🔔 Daily Reminder        │ │                                     │
│  │ Yes — Take your morning  │ │                                     │
│  │ vitamin tablet.          │ │                                     │
│  │ 287 ms · 1 decision      │ │                                     │
│  └──────────────────────────┘ │                                     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- Amber `🔔 Daily Reminder` badge
- Reply text: exactly `"Yes — Take your morning vitamin tablet."` — one line
- Decision card: amber tag, confidence %, DAILY_REMINDER bar filled

**Narration cue:** *"The Decider selects 'Daily Reminder'. The agent answers with exactly one line…"*

**Transition:** hold → click 💙 button

---

## Scene 5 — Reassurance
**Duration:** 1:15 – 1:35

```
┌───────────────────────────────┬─────────────────────────────────────┐
│  Conversation with Margaret   │  Strands Decider 2B — Support       │
│                               │  Action                             │
│  [previous turns above]       │ ┌───────────────────────────────┐   │
│                               │ │ support-action                │   │
│  ┌──────────────────────────┐ │ │ 💙 Reassurance  [74%] [conf.] │   │
│  │ User                     │ │ │ ░░░░░░░░░░ MEMORY_GAME        │   │
│  │ I keep forgetting things │ │ │ ░░░░░░░░░░ DAILY_REMINDER     │   │
│  │ and it makes me feel sad.│ │ │ ▓▓▓▓▓▓▓░░░ REASSURANCE        │   │
│  └──────────────────────────┘ │ │ ░░░░░░░░░░ CAREGIVER_REVIEW   │   │
│                               │ │ ░░░░░░░░░░ EMERGENCY_GUIDANCE │   │
│  ┌──────────────────────────┐ │ └───────────────────────────────┘   │
│  │ 💙 Reassurance           │ │                                     │
│  │ That is completely       │ │                                     │
│  │ understandable, Margaret.│ │                                     │
│  │ 391 ms · 1 decision      │ │                                     │
│  └──────────────────────────┘ │                                     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- Blue `💙 Reassurance` badge
- Reply: warm, two sentences — no questions, no mention of David
- Decision card: blue tag, confidence %

**Call-out graphic (add in post):** small text box reading:
> *"Silent background action: caregiver note logged — 'David, patient needs reassurance'"*
> *"Margaret never sees this."*

**Narration cue:** *"In the background, it silently logs a note for David…"*

**Transition:** hold → manually type in input field

---

## Scene 6 — Caregiver Query Mode
**Duration:** 1:35 – 1:55

```
┌───────────────────────────────┬─────────────────────────────────────┐
│  Conversation with Margaret   │  Strands Decider 2B — Support       │
│                               │  Action                             │
│  [previous turns above]       │ ┌───────────────────────────────┐   │
│                               │ │ caregiver-mode                │   │
│  ┌──────────────────────────┐ │ │ CAREGIVER_DIRECT   [bypassed] │   │
│  │ User                     │ │ │ "Caregiver identified —        │   │
│  │ I am David, did she take │ │ │  Decider bypassed,            │   │
│  │ her meds?                │ │ │  session summary served."     │   │
│  └──────────────────────────┘ │ └───────────────────────────────┘   │
│                               │                                     │
│  ┌──────────────────────────┐ │                                     │
│  │ Yes, the patient took    │ │                                     │
│  │ their meds.              │ │                                     │
│  │ 198 ms · 1 decision      │ │                                     │
│  └──────────────────────────┘ │                                     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- Decision card stage: `caregiver-mode` (not `support-action`)
- `CAREGIVER_DIRECT` action label
- Note: "Caregiver identified — Decider bypassed, session summary served."
- Reply: one-line direct answer

**Call-out graphic (add in post):** small text box:
> *"'I am David' triggers caregiver mode — Decider skipped, session log queried directly"*

**Narration cue:** *"Now David, the caregiver, checks in…"*

**Transition:** hold → click 🚨 button

---

## Scene 7 — Emergency Guidance
**Duration:** 1:55 – 2:15

```
┌───────────────────────────────┬─────────────────────────────────────┐
│  Conversation with Margaret   │  Strands Decider 2B — Support       │
│                               │  Action                             │
│  [previous turns above]       │ ┌───────────────────────────────┐   │
│                               │ │ support-action                │   │
│  ┌──────────────────────────┐ │ │ 🚨 Emergency Guidance  [94%]  │   │
│  │ User                     │ │ │ urgent signal  ▓▓▓▓▓▓▓▓▓▓ 0.9│   │
│  │ I fell over and my arm   │ │ │ ░░░░░░░░░░ MEMORY_GAME        │   │
│  │ hurts a lot.             │ │ │ ░░░░░░░░░░ DAILY_REMINDER     │   │
│  └──────────────────────────┘ │ │ ░░░░░░░░░░ REASSURANCE        │   │
│                               │ │ ░░░░░░░░░░ CAREGIVER_REVIEW   │   │
│  ┌──────────────────────────┐ │ │ ▓▓▓▓▓▓▓▓▓▓ EMERGENCY_GUIDANCE │   │
│  │ 🚨 Emergency Guidance    │ │ └───────────────────────────────┘   │
│  │ Stay calm and stay where │ │                                     │
│  │ you are.                 │ │                                     │
│  │ Emergency Services: 999  │ │                                     │
│  │ David: 555-0192          │ │                                     │
│  │ (MindMate has NOT called) │ │                                     │
│  │ 412 ms · 1 decision      │ │                                     │
│  └──────────────────────────┘ │                                     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- Red `🚨 Emergency Guidance` badge
- Decision card: `urgent signal` bar nearly full and green (high urgency probability)
- EMERGENCY_GUIDANCE probability bar full green
- Reply: step-by-step steps + contact numbers
- Disclaimer line: "MindMate AI has NOT automatically called anyone."

**Call-out graphic (add in post):** text box:
> *"'hurt' keyword → urgent=0.9 → EMERGENCY_GUIDANCE fires before confidence check"*

**Narration cue:** *"Urgency keywords are detected before the Decider even runs…"*

**Transition:** hold → scroll right panel to show all cards

---

## Scene 8 — Final Hold
**Duration:** 2:15 – 2:30

```
┌───────────────────────────────┬─────────────────────────────────────┐
│  Conversation with Margaret   │  Strands Decider 2B — Support       │
│  (5 turns visible)            │  Action                             │
│                               │ ┌─────────────────────────────┐     │
│  🧩 Word game reply           │ │ 🚨 Emergency Guidance  [94%] │     │
│  🔔 Tablet reminder reply     │ ├─────────────────────────────┤     │
│  💙 Reassurance reply         │ │ CAREGIVER_DIRECT [bypassed]  │     │
│  [David meds answer]          │ ├─────────────────────────────┤     │
│  🚨 Emergency reply           │ │ 💙 Reassurance         [74%] │     │
│                               │ ├─────────────────────────────┤     │
│                               │ │ 🔔 Daily Reminder      [78%] │     │
│                               │ ├─────────────────────────────┤     │
│                               │ │ 🧩 Memory Game         [82%] │     │
│                               │ └─────────────────────────────┘     │
│                               │  Built with the AWS User Group…     │
└───────────────────────────────┴─────────────────────────────────────┘
```

**On-screen text to highlight:**
- All five decision cards in the right panel
- Each card's action label and confidence badge
- Attribution line at the bottom right

**Narration cue:** *"Every decision is logged, labelled, and visible…"*

**Transition:** fade to black

---

## Post-production notes

| Element | Recommendation |
|---------|----------------|
| Call-out text boxes | White text on semi-transparent dark background; 16–18px |
| Arrow pointers | Simple white arrow; no heavy motion graphics |
| Fade transitions | 0.5 s black fade between scenes if cutting; otherwise hold |
| Background music | None recommended — clean and professional |
| Captions | Add closed captions for the narration |
| Planned features | If mentioned verbally, add a brief on-screen text tag: `[PLANNED — not yet implemented]` |
