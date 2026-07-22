# RunPod Whisper Diarization Worker

> Serverless worker for audio transcription with speaker diarization using Whisper and pyannote.

Deploy this as a RunPod serverless endpoint to transcribe audio files with automatic speaker identification. Audio is sent as base64, transcribed with OpenAI's Whisper, and speaker segments are identified using pyannote's diarization pipeline.

## Features

- **Automatic GPU/CPU detection** — Uses CUDA when available, falls back to CPU
- **Base64 audio input** — Send audio files encoded as base64
- **Speaker diarization** — Identifies who spoke when, with timestamps
- **Automatic resampling** — Resamples audio to 16kHz for pyannote compatibility
- **Temp file cleanup** — Removes temporary files after processing

## Future improvements

- Switch to WhisperX for batched inference (wav2vec2 alignment, faster-whisper etc.)
- Configurable min_speakers and max_speakers
- VAC (Voice Activity Detection)
- Try out more diarization frameworks such as DiariZen

## Prerequisites

- A [RunPod](https://runpod.io) account
- A [HuggingFace](https://huggingface.co) account with access to pyannote models
- You must accept the user conditions for the following models:
  - https://huggingface.co/pyannote/segmentation-3.0
  - https://huggingface.co/pyannote/speaker-diarization-3.1
  - https://huggingface.co/pyannote/speaker-diarization-community-1

## Deployment

### Create new serverless endpoint

<img width="851" height="363" alt="CleanShot 2025-12-16 at 18 59 23" src="https://github.com/user-attachments/assets/0461374d-f3dd-40f9-894c-ae60551e1311" />

### Select repo

<img width="1376" height="261" alt="CleanShot 2025-12-16 at 18 59 58" src="https://github.com/user-attachments/assets/5fa6cfe4-cc9f-449a-943e-19cc426889e0" />

### Click on Next

<img width="1388" height="426" alt="CleanShot 2025-12-16 at 19 00 16" src="https://github.com/user-attachments/assets/e9bd8d85-8551-4a0b-a53d-b961bb1234cd" />

### Select GPUs

16- or 24GB is enough for this workload.

<img width="1378" height="1094" alt="CleanShot 2025-12-16 at 19 00 31" src="https://github.com/user-attachments/assets/ce54845b-ea13-49d0-b39b-eb58d1020ae6" />

### Environment variables

<img width="1374" height="351" alt="CleanShot 2025-12-16 at 19 03 26" src="https://github.com/user-attachments/assets/fe0bbf31-29e4-4521-89bb-23e4ee237310" />

| Variable | Description |
| --- | --- |
| `HUGGINGFACE_ACCESS_TOKEN` | HuggingFace token with access to pyannote models |
| `ENV` | Set to `prod` to force CPU fallback (optional) |

### Click on Deploy Endpoint

<img width="1371" height="66" alt="CleanShot 2025-12-16 at 19 03 36" src="https://github.com/user-attachments/assets/c5172185-bbbd-4443-a980-507577c151db" />

## API

**Input:**
```json
{
  "input": {
    "file": "base64_encoded_audio",
    "ext": "mp3"
  }
}
```

**Output:**
```json
[
  {
    "start": 0.0,
    "end": 5.2,
    "speaker": "SPEAKER_00",
    "text": "Transcribed text segment..."
  }
]
```

## Local testing

```bash
./test_local.sh
```

This creates a virtual environment, installs dependencies, and runs the handler.

## License (MIT)

Copyright © 2025 [navopw](https://github.com/navopw)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
