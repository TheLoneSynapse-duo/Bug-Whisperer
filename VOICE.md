# Voice mode — talk to mistake-finder (optional)

> **Note:** Voice mode is an optional feature that requires IBM Watson Speech to
> Text and Text to Speech services. The core tool (CLI + pytest plugin + AI-agent
> skill) works perfectly without it. If you do not need voice mode, you can
> ignore this file entirely.

Say *"find the mistake in demo math"* and you get the same boxed report
`mistake-box` prints, on the file you meant, read back to you out loud.

```
you (into the mic): find the mistake in demo json
   file: demos\demo_json.py  (you said 'demo json')
   ╔══════════════════════════════════════════════════════════════════╗
   ║ MISTAKE FOUND - JSONDecodeError                                  ║
   ╠══════════════════════════════════════════════════════════════════╣
   ║ WHERE                                                            ║
   ║   demos\demo_json.py:11                                          ║
   ║ >>  11 | data = json.loads("{'name': 'Alice', 'age': 25}")      ║
   ╠══════════════════════════════════════════════════════════════════╣
   ║ WHAT WENT WRONG                                                  ║
   ║   JSONDecodeError: Expecting property name enclosed in ...       ║
   ╚══════════════════════════════════════════════════════════════════╝
speaker: "JSONDecodeError in demo_json.py, line 11. Print the raw payload
          before parsing; it is probably empty or an error page, not JSON."
```

## How to activate it

```bash
voice.bat                 # interactive push-to-talk loop
```

Then, at the `>` prompt:

- **Press Enter with nothing typed** → it starts recording. Speak, then press
  Enter again to stop. (It stops on its own after `VOICE_MAX_SECONDS`.)
- **Or type a command** instead of speaking — same pipeline, no mic:
  `find the mistake in mycode` — handy for testing, and it works over SSH.
- **`q`** to quit.

Ctrl+C cancels a recording in progress.

### One-shot commands

```bash
voice.bat --text "find the mistake in demo math"   # no mic, scriptable
voice.bat --file demos\demo_json.py                # analyse one file now
voice.bat --set-file mycode.py                     # pin "the file I'm working on"
voice.bat --list-mics                              # what microphones exist
voice.bat --once                                   # one turn, then exit
voice.bat --no-speak                               # print the box, stay silent
voice.bat --ascii                                  # plain +-| box
```

## Which file does it check?

Three ways, in order:

1. **A file you name out loud** — resolved against every `.py` file in the
   workspace, ignoring `.venv`, `.bob`, `__pycache__`, etc. It understands
   spoken punctuation, so *"demo underscore math"*, *"demo math"* and
   `"demo_math.py"` all land on `demos/demo_math.py`. It tells you which file
   it chose and why (`you said 'demo math'`), so a wrong guess is obvious.
2. **The pinned file** — `.voice_current`, set by `--set-file`, or automatically
   after every successful lookup. This is the closest thing to "the file I have
   open".
3. **The most recently modified `.py`** in the workspace — the file you were
   just editing. The agent's own source is never picked.

> The agent cannot read your editor's active tab directly — that needs an
> extension. The pin task is the honest, portable substitute.

## What happens on each turn

```
mic ──ffmpeg──▶ WAV ──Speech to Text──▶ transcript
                                            │
                             file recognition (or pinned / most recent)
                                            │
                               run the file, parse the traceback
                                            │
                          box printed  +  one-line summary spoken by Text to Speech
```

- **A find request** (`find` / `check` / `mistake` / `bug` / `error` …) runs the
  analysis and prints the box.
- **A question** (*"why does a KeyError happen in pandas"*) is answered by an
  AI model and spoken aloud.

## Setup (optional — only if you use voice mode)

Voice mode requires:

1. **IBM Watson Speech to Text** — for converting your speech to text.
   Create an instance at https://cloud.ibm.com.
2. **IBM Watson Text to Speech** — for reading the results aloud.
   Create an instance at https://cloud.ibm.com.
3. **An IBM Cloud API key** — create one at https://cloud.ibm.com/iam/apikeys.
4. **ffmpeg with microphone support** — for recording your voice.
5. **A microphone** — obviously.

Copy `.env.example` to `.env` and fill in the values:

```bash
copy .env.example .env        # Windows Command Prompt
cp .env.example .env          # Mac/Linux
```

Then edit `.env` and uncomment the values you need.

| Variable | What it is | Required? |
|---|---|---|
| `IBM_API_KEY` | IBM Cloud IAM API key | Yes, for voice mode |
| `IBM_STT_URL` | Watson Speech to Text instance URL | Yes, for voice mode |
| `IBM_TTS_URL` | Watson Text to Speech instance URL | Yes, for voice mode |
| `IBM_TTS_VOICE` | Voice name, e.g. `en-US_AllisonV3Voice` | No (has default) |
| `VOICE_MIC` | Microphone name (run `--list-mics` to see) | No (auto-picks) |
| `FFMPEG` | Path to ffmpeg with microphone support | No (auto-discovers) |

### Dependencies

Voice mode uses:
- `requests` — for API calls to IBM Watson (must be installed separately)
- `winsound` (stdlib) — for audio playback on Windows
- `ffmpeg` with microphone support — for recording audio

> **Note:** `requests` is not a dependency of the core tool. If you use voice
> mode, you need to install it separately: `pip install requests`.

The stock `ffmpeg.exe` bundled with some apps is built `--disable-indevs` and
**cannot record**. If voice mode says `no DirectShow-capable ffmpeg found`,
you need a full ffmpeg build. Set `FFMPEG=` in `.env` to point to it.

## Troubleshooting

| Symptom | Fix |
|---|---|
| `no DirectShow-capable ffmpeg found` | Set `FFMPEG=` in `.env` to a normal ffmpeg build. |
| Wrong microphone used | `voice.bat --list-mics`, then copy the exact name into `VOICE_MIC`. |
| `IBM rejected the API key` | Key rotated or wrong — update `IBM_API_KEY` in `.env`. |
| Box says `NO MISTAKE - TIMED OUT` | The file blocks (waits for input / long work); raise `VOICE_RUN_TIMEOUT`. |
| Nothing heard | Speak within ~15 s of the recording start; check `VOICE_MAX_SECONDS`. |
| It picks the wrong file | Say the name more clearly, or `--set-file` to pin it. |
| `ModuleNotFoundError: No module named 'requests'` | Install requests: `pip install requests` |

## Security

`.env` is in `.gitignore` — never commit it or paste its key anywhere public.
If a key leaks, disable it on the IBM Cloud console and put the new one in
`.env`; the agent reads it fresh on the next run.

> **Remember:** Voice mode sends your voice to IBM Watson for transcription.
> If you have privacy concerns, use the CLI or pytest plugin instead, which
> run entirely on your computer with no network calls.
