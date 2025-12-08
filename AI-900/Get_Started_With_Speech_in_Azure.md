# AI-900 — Azure Speech (Recognition, Synthesis & Azure Service)

## 1. Speech Recognition vs Speech Synthesis

### Speech Recognition (STT)

- Goal: **audio → text / data**
- Input:
      - Live mic audio
      - Recorded audio files
- Core models:
      - **Acoustic model** → audio → phonemes (sounds)
      - **Language model** → phonemes → words (most probable word sequence)
- Typical usages:
      - Closed captions (live or recorded video)
      - Transcripts of calls/meetings
      - Voice dictation (notes, docs)
      - Voice as input to apps/agents (commands, queries)

### Speech Synthesis (TTS)

- Goal: **text → spoken audio**
- Needs:
      - Text to speak
      - Voice (speaker) to use
- Process (conceptual):
      - Tokenize text → words
      - Assign phonetic sounds → words
      - Group into prosodic units (phrases/clauses/sentences)
      - Create phonemes + prosody (rate, pitch, volume)
      - Synthesize to audio using selected voice
- Typical usages:
      - Spoken responses in apps/agents
      - Phone IVR menus
      - Reading emails/texts hands-free
      - Announcements (airports, stations, public PA)

---

## 2. Azure Speech Service — Capabilities

- **Azure Speech service** supports:
      - **Speech to Text** (STT)
      - **Text to Speech** (TTS)
      - **Speech Translation** (speech → translated text/speech)
- Backed by Microsoft’s **Universal Language Model**
      - Optimized for:
            - **Conversational** scenarios
            - **Dictation**
      - Can be customized:
            - Acoustic model
            - Language model
            - Pronunciation model

---

## 3. Speech to Text (Azure STT)

### Real-time Transcription

- Use case: live presentations, demos, streams, meetings, assistants
- App responsibilities:
      - Capture audio (mic or streamed audio)
      - Stream audio to Azure Speech API
      - Receive transcribed text in real time

### Batch Transcription

- Use case: stored recordings (call archives, lectures, podcasts)
- Input:
      - Audio files (e.g., in Azure Storage)
      - Referenced via **SAS URI**
- Execution:
      - Jobs run **asynchronously** (best-effort scheduling)
      - No exact start-time guarantee
      - App polls or receives results when transcription completes

---

## 4. Text to Speech (Azure TTS)

- Converts **text → audible speech**
      - Can play directly from app
      - Or save to audio file (e.g., for later playback)

### Voices

- **Predefined voices**:
      - Multiple languages + regional accents
      - Includes **neural voices**:
            - More natural intonation
            - Human-like prosody
- **Custom voices**:
      - Train a voice that matches brand or specific persona
      - Then use it via TTS API

---

## 5. Speech Translation (Azure Speech Translation)

- Pipeline:
      1. **ASR**: Speech → text (source language)
      2. **Machine Translation**: Source text → target language(s)
      3. Output:
            - Translated **text**
            - Or **synthesized speech** in target language
- Supports:
      - Many source and target languages
- Integration:
      - Via **REST APIs** or **SDKs**
- Example scenarios:
      - Multilingual meetings
      - Live event captions with translation
      - Global customer support

---

## 6. Using Azure Speech — Tools & Resources

![alt text](image-47.png)

### Access Methods

- **Studio interfaces**
      - Use **Microsoft Foundry → Speech Playground** for:
            - Quick testing
            - Prototyping prompts & audio
- **CLI**
      - Scripted workflows, automation/testing
- **REST APIs & SDKs**
      - Integrate into apps:
            - STT
            - TTS
            - Translation

### Azure Resources for Speech

- To use Speech in Azure, create **one** of:

1. **Speech resource**
      - Use when:
            - Only need Azure Speech
            - Want separate access/billing just for Speech

2. **Foundry Tools resource**
      - Use when:
            - Speech will be used **with other Foundry Tools**
            - Want unified access/billing across Foundry services

