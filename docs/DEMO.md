# HIS-Agent Reviewer Demo Guide

This guide is for reviewers who want to verify the installable demo package quickly.

## What to Verify

HIS-Agent demonstrates the following workflow:

1. Log in to a simulated HIS workspace.
2. Capture a two-speaker doctor-patient visit.
3. Assign ASR turns to Diart speaker segments by time overlap.
4. Correct anonymous speakers into `Doctor` and `Patient` using an LLM.
5. Convert the reviewed transcript into an editable HIS task.
6. Execute the confirmed task through allowlisted browser actions.

## Five-Minute Frontend Path

1. Start the static frontend:

   ```bash
   python3 -m http.server 5500
   ```

2. Open:

   ```text
   http://127.0.0.1:5500/html/login.html
   ```

3. Log in:

   ```text
   Account: 123
   Password: 123
   ```

4. Open the floating HIS-Agent widget.

5. Try a text task first:

   ```text
   After login, open Patient Management, find P001 Zhang Wei, change the phone number to 13800138000, and save.
   ```

6. For the full voice demo, start backend, ASR, and diarization services as described in the README.

## Voice Demo Script

Use two speakers if possible.

Doctor:

```text
Good morning. Please confirm your name and tell me what brings you here today.
```

Patient:

```text
My name is Zhang Wei. I have had a cough for two days and a mild fever at night.
```

Doctor:

```text
I will record cough for two days with low-grade fever and keep the department as Respiratory Medicine.
```

Patient:

```text
Yes, please update my record with that.
```

Expected behavior:

- ASR produces final turns.
- Diart produces speaker segments.
- The transcript separates the two speakers.
- Semantic correction labels turns as Doctor and Patient.
- HIS-Agent drafts an editable task rather than executing immediately.

## Useful Health Checks

```bash
curl http://127.0.0.1:8000/api/health
curl http://127.0.0.1:8000/api/llm/test
curl http://127.0.0.1:8010/health
curl http://127.0.0.1:8020/diarization/health
```

## Expected Review Signals

- The frontend opens without a build step.
- The backend reports healthy after `.env` is configured.
- The voice flow does not execute page actions automatically.
- The visit-session task is confirmable and editable.
- No real API keys or patient data are stored in the repository.
