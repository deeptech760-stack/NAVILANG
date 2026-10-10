# NAVILANG

> **Status: planning and research. Nothing is built yet.**
> This README describes the idea, the open questions, and the candidate technologies being investigated. Nothing here is a final decision.

NAVILANG is an upcoming mobile app that teaches English through conversation. It:

- talks with you and responds interactively,
- corrects your mistakes in real time,
- helps you with pronunciation,
- helps you grow your vocabulary,
- and speaks back **in your own cloned voice** instead of a robotic one.

## Why

Most language apps are drills or a generic synthetic voice. The idea behind NAVILANG is a practice partner that listens, corrects you as you speak, and answers in a voice that sounds like you, so you hear what correct English sounds like coming from your own voice.

## Current phase

| Phase | State |
|---|---|
| Idea and scope | In progress (this document) |
| Technology research | In progress |
| Architecture and pipeline design | Being articulated below |
| Prototypes of risky parts (latency, voice clone quality, pronunciation scoring) | Not started |
| App development | Not started |

The architecture and technology sections below are working notes. They will change as the research produces evidence.

## Planned pipeline

The core experience is one real-time loop. Each step lists what it must do and the options still being researched.

```
Microphone -> Speech recognition -> Analysis (grammar + pronunciation)
           -> Tutor brain (LLM) -> Cloned-voice speech -> Speaker
                                     ^
                              Learner profile / vocabulary memory
```

### 1. Audio capture
- **Goal:** clean microphone input on iOS and Android, with streaming to the next step.
- **Open questions:** noise handling, echo cancellation while the app is also speaking, push-to-talk vs. hands-free.

### 2. Speech recognition (ASR)
- **Goal:** fast, streaming transcription of non-native English, including word-level timestamps.
- **Options to evaluate:** on-device models (e.g. Whisper-family) vs. cloud speech APIs.
- **Open questions:** accuracy on accented speech, latency, cost, offline use.

### 3. Real-time correction (grammar and word choice)
- **Goal:** detect mistakes while the learner is speaking or right after, and explain them simply.
- **Options to evaluate:** LLM-based correction, rule-based grammar checkers, or a mix.
- **Open questions:** how to correct without interrupting the flow of conversation.

### 4. Pronunciation assessment
- **Goal:** score pronunciation at phoneme or word level and point to the exact sound that was wrong.
- **Options to evaluate:** forced-alignment and phoneme-recognition models, cloud pronunciation-assessment services.
- **Open questions:** reliability across accents, how to give actionable feedback rather than a bare score.

### 5. Tutor brain (conversation and teaching)
- **Goal:** hold a natural conversation at the learner's level, steer toward target vocabulary, and adapt difficulty.
- **Options to evaluate:** hosted LLM APIs vs. smaller on-device models.
- **Open questions:** latency, cost per session, safety and age-appropriateness, prompt design for teaching.

### 6. Vocabulary and progress memory
- **Goal:** track words learned, mistakes repeated, and schedule review (spaced repetition).
- **Options to evaluate:** on-device database with optional cloud sync.
- **Open questions:** data model for "a word the learner knows", how to decide a word is mastered.

### 7. Voice cloning and speech synthesis (TTS)
- **Goal:** reply in the learner's own voice, with clear, correct English pronunciation.
- **Options to evaluate:** a voice-cloning TTS service vs. open-source models.
- **Open questions:**
  - How much recorded speech is needed to clone a voice?
  - Does a clone built from accented speech sound accented? Whether the clone can pronounce English correctly in the learner's timbre is the central research risk.
  - Latency of streaming synthesis, cost, and whether it can run on-device.

### 8. Playback and interaction loop
- **Goal:** stream the response audio as soon as the first chunk is ready so conversation feels live.
- **Open questions:** end-to-end latency budget (speech in to speech out) and how to interrupt the app mid-sentence.

## Candidate technology areas

These are areas being researched, not commitments.

| Area | Candidates being looked at |
|---|---|
| Mobile app | Cross-platform (React Native / Flutter) vs. native iOS + Android |
| Backend | A small API service for model calls, accounts and sync |
| Speech recognition | Whisper-family models, cloud ASR |
| Pronunciation scoring | Phoneme-level models, cloud assessment APIs |
| Language model | Hosted LLM APIs, smaller on-device models |
| Voice cloning / TTS | Cloud voice-cloning services, open-source TTS |
| Storage | On-device database, cloud sync for profiles |

## Privacy and consent (to be designed early)

Cloned voices and recorded speech are sensitive.

- A voice clone must only be created from the account holder's own voice, with explicit consent.
- Voice samples and clone models should be deletable by the user at any time.
- Decide early what is processed on-device versus sent to a server.

## Research questions, in priority order

1. Can a cloned voice speak correct English clearly enough to teach pronunciation?
2. Can the full loop (listen, analyze, respond, speak) stay fast enough to feel conversational?
3. How accurate is pronunciation feedback for learners with different accents?
4. What does one practice session cost per user, and can it be reduced with on-device models?
5. On-device vs. cloud for each step.

## Roadmap (tentative)

1. Finish the research notes and pick technologies for the riskiest steps.
2. Build small throwaway prototypes: ASR + correction, pronunciation scoring, voice clone + TTS.
3. Measure latency, quality and cost, then settle the architecture.
4. Build the mobile app MVP.

## Contributing

The repository is public so the research can be followed along, but the project is still in planning and is not open for contributions yet.

## License

Not chosen yet.
