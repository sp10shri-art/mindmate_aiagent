# MindMate AI — Demo Narration Script

**Target length:** 2 minutes 30 seconds  
**Tone:** calm, clear, professional — suitable for a hackathon judging panel  
**Format:** read aloud while the screen recording runs  

---

## [0:00 – 0:15] Opening — Title Card

> "This is MindMate AI — an adaptive daily-support agent for elderly people with memory
> difficulties, and the caregivers who look after them.
>
> Before every single reply, Strands Decider 2B classifies the situation and selects the right
> support action. The decision is visible in real time on the right side of the screen."

*Show the application loading at `http://localhost:8501`. Point to the header: agent name,
model metadata, and the Strands Decider 2B label.*

---

## [0:15 – 0:35] The Interface

> "The interface has two panels. On the left, the conversation with Margaret — our demo
> patient. On the right, every decision Strands Decider 2B makes: the chosen action,
> a confidence percentage, and a probability bar for each option.
>
> There are five quick-scenario buttons across the top. We will use each one."

*Pan slowly across the full browser window. Point to the five scenario buttons, the message
input, and the empty decision panel.*

---

## [0:35 – 0:55] Scene 1 — Cognitive Game

> "First — Margaret is feeling alert and wants to play a word game.
>
> Watch the decision panel. Strands Decider 2B runs one forward pass and selects
> 'Memory Game' with high confidence. The agent then picks a word challenge from its
> question bank and delivers it directly — no soft prompts, no hand-holding."

*Click the **🧩 Memory game** button, then Send. Wait for the reply. Point to the teal
'Memory Game' badge in the chat bubble and the decision card showing the confidence score
and probability bars.*

---

## [0:55 – 1:15] Scene 2 — Daily Reminder

> "Next — Margaret asks whether she has taken her morning tablet.
>
> The Decider selects 'Daily Reminder'. The agent checks the reminder schedule, finds one
> due, and answers with exactly one line: yes or no. No ambiguity. No follow-up questions."

*Click the **🔔 Daily reminder** button, then Send. Wait for reply. Point to the amber
'Daily Reminder' badge and the decision card. Highlight the yes/no format of the reply.*

---

## [1:15 – 1:35] Scene 3 — Reassurance

> "Now Margaret says she keeps forgetting things and feels sad. A generative model alone
> might respond in any direction. Strands Decider 2B classifies this as needing Reassurance —
> not a game, not a reminder — and the agent responds with calm, warm language.
>
> In the background, it silently logs a note for David the caregiver: 'patient needs
> reassurance'. Margaret never sees that note."

*Click the **💙 Reassurance** button, then Send. Wait for reply. Point to the blue
'Reassurance' badge. Mention the silent caregiver log.*

---

## [1:35 – 1:55] Scene 4 — Caregiver Query

> "Now David, the caregiver, checks in. He types 'I am David' to identify himself. The
> Decider is bypassed entirely — the agent switches to caregiver mode and queries the
> session log directly.
>
> He gets a structured answer: were meds taken? Does she need reassurance? Direct, factual,
> one line each."

*Type manually: `I am David, did she take her meds?` — then Send. Wait for reply.
Point to the caregiver-mode decision record and the direct yes/no reply.*

---

## [1:55 – 2:15] Scene 5 — Emergency

> "Finally — the most important scenario. Margaret says she fell over and her arm hurts.
>
> Urgency keywords are detected before the Decider even runs. The 'urgent' question returns
> yes, and emergency guidance fires immediately — before any confidence calculation can delay
> it. The agent reads out the emergency steps and the configured contact numbers. It does not
> claim to have called anyone."

*Click the **🚨 Emergency** button, then Send. Wait for reply. Point to the red
'Emergency Guidance' badge, the urgency signal bar in the decision card, and the disclaimer
in the reply.*

---

## [2:15 – 2:30] Closing

> "Every decision is logged, labelled, and visible. The confidence score is a real number the
> code branches on — not a description buried in a prompt.
>
> MindMate AI is built on Strands Decider 2B, Strands Agents SDK, and Amazon Bedrock Nova Pro,
> running on the Amazon Bedrock AgentCore Runtime.
>
> Built with the AWS User Group Madurai Strands Decider hackathon kit."

*Hold on the full browser window with all five decision cards visible in the right panel.
Fade to black.*

---

## Timing Summary

| Timestamp | Scene |
|-----------|-------|
| 0:00–0:15 | Title / opening |
| 0:15–0:35 | Interface walkthrough |
| 0:35–0:55 | Cognitive game (Memory Game) |
| 0:55–1:15 | Daily reminder |
| 1:15–1:35 | Reassurance + silent caregiver flag |
| 1:35–1:55 | Caregiver query mode |
| 1:55–2:15 | Emergency guidance |
| 2:15–2:30 | Closing |
