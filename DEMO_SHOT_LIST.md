# MindMate AI — Demo Shot List

Every shot corresponds to a running instance of the application at `http://localhost:8501`.  
Perform each action exactly as described. Do not skip steps — the decision panel builds up
progressively and the final hold shot requires all five decision cards to be visible.

---

## Pre-recording checklist

- [ ] Application is running: `python -m uvicorn demo.server:app --port 8501`
- [ ] Browser is open at `http://localhost:8501`, full-screen or maximised
- [ ] Browser zoom is set to 100%
- [ ] Screen resolution is 1920×1080 (or 1280×720 minimum)
- [ ] No other browser tabs, notifications, or desktop pop-ups are visible
- [ ] `.env` is configured with valid `DECIDER_API_KEY` and AWS credentials
- [ ] Screen recorder is set to record the full browser window with system audio off
- [ ] A separate microphone is used for narration (record narration in post, not live)

---

## Shot 1 — Application loads (0:00–0:15)

| # | Action | What to show |
|---|--------|--------------|
| 1a | Open `http://localhost:8501` in a clean browser window | Page loads: header with orange dot, "MindMate AI — Daily Support Agent", model metadata line |
| 1b | Let the `/api/info` call complete | Metadata bar updates: LLM model name, Decider mode, "local agent" or "AgentCore Runtime" |
| 1c | Hold for 3 seconds | Full page visible: left panel empty chat, right panel "Decisions appear here…" placeholder |

**Camera note:** full-page static shot, no scrolling.

---

## Shot 2 — Interface walkthrough (0:15–0:35)

| # | Action | What to show |
|---|--------|--------------|
| 2a | Slowly move the mouse to the five scenario buttons | Highlight each button: 🧩 🔔 💙 👁 🚨 |
| 2b | Move mouse to the message input box | Show the placeholder text "Type a message for Margaret…" |
| 2c | Move mouse to the right panel header | Show "Strands Decider 2B — Support Action" label |
| 2d | Move mouse to the attribution line at the bottom right | Show "Built with the AWS User Group Madurai Strands Decider hackathon kit" |

**Camera note:** slow guided mouse movement, no clicking yet.

---

## Shot 3 — Cognitive game: Memory Game (0:35–0:55)

| # | Action | What to show |
|---|--------|--------------|
| 3a | Click **🧩 Memory game** scenario button | Input fills with: "I feel like playing a word game today!" |
| 3b | Click **Send** | User message bubble appears on the left, "thinking…" placeholder appears |
| 3c | Wait for the reply to arrive | Agent reply with teal **🧩 Memory Game** badge, followed by a word challenge |
| 3d | Move mouse to the right panel | Decision card appears: teal "Memory Game" tag, confidence % pill (green if ≥65%), probability bars for all five actions, "confident" green tag |
| 3e | Hold for 3 seconds | Both panels visible simultaneously |

**What judges see:** Decider chose MEMORY_GAME, confidence displayed numerically, bars show the relative probability of each action.

---

## Shot 4 — Daily reminder (0:55–1:15)

| # | Action | What to show |
|---|--------|--------------|
| 4a | Click **🔔 Daily reminder** scenario button | Input fills with: "Have I taken my morning tablet yet?" |
| 4b | Click **Send** | User bubble appears |
| 4c | Wait for reply | Agent reply with amber **🔔 Daily Reminder** badge; reply starts "Yes —" followed by reminder text |
| 4d | Move mouse to the decision card | Amber "Daily Reminder" tag, confidence pill, probability bars |
| 4e | Point mouse at the reply text | Show the "Yes —" yes/no format clearly |

**What judges see:** Decider chose DAILY_REMINDER, reply is exactly one line starting with yes/no.

---

## Shot 5 — Reassurance (1:15–1:35)

| # | Action | What to show |
|---|--------|--------------|
| 5a | Click **💙 Reassurance** scenario button | Input fills with: "I keep forgetting things and it makes me feel sad." |
| 5b | Click **Send** | User bubble appears |
| 5c | Wait for reply | Agent reply with blue **💙 Reassurance** badge; calm two-sentence response |
| 5d | Move mouse to the decision card | Blue "Reassurance" tag, confidence pill |
| 5e | Briefly pause on the reply | Two sentences only — no follow-up questions, no mention of the caregiver flag |

**What judges see:** Decider chose REASSURANCE, reply is warm and brief, silent caregiver note was logged (not shown to the user).

---

## Shot 6 — Caregiver query mode (1:35–1:55)

| # | Action | What to show |
|---|--------|--------------|
| 6a | Click the message input box | Focus the input |
| 6b | Type manually (do not use a scenario button): `I am David, did she take her meds?` | Show the typed text clearly before sending |
| 6c | Click **Send** | User bubble with the "I am David…" message |
| 6d | Wait for reply | Direct reply: "Yes, the patient took their meds." or "Unknown — meds were not checked this session." |
| 6e | Move mouse to the decision card | Card shows `caregiver-mode` stage, `CAREGIVER_DIRECT` action, note: "Caregiver identified — Decider bypassed, session summary served." |

**What judges see:** Caregiver identification bypasses the Decider, session log is queried, direct yes/no answer is returned.

---

## Shot 7 — Emergency guidance (1:55–2:15)

| # | Action | What to show |
|---|--------|--------------|
| 7a | Click **🚨 Emergency** scenario button | Input fills with: "I fell over and my arm hurts a lot." |
| 7b | Click **Send** | User bubble appears |
| 7c | Wait for reply | Agent reply with red **🚨 Emergency Guidance** badge; step-by-step guidance and contact numbers |
| 7d | Move mouse to the decision card | Red "Emergency Guidance" tag; `urgent signal` bar is green and high (near 1.0); confidence pill; "confident" tag |
| 7e | Slowly scroll the reply if needed | Show the disclaimer: "MindMate AI has NOT automatically called anyone." |

**What judges see:** urgency signal detected, EMERGENCY_GUIDANCE forced regardless of other probabilities, disclaimer present.

---

## Shot 8 — Final hold (2:15–2:30)

| # | Action | What to show |
|---|--------|--------------|
| 8a | Scroll the right decision panel to the top | All five decision cards visible: teal, amber, blue, caregiver-mode, red |
| 8b | Move mouse away from all UI elements | Clean view of both panels |
| 8c | Hold for 10 seconds | Full browser window — left panel shows all five conversation turns, right panel shows all five decision cards |
| 8d | Fade to black in post-production | — |

---

## Backup shots (record these in case of a slow API response)

| Shot | Message to type | Purpose |
|------|----------------|---------|
| B1 | `Good morning! I feel great today.` | Alternative opening — REASSURANCE path |
| B2 | `Can we play a number game?` | Alternative MEMORY_GAME path showing number challenge |
| B3 | `I am David, does she need reassurance?` | Alternative caregiver query |
| B4 | `I can't breathe and I feel dizzy.` | Alternative EMERGENCY_GUIDANCE trigger |

---

## Notes on timing

- Each API call to Bedrock typically takes 3–8 seconds. Budget this into pauses.
- If a response is unexpectedly slow, continue narrating — do not cut the recording.
- The decision card animates in with a pop effect (`@keyframes pop`) — make sure this is
  visible in the recording; do not skip past it.
