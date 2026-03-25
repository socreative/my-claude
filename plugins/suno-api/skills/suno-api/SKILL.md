---
name: suno-api
description: Generates music, lyrics, sound effects, and audio using the Suno AI API (sunoapi.org). Use this skill when the user asks to create music, generate songs, write lyrics with AI, create sound effects, extend or remix audio, separate vocals, generate MIDI, or work with any Suno API integration.
---

# Suno AI Music Generation API

You are an expert in the Suno API for AI-powered music generation, lyrics creation, audio processing, and video production. The API is hosted at `https://api.sunoapi.org`.

## Authentication

All requests require a Bearer token in the Authorization header:

```
Authorization: Bearer YOUR_API_KEY
```

API keys are obtained from https://sunoapi.org/api-key

## Rate Limits & Constraints

- **Max concurrency:** 20 requests per 10 seconds
- **File retention:** Generated files are retained for 14-15 days before automatic deletion
- Exceeding rate limits returns HTTP 430

## AI Model Versions

| Model | Features | Max Duration |
|-------|----------|-------------|
| `V4` | Improved Vocals | 4 min |
| `V4_5` | Smart Prompts | 8 min |
| `V4_5PLUS` | Richer Tones | 8 min |
| `V4_5ALL` | Better Song Structure | 8 min |
| `V5` | Latest Model | - |

## Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 400 | Invalid parameters |
| 401 | Unauthorized |
| 404 | Invalid method/path |
| 405 | Rate limit exceeded |
| 413 | Theme/prompt too long |
| 429 | Insufficient credits |
| 430 | Call frequency too high |
| 455 | System maintenance |
| 500 | Server error |

---

## Callback Pattern

All generation endpoints are **asynchronous**. They return `{ code: 200, data: { taskId } }` immediately. Results are delivered via webhook POST to the provided `callBackUrl` in stages:

- `text` — lyrics/text generation complete
- `first` — first track complete (streaming URL available, ~30-40s)
- `complete` — all tracks complete (download URLs available, ~2-3 min)

**Callback payload structure:**

```json
{
  "code": 200,
  "msg": "All generated successfully.",
  "data": {
    "callbackType": "complete",
    "task_id": "xxx",
    "data": [
      {
        "id": "audio-id",
        "audio_url": "https://...",
        "stream_audio_url": "https://...",
        "image_url": "https://...",
        "prompt": "lyrics used",
        "model_name": "chirp-v4",
        "title": "Song Title",
        "tags": "pop, upbeat",
        "createTime": "2024-01-01T00:00:00.000Z",
        "duration": 198.44
      }
    ]
  }
}
```

If no callback URL is provided, poll the corresponding `record-info` endpoint using the `taskId`.

**Task status values:** `PENDING`, `TEXT_SUCCESS`, `FIRST_SUCCESS`, `SUCCESS`, `CREATE_TASK_FAILED`, `GENERATE_AUDIO_FAILED`, `CALLBACK_EXCEPTION`, `SENSITIVE_WORD_ERROR`

---

## API Endpoints

### 1. Generate Music

**POST** `/api/v1/generate`

Generates 2 songs per request.

**Parameters:**

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `customMode` | Yes | bool | `true` = provide lyrics/style/title; `false` = description-based |
| `instrumental` | Yes | bool | `true` = no vocals |
| `callBackUrl` | Yes | string | Webhook URL for results |
| `model` | Yes | string | `V4`, `V4_5`, `V4_5PLUS`, `V4_5ALL`, `V5` |
| `prompt` | Conditional | string | Lyrics (customMode=true) or description (customMode=false) |
| `style` | Conditional | string | Genre/style tags (customMode=true) |
| `title` | Conditional | string | Song title (customMode=true) |
| `personaId` | Optional | string | Persona ID for voice/style cloning |
| `personaModel` | Optional | string | `style_persona` or `voice_persona` |
| `negativeTags` | Optional | string | Styles to avoid |
| `vocalGender` | Optional | string | `m` or `f` |
| `styleWeight` | Optional | number | 0-1, style influence strength |
| `weirdnessConstraint` | Optional | number | 0-1, creativity level |
| `audioWeight` | Optional | number | 0-1, audio influence strength |

**Character limits:**
- `prompt`: V4=3000, V4_5+=5000, non-custom=500
- `style`: V4=200, V4_5+=1000
- `title`: V4/V4_5ALL=80, others=100

**Example — Custom mode with lyrics:**

```json
POST /api/v1/generate
{
  "customMode": true,
  "instrumental": false,
  "prompt": "[Verse]\nWalking through the city lights\nEverything feels right tonight\n\n[Chorus]\nWe're alive, we're on fire\nNothing's gonna stop us now",
  "style": "indie pop, upbeat, dreamy",
  "title": "City Lights",
  "model": "V4_5",
  "callBackUrl": "https://your-server.com/callback"
}
```

**Example — Description mode (simple):**

```json
POST /api/v1/generate
{
  "customMode": false,
  "instrumental": false,
  "prompt": "A happy upbeat pop song about summer vacation",
  "model": "V4_5",
  "callBackUrl": "https://your-server.com/callback"
}
```

---

### 2. Extend Music

**POST** `/api/v1/generate/extend`

Extends an existing generated song. Model must match the source audio version.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `defaultParamFlag` | Yes | bool | `true` = use original params; `false` = provide new params |
| `audioId` | Yes | string | ID of audio to extend |
| `callBackUrl` | Yes | string | Webhook URL |
| `model` | Yes | string | Must match source audio model |
| `prompt` | Conditional | string | New lyrics (when defaultParamFlag=false) |
| `style` | Conditional | string | New style |
| `title` | Conditional | string | New title |
| `continueAt` | Conditional | number | Seconds to continue from |
| `personaId` | Optional | string | Persona ID |
| `personaModel` | Optional | string | `style_persona` or `voice_persona` |
| `negativeTags` | Optional | string | Styles to avoid |
| `vocalGender` | Optional | string | `m` or `f` |

**Example:**

```json
POST /api/v1/generate/extend
{
  "defaultParamFlag": false,
  "audioId": "abc123",
  "prompt": "[Bridge]\nBut the night is young\nAnd so are we",
  "style": "indie pop, dreamy",
  "title": "City Lights (Extended)",
  "continueAt": 120,
  "model": "V4_5",
  "callBackUrl": "https://your-server.com/callback"
}
```

---

### 3. Upload and Cover Audio

**POST** `/api/v1/generate/upload-cover`

Creates an AI cover of uploaded audio.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `uploadUrl` | Yes | string | URL of source audio (max 8min, 1min for V4_5ALL) |
| `customMode` | Yes | bool | Custom or description mode |
| `instrumental` | Yes | bool | Instrumental only |
| `callBackUrl` | Yes | string | Webhook URL |
| `model` | Yes | string | Model version |
| `prompt` | Conditional | string | Lyrics or description |
| `style` | Conditional | string | Genre/style |
| `title` | Conditional | string | Song title |

Same optional parameters as Generate Music.

---

### 4. Upload and Extend Audio

**POST** `/api/v1/generate/upload-extend`

Extends uploaded audio with AI continuation.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `uploadUrl` | Yes | string | URL of source audio |
| `defaultParamFlag` | Yes | bool | Use defaults or custom params |
| `callBackUrl` | Yes | string | Webhook URL |
| `model` | Yes | string | Model version |
| `prompt` | Conditional | string | Lyrics for extension |
| `style` | Conditional | string | Style tags |
| `title` | Conditional | string | Title |
| `continueAt` | Conditional | number | Continue from (seconds) |
| `instrumental` | Conditional | bool | Instrumental only |

---

### 5. Add Vocals

**POST** `/api/v1/generate/add-vocals`

Adds vocals to an instrumental track.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `uploadUrl` | Yes | string | URL of instrumental audio |
| `prompt` | Yes | string | Lyrics |
| `title` | Yes | string | Song title |
| `style` | Yes | string | Vocal style |
| `negativeTags` | Yes | string | Styles to avoid |
| `callBackUrl` | Yes | string | Webhook URL |
| `vocalGender` | Optional | string | `m` or `f` |
| `model` | Optional | string | `V4_5PLUS` (default) or `V5` |

---

### 6. Add Instrumental

**POST** `/api/v1/generate/add-instrumental`

Adds instrumental backing to a vocal track.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `uploadUrl` | Yes | string | URL of vocal audio |
| `title` | Yes | string | Title |
| `tags` | Yes | string | Instrument/style tags |
| `negativeTags` | Yes | string | Styles to avoid |
| `callBackUrl` | Yes | string | Webhook URL |
| `model` | Optional | string | `V4_5PLUS` (default) or `V5` |

---

### 7. Generate Mashup

**POST** `/api/v1/generate/mashup`

Combines exactly 2 audio tracks into a mashup.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `uploadUrlList` | Yes | array | Exactly 2 audio URLs |
| `customMode` | Yes | bool | Custom or description mode |
| `model` | Yes | string | Model version |
| `callBackUrl` | Yes | string | Webhook URL |

Same conditional/optional params as Generate Music.

---

### 8. Replace Section

**POST** `/api/v1/generate/replace-section`

Replaces a specific time range within an existing song.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Original task ID |
| `audioId` | Yes | string | Audio ID |
| `prompt` | Yes | string | New lyrics for section |
| `tags` | Yes | string | Style tags |
| `title` | Yes | string | Title |
| `infillStartS` | Yes | number | Start time (seconds) |
| `infillEndS` | Yes | number | End time (seconds) |
| `negativeTags` | Optional | string | Styles to avoid |
| `callBackUrl` | Optional | string | Webhook URL |

**Constraints:** Duration must be 6-60 seconds and not exceed 50% of original duration.

---

### 9. Generate Lyrics

**POST** `/api/v1/lyrics`

Generates multiple lyric variations from a prompt.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `prompt` | Yes | string | Description of desired lyrics (max 200 chars) |
| `callBackUrl` | Yes | string | Webhook URL |

**Poll results:** `GET /api/v1/lyrics/record-info?taskId=...`

---

### 10. Get Timestamped Lyrics

**POST** `/api/v1/generate/get-timestamped-lyrics`

Returns word-level timestamps for generated audio.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Task ID |
| `audioId` | Yes | string | Audio ID |

**Response includes:** `alignedWords[]` (word, startS, endS, success, palign), `waveformData[]`, `isStreamed`

---

### 11. Separate Vocals from Music

**POST** `/api/v1/vocal-removal/generate`

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Task ID |
| `audioId` | Yes | string | Audio ID |
| `type` | Yes | string | `separate_vocal` (10 credits) or `split_stem` (50 credits) |
| `callBackUrl` | Yes | string | Webhook URL |

- `separate_vocal`: Returns vocals + instrumental tracks
- `split_stem`: Returns up to 12 stems (vocals, backing_vocals, drums, bass, guitar, keyboard, strings, brass, woodwinds, percussion, synth, fx)

**Poll results:** `GET /api/v1/vocal-removal/record-info?taskId=...`

---

### 12. Convert to WAV

**POST** `/api/v1/wav/generate`

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Task ID |
| `audioId` | Yes | string | Audio ID |
| `callBackUrl` | Yes | string | Webhook URL |

Returns `audioWavUrl`. **Poll:** `GET /api/v1/wav/record-info?taskId=...`

---

### 13. Create Music Video

**POST** `/api/v1/mp4/generate`

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Task ID |
| `audioId` | Yes | string | Audio ID |
| `callBackUrl` | Yes | string | Webhook URL |
| `author` | Optional | string | Author name (max 50 chars) |
| `domainName` | Optional | string | Domain name (max 50 chars) |

Returns `video_url`. **Poll:** `GET /api/v1/mp4/record-info?taskId=...`

---

### 14. Generate Cover Art

**POST** `/api/v1/suno/cover/generate`

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Original music task ID |
| `callBackUrl` | Yes | string | Webhook URL |

Returns 2 different style images. Each task can only generate cover art once.

**Poll:** `GET /api/v1/suno/cover/record-info?taskId=...`

---

### 15. Boost Music Style

**POST** `/api/v1/style/generate`

Synchronous endpoint — returns immediately.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `content` | Yes | string | Style description to enhance |

**Response:**

```json
{
  "code": 200,
  "data": {
    "result": "enhanced style text",
    "creditsConsumed": 1,
    "creditsRemaining": 99
  }
}
```

---

### 16. Generate Persona

**POST** `/api/v1/generate/generate-persona`

Creates a reusable voice/style persona from existing audio.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Task ID of source audio |
| `audioId` | Yes | string | Audio ID |
| `name` | Yes | string | Persona name |
| `description` | Yes | string | Persona description |
| `vocalStart` | Optional | number | Start of vocal sample (default 0.0) |
| `vocalEnd` | Optional | number | End of vocal sample (default 30.0, range 10-30s) |
| `style` | Optional | string | Style tags |

Returns `personaId` for use in `personaId` parameter of generation endpoints.

---

### 17. Generate MIDI

**POST** `/api/v1/midi/generate`

Requires a completed vocal separation task first.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `taskId` | Yes | string | Task ID (from vocal separation) |
| `callBackUrl` | Yes | string | Webhook URL |
| `audioId` | Optional | string | Audio ID |

Returns MIDI data with instruments and notes (pitch, start, end, velocity).

**Poll:** `GET /api/v1/midi/record-info?taskId=...`

---

### 18. Generate Sounds

**POST** `/api/v1/generate/sounds`

Generate sound effects and loops.

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `prompt` | Yes | string | Sound description (max 500 chars) |
| `model` | Yes | string | Must be `V5` |
| `soundLoop` | Optional | bool | Create loopable sound |
| `soundTempo` | Optional | number | 1-300 BPM |
| `soundKey` | Optional | string | Musical key (e.g. `C minor`, `G major`, `Any`) |
| `grabLyrics` | Optional | bool | Include lyrics |
| `callBackUrl` | Optional | string | Webhook URL |

---

### 19. Get Remaining Credits

**GET** `/api/v1/generate/credit`

No parameters. Returns remaining credit count.

```json
{ "code": 200, "data": 500 }
```

---

## File Upload API

**Base URL:** `https://sunoapiorg.redpandaai.co`

Files are auto-deleted after 3 days. Uses the same Bearer token auth. Max file size: 100MB.

### Upload via URL

**POST** `/api/file-url-upload`

```json
{
  "fileUrl": "https://example.com/audio.mp3",
  "uploadPath": "my-project",
  "fileName": "custom-name.mp3"
}
```

### Upload via Stream

**POST** `/api/file-stream-upload` (multipart/form-data)

- `file`: binary file
- `uploadPath`: string
- `fileName`: optional string

### Upload via Base64

**POST** `/api/file-base64-upload`

```json
{
  "base64Data": "base64-encoded-content",
  "uploadPath": "my-project",
  "fileName": "audio.mp3"
}
```

Recommended max 10MB (base64 adds ~33% size overhead).

**Upload response:**

```json
{
  "fileName": "audio.mp3",
  "filePath": "/my-project/audio.mp3",
  "downloadUrl": "https://...",
  "fileSize": 1234567,
  "mimeType": "audio/mpeg",
  "uploadedAt": "2024-01-01T00:00:00.000Z"
}
```

---

## Common Workflow Patterns

### Full Song Creation Pipeline

1. **Generate lyrics** → `/api/v1/lyrics`
2. **Generate music** with lyrics → `/api/v1/generate` (customMode=true)
3. **Extend** the song → `/api/v1/generate/extend`
4. **Generate cover art** → `/api/v1/suno/cover/generate`
5. **Create music video** → `/api/v1/mp4/generate`
6. **Convert to WAV** → `/api/v1/wav/generate` (for production use)

### Audio Processing Pipeline

1. **Upload audio** → File Upload API
2. **Separate vocals** → `/api/v1/vocal-removal/generate`
3. **Generate MIDI** → `/api/v1/midi/generate` (requires vocal separation first)
4. **Add new vocals** → `/api/v1/generate/add-vocals`
5. **Add new instrumental** → `/api/v1/generate/add-instrumental`

### Persona-Based Generation

1. **Generate initial song** → `/api/v1/generate`
2. **Create persona** from result → `/api/v1/generate/generate-persona`
3. **Generate new songs** with persona → `/api/v1/generate` with `personaId` + `personaModel`

---

## Best Practices

1. **Always provide a `callBackUrl`** — polling is less efficient and adds latency.
2. **Match model versions** when extending audio — extension model must match the source.
3. **Use structured lyrics format** with section markers: `[Verse]`, `[Chorus]`, `[Bridge]`, `[Outro]`, etc.
4. **Check credits** before batch operations using `GET /api/v1/generate/credit`.
5. **Download generated files promptly** — they expire after 14-15 days.
6. **Use the Style Boost endpoint** to refine style descriptions before generation.
7. **Respect rate limits** — 20 requests per 10 seconds max. Implement exponential backoff on 430 errors.
8. **Use personas** for consistent voice/style across multiple songs.
9. **For production audio**, convert to WAV format for highest quality.
10. **Replace Section** is ideal for fixing specific parts without regenerating the whole song — duration must be 6-60s and ≤50% of original.

## Resources

- **API Documentation:** https://docs.sunoapi.org/
- **API Keys:** https://sunoapi.org/api-key
- **Base URL:** `https://api.sunoapi.org`
- **File Upload URL:** `https://sunoapiorg.redpandaai.co`
