# MindMate AI — Demo Recording Instructions

This document explains how to set up the application, configure a screen recorder, and
produce the demo video. Read it fully before you start recording.

---

## 1. Prerequisites

### Software

| Tool | Purpose | Where to get it |
|------|---------|-----------------|
| Python 3.10+ | Run the application | Already installed |
| Screen recorder | Capture the browser | See options below |
| Microphone | Record narration | Any USB or built-in mic |
| Video editor (optional) | Add call-out text, fade transitions | DaVinci Resolve (free) or any editor |

### Screen recorder options

| Tool | Platform | Notes |
|------|----------|-------|
| **OBS Studio** | Windows / macOS / Linux | Free, full control, recommended |
| **ShareX** | Windows | Free, lightweight |
| **QuickTime Player** | macOS | Built-in, simple |
| **Loom** | Windows / macOS | Web-based, exports quickly |

---

## 2. Start the Application

Open a terminal in the project root and run:

```powershell
# Windows PowerShell
python -m uvicorn demo.server:app --port 8501
```

Wait for the line:
```
INFO:     Uvicorn running on http://127.0.0.1:8501 (Press CTRL+C to quit)
```

Open a browser and navigate to:
```
http://localhost:8501
```

Verify the header shows "MindMate AI — Daily Support Agent" and the metadata bar has loaded
the model information. If the metadata bar is empty, your `.env` file may not be configured.

### Required `.env` values

Copy `.env.example` to `.env` if you have not already:

```powershell
Copy-Item .env.example .env
```

Open `.env` and fill in at minimum:

```
DECIDER_MODE=ugmdu
DECIDER_URL=https://da25tnxy06gjk.cloudfront.net
DECIDER_API_KEY=your_key_here
LLM_PROVIDER=bedrock
BEDROCK_MODEL_ID=us.amazon.nova-pro-v1:0
AWS_REGION=us-east-1
```

You also need valid AWS credentials in your environment (`AWS_ACCESS_KEY_ID`,
`AWS_SECRET_ACCESS_KEY`, or an active SSO session).

---

## 3. Configure the Screen Recorder

### Resolution and frame rate

| Setting | Recommended value |
|---------|------------------|
| Capture area | Full browser window (not full desktop) |
| Resolution | 1920×1080 (minimum 1280×720) |
| Frame rate | 30 fps |
| Audio | Off (record narration separately) |
| File format | MP4 (H.264) |

### OBS Studio setup (step-by-step)

1. Open OBS Studio.
2. Under **Sources**, click **+** → **Window Capture**.
3. Select your browser window (not the desktop).
4. Set **Output** → **Recording Path** to a known folder.
5. Under **Settings** → **Output** → **Recording**, set Format to `mp4`, Encoder to `x264`.
6. Under **Settings** → **Video**, set Base Resolution and Output Resolution to `1920×1080`.
7. Set FPS to `30`.
8. Disable all audio inputs under **Audio Mixer** (you are recording narration separately).
9. Click **Start Recording** when ready.

### Browser preparation

- Set browser zoom to **100%** (`Ctrl+0` / `Cmd+0`)
- Close all other browser tabs
- Disable browser notifications (`Settings → Notifications → Block all`)
- Set browser to full-screen (`F11`) or maximise the window
- Clear any previous session by refreshing the page (`F5`) before recording

---

## 4. Recording Order

Follow the shot list in `DEMO_SHOT_LIST.md` exactly. The order is:

1. Application load and interface walkthrough
2. 🧩 Memory game (click scenario button)
3. 🔔 Daily reminder (click scenario button)
4. 💙 Reassurance (click scenario button)
5. Caregiver query — **type manually**: `I am David, did she take her meds?`
6. 🚨 Emergency (click scenario button)
7. Final hold — scroll right panel to show all five decision cards

**Do not use the scenario buttons for the caregiver turn.** You must type the message
manually so it starts with "I am David" to trigger caregiver detection.

---

## 5. Timing Guide

Each scene should occupy roughly the following time. Pause briefly after clicking Send and
wait for the reply to arrive before moving the mouse to the decision panel.

| Scene | Target duration | Wait for API |
|-------|----------------|--------------|
| Load + walkthrough | 20 s | — |
| Memory game | 20 s | ~4 s |
| Daily reminder | 20 s | ~3 s |
| Reassurance | 20 s | ~4 s |
| Caregiver query | 20 s | ~3 s |
| Emergency | 20 s | ~4 s |
| Final hold | 15 s | — |
| **Total** | **~2:15** | |

If a response takes longer than 8 seconds, continue narrating naturally — do not cut.

---

## 6. Recording Narration

Record narration as a separate audio track using the script in `DEMO_SCRIPT.md`.

**Options:**
- Record live into a USB microphone while the screen recording runs simultaneously.
  Sync in post using the first mouse click as a reference point.
- Record the screen silently, then record the narration audio separately into an audio
  editor (Audacity is free), then combine in a video editor.

**Microphone tips:**
- Speak 20–30 cm from the microphone
- Use a quiet room with no echo
- Do one dry run reading the script aloud before recording

---

## 7. Post-Production Checklist

After recording, open the video in a video editor and:

- [ ] Trim any dead time at the start and end
- [ ] Add call-out text boxes where noted in `DEMO_STORYBOARD.md`
- [ ] Add 0.5 s black fade at the end
- [ ] Sync narration audio to the screen recording
- [ ] Add closed captions for the narration (required for accessibility)
- [ ] Export as MP4, H.264, 1920×1080, 30 fps, under 500 MB
- [ ] Verify total runtime is under 3 minutes

### Call-out text style (for consistency)

- Font: the same font as the UI (Inter) or a clean sans-serif
- Size: 16–18 px equivalent in the video
- Background: semi-transparent dark box (`rgba(0,0,0,0.7)`)
- Text colour: white `#E7ECF6`
- Position: bottom-left of screen, clear of UI elements

---

## 8. What to Highlight for Judges

The judging panel will look for:

1. **Strands Decider 2B is actually used** — show the decision card populating after every
   message. The confidence % and probability bars prove a real model classification ran.

2. **Five distinct action paths** — each scenario button produces a visually distinct badge
   colour and a different card in the decision panel.

3. **Safety is hard-coded** — the emergency scenario shows `urgent signal` near 1.0 and
   EMERGENCY_GUIDANCE probability bar full, even before any confidence logic.

4. **Caregiver mode is separate** — the `caregiver-mode` card has a different stage label
   and says "Decider bypassed", proving the classification path switched entirely.

5. **Low-confidence handling** — if time permits, type a vague message such as
   `I maybe want something, I think.` The decision card may show a ⚠ warning and route
   to CAREGIVER_REVIEW with an amber "low-confidence" tag instead of guessing.

---

## 9. Troubleshooting

| Problem | Likely cause | Fix |
|---------|-------------|-----|
| Metadata bar is empty after load | `.env` not configured or server not running | Check terminal output; verify `.env` has DECIDER_API_KEY |
| Reply never arrives | API key invalid or Bedrock credentials missing | Check terminal for error; verify AWS credentials |
| Decision panel stays empty | `decisions` array is empty in API response | Check browser console for errors; restart the server |
| "I am David" not triggering caregiver mode | Message does not start with the exact phrase | Ensure you typed `I am David` at the beginning of the message |
| Video file too large | Encoder bitrate too high | Re-export with CRF 23 in OBS or reduce to 720p |

---

## 10. File Reference

| File | Purpose |
|------|---------|
| `DEMO_SCRIPT.md` | Narration script with timestamps |
| `DEMO_SHOT_LIST.md` | Step-by-step actions to perform on screen |
| `DEMO_STORYBOARD.md` | Scene descriptions and on-screen text |
| `DEMO_README.md` | This file — recording instructions |
| `agent/demo_script.yaml` | Machine-readable script used by `make_video.py` |

---

*Built with the AWS User Group Madurai Strands Decider hackathon kit.*

---

## Automated Video Generation (no manual recording)

Use `scripts/make_video_silent.py` to generate the demo video fully automatically.

### Step 1 — Install the Chromium browser (one-time, ~150 MB)

```powershell
python -m playwright install chromium
```

This is the only additional download required. Everything else (ffmpeg, PIL, numpy,
Playwright) is already installed via `requirements.txt`.

### Step 2 — Ensure `.env` is configured

The script starts the demo server automatically and makes real API calls.
Your `.env` must contain valid values for:
```
DECIDER_API_KEY=...
AWS_REGION=us-east-1
BEDROCK_MODEL_ID=us.amazon.nova-pro-v1:0
```
Plus valid AWS credentials in your environment.

### Step 3 — Run the generator

```powershell
python scripts/make_video_silent.py
```

Optional flags:
```
--port 8502          Demo server port (default 8502, avoids clash with 8501)
--fps  30            Output frame rate
--out  videos/mindmate_demo.mp4
```

### What it produces

`videos/mindmate_demo.mp4` — a 2–3 minute, 1280×720, 30 fps MP4 with:
- Title card with project name and tech stack
- Interface overview screenshot with subtitle overlay
- Five scene intro cards (one per action)
- Live screenshots from the real running application
- Decision detail cards showing action, confidence, and latency
- End card with tags and attribution
- Smooth 0.5 s crossfade transitions between all scenes
- No voice-over (subtitles are rendered as on-screen text)

### What it does NOT produce

- Voice narration (intentionally omitted — subtitles cover it)
- Background music (no royalty-free audio pipeline is included)

### Troubleshooting

| Error | Fix |
|-------|-----|
| `BrowserType.launch: Executable doesn't exist` | Run `python -m playwright install chromium` |
| `Demo server did not start` | Check your `.env` has valid API keys and AWS credentials |
| API timeout during a scenario | The Bedrock/Decider endpoint is slow; re-run and it usually succeeds |
| `ffmpeg failed` | Ensure `imageio-ffmpeg` is installed: `pip install imageio-ffmpeg` |
