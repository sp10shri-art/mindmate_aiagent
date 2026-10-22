# MindMate AI — Adaptive Memory Support and Cognitive Wellness Agent

**Tags:** `#strands-decider-2b` `#policy`

*Built with the AWS User Group Madurai Strands Decider hackathon kit*

---

## 1. Introduction

MindMate AI is an adaptive daily-support agent built for elderly people living with memory
difficulties and the family members or caregivers who look after them. It combines
**Strands Decider 2B** — a fast, structured decision model — with **Amazon Bedrock** (Nova Pro)
for natural conversation, all running on the **Amazon Bedrock AgentCore Runtime**.

The central idea is straightforward: before generating any reply, MindMate AI uses Strands
Decider 2B to classify the incoming situation into one of five clearly defined support actions.
The result is an agent that does not just respond — it first decides *how* to respond, and
explains that decision in a live panel the caregiver can watch in real time.

---

## 2. Problem Statement

More than 55 million people worldwide live with dementia, and that number is projected to
nearly triple by 2050. For many elderly people, the daily experience of memory loss involves
repeated confusion, missed medications, social withdrawal, and — in the worst moments — genuine
safety emergencies. At the same time, the caregivers supporting them face exhaustion and
information gaps: they cannot be present at every moment and often receive no structured
report of what happened between their visits.

Existing reminder apps and chatbots treat every message the same way. They do not distinguish
between a person who wants to play a word game, a person who missed a medication dose, a person
expressing loneliness, and a person who may have fallen. That inability to differentiate means
the response is often wrong, sometimes dangerously so.

A smarter first step is needed: one that reads the situation first, decides on the right
category of response with a measurable confidence score, and only then generates a reply.

---

## 3. Proposed Solution

MindMate AI introduces a **policy decision layer** between the user's message and the
generative model. Every incoming message passes through Strands Decider 2B, which answers two
typed questions:

1. **`action` (choice):** which of the five support actions fits this situation best?
2. **`urgent` (noul):** are there urgency or safety signals that need immediate guidance?

The decision takes a single forward pass. It returns a chosen action label, a confidence score,
and per-action probability values. The code branches on these numbers — it does not ask the
generative LLM to make safety decisions. Only after the action is selected does Amazon Bedrock
generate the reply, using a system prompt and tool list chosen specifically for that action.

This separation — Decider for classification, Bedrock for conversation — makes the safety
logic fast, transparent, testable, and cheap to run on every message.

---

## 4. Target Users and Their Needs

### Primary user: Margaret (the patient)

Margaret is 78, living at home with mild-to-moderate memory difficulties. Her needs:

- **Simple, short answers.** She cannot parse long paragraphs or follow complex instructions.
- **Routine reminders.** She needs to be told clearly — yes or no — whether her tablet is due,
  whether it is time for a walk, or whether lunch has happened.
- **Gentle reassurance.** When confused or sad, she needs a calm, warm, two-sentence response
  that normalises her experience without alarming her.
- **Cognitive engagement.** When she is alert and in a good mood, a short word, number, or
  memory game keeps her mind active.
- **Safety guidance she can act on.** If she reports pain, a fall, or distress, she needs
  specific contact numbers immediately — not a question or a wait.

### Secondary user: David (the caregiver)

David is Margaret's son and primary caregiver. He checks in periodically but cannot be present
all day. His needs:

- **Quick yes/no answers about the session.** Did she take her meds? Does she need reassurance?
- **Flagged events waiting for him.** Anything unusual should be logged and retrievable.
- **No false alarms.** He should only be flagged when the situation genuinely needs his attention.
- **A clear line between "noted" and "notified."** MindMate AI makes notes — it does not claim
  to have called or messaged anyone unless it actually did.

---

## 5. Main Features — Implemented in This Repository

### 5a. Daily Reminders (`DAILY_REMINDER` action)

**Implemented.** The `get_due_reminders` tool (`tools.py`) returns a `reminders_due: "yes"/"no"`
field and the due reminder text. The agent replies with exactly `"Yes — [reminder text]."` or
`"No reminders due right now."` — one line, no elaboration.

Margaret's synthetic profile includes four daily reminders:

| Time  | Reminder                                     |
|-------|----------------------------------------------|
| 08:00 | Take your morning vitamin tablet.            |
| 12:00 | Have lunch and drink a glass of water.       |
| 14:00 | 30-minute walk in the garden.                |
| 20:00 | Evening tablet and bedtime routine.          |

When the morning tablet reminder is confirmed, the session log automatically records
`meds_confirmed: "yes"` so David can query it later.

### 5b. Reassurance and Emotional Support (`REASSURANCE` action)

**Implemented.** When Strands Decider 2B classifies the situation as needing reassurance, the
agent:

1. Replies to Margaret with one or two calm, warm sentences.
2. Silently calls `flag_for_caregiver_review(source="reassurance")`, which writes a
   caregiver-addressed note: *"David, patient needs reassurance — [reason]. Noted at HH:MM.
   No urgent action needed."*

Margaret does not see the caregiver note. The session log records `reassurance_needed: True`
and the reason, so David can query it later.

### 5c. Caregiver Review and Flagging (`CAREGIVER_REVIEW` action)

**Implemented.** When the Decider selects this action — because the situation is unusual,
ambiguous, or involves repeated confusion — the agent:

1. Calls `flag_for_caregiver_review` to log a note addressed to David by name.
2. Replies to Margaret with a single yes/no line: *"Yes, David has been noted."* or
   *"No, nothing urgent — David will check in."*

All flags are stored in `SESSION_LOG["flags"]` for David to retrieve.

### 5d. Caregiver Query Mode

**Implemented.** When David starts a message with *"I am David"*, *"I'm David"*, *"This is
David"*, or similar phrases, the agent detects this in `_is_caregiver()` (`agent.py`) and
switches to caregiver mode, bypassing the Decider entirely. It uses the `get_session_summary`
tool to answer his questions directly:

- *"Did she take her meds?"* → `"Yes, the patient took their meds."` / `"No, they have not."` /
  `"Unknown — meds were not checked this session."`
- *"Does she need reassurance?"* → `"Yes, they need reassurance — [reason]."` /
  `"No, they seem fine."`
- General status → a one-sentence summary from the flags list.

### 5e. Emergency Guidance (`EMERGENCY_GUIDANCE` action)

**Implemented.** This action fires in two ways:

1. The `urgent` question in Strands Decider 2B returns `yes` (triggered by words such as
   *help, fallen, hurt, pain, chest, dizzy, scared, emergency* in the message).
2. This override is **hard-coded before the confidence check**, so a low-confidence situation
   with an urgency keyword still goes straight to emergency guidance — no delay.

The `get_emergency_guidance` tool returns:
- Step-by-step calm instructions.
- The configured emergency services number (999 in the synthetic demo).
- The caregiver's name and phone number.
- An explicit disclaimer: *"MindMate AI has NOT automatically called anyone. Please use the
  numbers above to call for help."*

### 5f. Cognitive Games (`MEMORY_GAME` action)

**Implemented.** The `start_memory_game` tool (`tools.py`) provides three game types, pitched
at approximately ages 10–15 (engaging for elderly users who retain reasonable verbal and
numerical reasoning):

**Word games** — 6 questions including: naming world capitals, finding rhymes, naming animal
verbs, listing compound words, identifying palindromes, and synonym/antonym pairing.

**Number games** — 6 problems including: 13 × 14, fraction of seats on a bus, 20% of 350,
simple algebra (x + 9 = 23), apple-pricing, and a doubling sequence.

**Memory/recall games** — 4 challenges including: ordered 5-item recall, unordered 6-item
recall, backwards repetition, and filtering words from a mixed list.

The game type (word, number, or memory) is selected by the LLM based on conversation context.
The agent presents challenges directly without softening language or offering unsolicited hints.

### 5g. Medicine Alert Tracking

**Implemented** (session-level). When the morning tablet reminder fires, `SESSION_LOG
["meds_confirmed"]` is set to `"yes"`. If it has not fired, it remains `None` (unknown).
David can query this via the caregiver mode at any time. A `"no"` state can be set manually
by a future extension.

> **Planned (not yet implemented):** persistent multi-day medication history, scheduled
> push alerts to David's device when meds are overdue, and integration with a pharmacy schedule.

### 5h. Session Log for Carers

**Implemented.** `SESSION_LOG` in `tools.py` accumulates:
- `meds_confirmed` — whether the tablet reminder was acknowledged this session.
- `reassurance_needed` and `reassurance_reason` — set whenever the reassurance path fires.
- `flags` — a timestamped list of every flagged event with its reason and source.

`get_session_summary` packages this into a concise yes/no report for David.
`log_interaction` allows the agent to write a free-text summary of any turn for the daily log.

---

## 6. Features Listed in the Request — Planned, Not Yet Implemented

The following capabilities are part of the intended vision for MindMate AI but are **not
present in the current codebase**:

| Feature | Status |
|---|---|
| **Clock / date display** | Planned — the current time is used in timestamps but not displayed to Margaret as an orientation aid |
| **Family photos** | Planned — the demo UI supports image attachment, but no photo-recognition or family-album feature exists |
| **Messaging between family members** | Planned — the current system flags notes for David but does not send messages to multiple family members |
| **Community features** | Planned — no peer or community interaction is implemented |
| **Physician support / clinical integration** | Planned — the system is explicitly not a medical diagnostic tool; physician integration would require clinical oversight beyond the scope of this submission |
| **Music games** | Planned — only word, number, and memory-recall games are implemented |
| **Tile / sorting / pattern games** | Planned — the `start_memory_game` tool covers word, number, and sequential-memory formats; tile, sorting, and pattern-matching are not yet implemented |
| **Adaptive difficulty based on cognitive stage** | Partially planned — see Section 7 |

---

## 7. Cognitive Difficulty and Adaptation

### Currently implemented

The game difficulty is fixed at a level appropriate for the target audience (broadly equivalent
to ages 10–15 for number and word tasks; short sequential-memory challenges for recall). The
LLM selects the game type (word / number / memory) based on conversational context — it tends
to choose number games after arithmetic mentions and memory games after sequential-recall
discussions.

The Decider's confidence threshold (0.65) acts as an indirect safety valve: when the user's
message is unusual or hard to classify, the agent falls back to `CAREGIVER_REVIEW` rather than
forcing a game or reminder on someone who may be distressed.

### Planned: true adaptive difficulty

A full adaptive system would track:
- Correct and incorrect answers across sessions.
- Response time as a proxy for cognitive load.
- Caregiver-confirmed cognitive stage (mild / moderate / severe impairment).

It would then select from a wider difficulty bank — simpler questions for harder days, harder
ones for good days. The `#context-memory` pattern in the kit's `decider_kit/patterns/
context_memory.py` provides a ready-made seam for this: the `DeciderMemory` class can recall
relevant notes (e.g. "prefers number games", "struggles with memory tasks on Mondays") and
filter them into the agent's context on each turn.

---

## 8. System Architecture and Technologies

### Technology stack (all from the actual repository)

| Component | Technology | File(s) |
|---|---|---|
| Decision model | Strands Decider 2B | `decider_kit/client.py`, `agent/decision.py` |
| Decision pattern | `#policy` (PolicyCheck) | `agent/decision.py`, `decider_kit/patterns/policy.py` |
| Agent orchestration | Strands Agents SDK (`strands-agents>=1.60.0`) | `agent/agent.py` |
| Generative LLM | Amazon Bedrock — Nova Pro (default) | `decider_kit/llm.py`, `.env` |
| Deployment runtime | Amazon Bedrock AgentCore Runtime | `deploy/app/agent/main.py` |
| Web demo | FastAPI + vanilla JS, served on port 8501 | `demo/server.py`, `demo/static/index.html` |
| Benchmarking | `decider_kit/bench.py` (compares Decider vs Bedrock LLM baseline) | `decider_kit/bench.py` |
| Eval cases | 24 labelled JSONL cases (12 tune / 12 test) | `agent/eval_cases.jsonl` |
| Tests | pytest, 26 tests (5 kit contract + 21 MindMate-specific) | `tests/test_mindmate.py`, `tests/test_agent_contract.py` |
| Dependencies | Python 3.10+, boto3, FastAPI, uvicorn, PyYAML, matplotlib, playwright | `requirements.txt` |

### How a message flows through the system

```
Margaret types a message
         │
         ▼
_is_caregiver(message)?
    YES ─────────────────────────────────────────────────────────────────►
         │                                                                │
         NO                                                    CAREGIVER_PROMPT
         │                                                    + get_session_summary
         ▼                                                                │
build_situation()                                                         │
  (message + last 4 turns + reminder count + urgency flag)               │
         │                                                                │
         ▼                                                                │
Strands Decider 2B                                                        │
  Q1: action (choice) → MEMORY_GAME | DAILY_REMINDER |                   │
                         REASSURANCE | CAREGIVER_REVIEW |                │
                         EMERGENCY_GUIDANCE                               │
  Q2: urgent (noul) → yes / no                                           │
         │                                                                │
  urgent.yes? ──yes──► EMERGENCY_GUIDANCE  (safety override)             │
  confidence < 0.65? ─► CAREGIVER_REVIEW   (low-confidence fallback)     │
         │                                                                │
         ▼                                                                │
Select SYSTEM_PROMPTS[action] + TOOLS_FOR[action]                        │
         │                                                                │
         ▼                                                                │
Strands Agent (Amazon Bedrock Nova Pro)                                  │
  generates reply using the selected prompt and narrow tool list          │
         │                                                                ▼
         └──────────────────────────────────────────────────────────────►
                    chat() returns:
                    { reply, decisions, model_id, latency_ms, agent, eager }
                         │
                         ▼
                    Demo UI:
                    - reply bubble with action badge
                    - decision panel: action, confidence %, probability bars,
                      ⚠ low-confidence warning if applicable
```

### The reusable `#policy` decision pattern

`SupportActionPolicy` in `agent/decision.py` is fully self-contained and reusable. To apply it
in a different application:

1. Copy `agent/decision.py`.
2. Replace `SUPPORT_ACTIONS` with your own taxonomy (any number of labelled categories).
3. Replace `SUPPORT_POLICY` with your domain's policy text.
4. Adjust `CONFIDENCE_THRESHOLD` (default 0.65).
5. Call `SupportActionPolicy(Decider()).classify(situation_text)`.
6. Branch on `verdict.action`, `verdict.confidence`, `verdict.urgent`, `verdict.low_confidence`.

The classifier is completely separate from the generative component, so it can be benchmarked
independently using `decider_kit/bench.py`.

---

## 9. A Typical User Journey

**Morning, 08:05**

Margaret opens the app and says *"Good morning! Have I had my tablet yet?"* The Decider
classifies this as `DAILY_REMINDER` (confidence 0.82, no urgency). The agent calls
`get_due_reminders`, finds R1 due, and replies: *"Yes — Take your morning vitamin tablet."*
The session log records `meds_confirmed: "yes"`.

**09:30**

Margaret says *"I keep forgetting things and it makes me feel sad."* The Decider classifies
this as `REASSURANCE` (confidence 0.79). The agent replies warmly in two sentences. In the
background, without telling Margaret, it calls `flag_for_caregiver_review(source="reassurance")`
which writes: *"David, patient needs reassurance — feelings of sadness about forgetting. Noted
at 09:30. No urgent action needed."*

**10:15**

Margaret says *"I feel like playing a word game today!"* The Decider classifies this as
`MEMORY_GAME` (confidence 0.88). The agent calls `start_memory_game("word")` and presents:
*"Give four words that rhyme with 'light' but don't contain the letters L-I-G-H-T."*

**11:45**

David checks in: *"I am David, does she need reassurance?"* The agent detects this, bypasses
the Decider, queries `get_session_summary`, and replies: *"Yes, they need reassurance —
feelings of sadness about forgetting."*

**15:20**

Margaret says *"I fell over in the garden and my arm hurts a lot."* The urgency keyword
*"hurt"* is detected in `build_situation`. The Decider asks the `urgent` question; it returns
`yes`. Emergency guidance fires immediately: *"Stay calm and stay where you are. Call for help
or press your personal alarm. Emergency Services: 999. Your caregiver David can be reached at:
555-0192."*

---

## 10. Benefits, Limitations, Privacy, and Future Improvements

### Benefits

- **One decision per message, not one guess.** The Decider produces a calibrated confidence
  score the code can branch on. There is no hidden "the LLM decided everything" behaviour.
- **Safety is hard-coded, not prompted.** Emergency guidance fires before the confidence check.
  No system prompt can talk the agent out of it.
- **Caregivers get structured, not free-text, updates.** The session log stores structured
  yes/no facts, not a chat transcript. David gets direct answers, not summaries to parse.
- **Transparent to observers.** The demo UI shows every decision the Decider made, its
  confidence, and a probability bar for each action. A caregiver watching the session can
  see exactly why the agent chose reassurance over a game.
- **Testable in isolation.** All 26 tests run offline with `FakeDecider`. The decision logic
  does not require AWS credentials to verify.

### Limitations

- **The session log is in-process only.** It resets when the server restarts. There is no
  database, no persistent history across days.
- **Benchmark results are not yet available.** The live Decider endpoint requires a valid
  `DECIDER_API_KEY`. The `eval_cases.jsonl` and bench tooling are in place; actual accuracy,
  latency, and cost figures will be populated when the endpoint is accessible.
- **Caregiver identity is trust-based.** The system accepts "I am David" at face value. There
  is no authentication. In a real deployment this would require a login or passcode.
- **One user profile.** The synthetic demo has one patient (Margaret) and one caregiver (David).
  Multi-user support is not implemented.
- **No persistent medication history.** The system tracks whether the morning tablet reminder
  was acknowledged *this session*. It does not track missed doses across days.
- **MindMate AI is not a medical device.** It does not diagnose dementia, recommend medication
  changes, or replace clinical judgement. Emergency guidance provides contact numbers — it does
  not dispatch services.

### Privacy

- All demo data is synthetic. No real names, phone numbers, medical records, or personal
  details are used in the codebase.
- The `.env` file (which would contain API keys and AWS credentials) is listed in `.gitignore`
  and is never committed.
- The session log (`SESSION_LOG`) is in-memory only and is cleared on server reset.
- Image attachments are base64-encoded and sent only to the configured LLM endpoint; they are
  not stored.

### Future improvements

| Improvement | Notes |
|---|---|
| Persistent session store | Replace in-memory `SESSION_LOG` with DynamoDB via AgentCore Memory |
| Multi-day medication tracking | Record confirmed/missed doses per day; alert David if two consecutive mornings are missed |
| Adaptive game difficulty | Use `DeciderMemory` (`#context-memory` pattern) to recall past performance and select easier or harder games accordingly |
| Tile, sorting, and pattern games | Add new game types to `start_memory_game` for richer cognitive variety |
| Music games | Rhythm and melody recall exercises for users who respond better to music than text |
| Family photo recognition | Use Bedrock vision to name people in uploaded photos, aiding autobiographical memory |
| Multi-caregiver support | Allow multiple registered caregivers to query the session log |
| Clock / orientation aid | Display the current date, time, and day of the week prominently for orientation |
| Real notification sending | Integrate Amazon SNS or SES to actually notify caregivers when flagged |
| Clinical dashboard | A read-only view for a GP or memory clinic nurse showing trends across weeks |

---

## 11. Conclusion

MindMate AI demonstrates that a small, fast, structured decision model can materially improve
the quality and safety of an AI agent's behaviour — without requiring that model to generate
prose.

Strands Decider 2B answers two specific questions per turn: what action to take, and whether
there is an emergency. Those answers take one forward pass, return a confidence score, and
feed directly into code that selects the right prompt and tool list. Amazon Bedrock Nova Pro
then does what it is good at: generating a warm, simple, contextually appropriate reply.

The result is an agent that knows the difference between a person who wants to play a game,
a person who missed their tablet, a person who is lonely, and a person who has fallen. Each
gets an appropriate response. Each decision is visible, auditable, and independently testable.

For the caregiver, the experience is equally concrete: a session summary available on demand,
notes addressed to them by name, and a clear distinction between "MindMate has noted this" and
"MindMate has contacted you" — because the system never claims to have done what it has not.

This submission is an early working prototype. The architecture is designed to grow: persistent
memory, richer games, real notifications, and clinical integration are all natural extensions
of the patterns already in place.

---

*MindMate AI — built for the AWS User Group Madurai Strands Decider Hackathon.*
*Built with the AWS User Group Madurai Strands Decider hackathon kit.*
