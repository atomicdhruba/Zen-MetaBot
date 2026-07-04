<div align="center">

  <h1>ZenXYT</h1>
  
  <p>
    <strong>Automated Metadata Optimization Engine for YouTube</strong><br>
    <em>Powered by Concurrent LLM Analysis (Gemini 1.5 Pro & NVIDIA Nemotron)</em>
  </p>

  <p>
    <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/python-3.10+-blue.svg" alt="Python 3.10+"></a>
    <a href="https://github.com/sponsors/atomicdhruba"><img src="https://img.shields.io/badge/sponsor-30363D?style=flat&logo=GitHub-Sponsors&logoColor=#EA4AAA" alt="Sponsor"></a>
    <a href="https://ko-fi.com/atomicdhruba"><img src="https://img.shields.io/badge/Ko--fi-F16061?style=flat&logo=ko-fi&logoColor=white" alt="Ko-fi"></a>
    <a href="https://patreon.com/c/AtomicDhruba"><img src="https://img.shields.io/badge/Patreon-F96854?style=flat&logo=patreon&logoColor=white" alt="Patreon"></a>
  </p>

</div>

---

> ZenXYT operates as a local daemon or desktop app. The pipeline is designed around a "Debate Engine" pattern where two distinct models evaluate the same video context independently before a final judge prompt synthesizes the optimal metadata payload.

## Core Architecture

The system utilizes a multi-agent pipeline to process raw video files into highly optimized YouTube metadata.

```text
 [ RAW VIDEO ] ───▶ ( yt-dlp ) ───▶ [ MULTIMODAL PARSING ]
                                             │
                                             ▼
                                   [ brain.txt CACHE ]
                                             │
                ┌────────────────────────────┼────────────────────────────┐
                ▼                            ▼                            ▼
       [ NVIDIA MODE ]                [ DEBATE MODE ]              [ GEMINI MODE ]
    (Nemotron Inference)        (Concurrent Multi-Agent)         (Gemini Inference)
                │                            │                            │
                └────────────────────────────┼────────────────────────────┘
                                             ▼
                                 [ SYNTHESIS & SEO GRADING ]
                                             │
                                             ▼
                                   [ YOUTUBE DATA API ]
                                             │
                                             ▼
                                   [ LIVE VIDEO UPDATE ]
```

## Technical Capabilities

| System Component | Description |
| :--- | :--- |
| **Vision Extraction** | Frame-by-frame multimodal parsing using Gemini to build a persistent context cache (`brain.txt`). |
| **Debate Pipeline** | Multi-agent drafting where NVIDIA and Gemini generate competing metadata, followed by synthesis evaluation. |
| **Execution Modes** | Toggle between isolated models (`nvidia`, `gemini`) or the concurrent `debate` synthesizer via the UI. |
| **Heuristic Scoring** | Built-in SEO grading algorithm (0–100) evaluating title CTR potential, tag relevance, and algorithmic hook. |
| **Asynchronous UI** | CustomTkinter dark-mode desktop interface running decoupled from the background API blocking calls. |

<br>

<details>
<summary><strong>[ Expand ] Project Structure</strong></summary>

```text
.
├── bot.py                  # Main entry point (GUI or CLI daemon)
├── requirements.txt        # Dependency locks
├── .env.example            # Template for required environment variables
│
├── zenxyt/                 # Core Package
│   ├── youtube.py          # YouTube Data API v3 integration layer
│   ├── downloader.py       # Audio/Video multiplexer via yt-dlp
│   ├── brain.py            # Multimodal extraction interface
│   ├── ai_nvidia.py        # NVIDIA Nemotron inference client
│   ├── ai_gemini.py        # Gemini inference client
│   ├── debate.py           # Multi-agent synthesis and SEO grading
│   ├── orchestrator.py     # Central pipeline controller
│   ├── gui.py              # CustomTkinter interface rendering
│   └── ...                 # Utilities & Configs
│
└── brain/                  # Local cache directory for extracted context
```
</details>

---

## Setup & Initialization

To run this project locally, provision external API keys for the inference models and Google services.

- **Environment:** `Python 3.10+`
- **System Binaries:** `FFmpeg` in your system PATH.
- **Google Cloud:** A GCP project with the YouTube Data API v3 enabled.
- **NVIDIA Developer:** An API key for Nemotron inference.
- **Google AI Studio:** A Gemini API key for the multimodal extraction.

### Quick Start

```bash
git clone https://github.com/atomicdhruba/ZenXYT.git
cd ZenXYT
pip install -r requirements.txt
cp .env.example .env
```

Populate `.env` with your API keys:
```ini
NVIDIA_API_KEY=your_key_here
GEMINI_API_KEY=your_key_here
GENERATION_MODE=debate
```

<br>

<details>
<summary><strong>[ Expand ] YouTube OAuth Configuration (BYOK)</strong></summary>

<br>

To avoid standard Google App Verification overhead, this repository uses a "Bring Your Own Key" (BYOK) model for YouTube authentication. Your tokens remain strictly local.

1. Create a project in the <kbd>Google Cloud Console</kbd>.
2. Navigate to **APIs & Services > Library**, search for **YouTube Data API v3**, and click **Enable**.
3. Configure the **OAuth Consent Screen**:
   - Set User Type to **External**.
   - Under **Scopes**, add `.../auth/youtube.force-ssl`.
   - Under **Test users**, add the email address linked to your YouTube channel.
4. Navigate to **Credentials** > **Create Credentials** > **OAuth client ID** (Choose **Desktop app**).
5. Download the JSON credential file, rename it to `client_secrets.json`, and place it in the root of the repository.

*On initial execution, a local server will spawn to handle the OAuth callback, generating a `token.pickle` file for subsequent headless runs.*
</details>

---

## Execution

Initialize the desktop GUI dashboard:
```bash
python bot.py
```

Execute in headless CLI mode for server environments:
```bash
python bot.py --cli
```

---

## Security Context

**Never commit your secrets.**  
The provided `.gitignore` explicitly blocks `.env`, `client_secrets.json`, and `token.pickle`. Do not override these rules. Leaking `token.pickle` or `client_secrets.json` compromises your YouTube channel.

---

## Support the Project

Building and maintaining AI automation tools requires significant architectural time and API resource allocation. If ZenXYT optimizes your workflow, consider supporting the development.

<div align="center">
  <a href="https://ko-fi.com/atomicdhruba"><img src="https://ko-fi.com/img/githubbutton_sm.svg" width="400" alt="Buy Me a Coffee at ko-fi.com" /></a><br>
  <a href="https://patreon.com/c/AtomicDhruba"><img src="https://img.shields.io/badge/BECOME_A_PATRON-000000?style=for-the-badge&logo=patreon&logoColor=white" width="400" alt="Become a Patron" /></a>
</div>
