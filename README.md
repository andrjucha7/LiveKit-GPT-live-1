# LiveKit GPT Live Voice Agent

A voice AI agent built with [LiveKit Agents for Python](https://github.com/livekit/agents). It uses OpenAI's realtime **`gpt-live-1`** model (voice `marin`) through the LiveKit OpenAI plugin, and ai-coustics audio enhancement to clean up background noise. It also has an **End Call** tool, so the agent can hang up by itself.

All the project code is in the [`my-agent/`](my-agent/) folder.

---

## Prerequisites

Install these before you start:

| Tool | Version | What it's for | Install |
|------|---------|---------------|---------|
| **Python** | 3.10 – 3.14 | Runs the agent | [python.org/downloads](https://www.python.org/downloads/) or `brew install python@3.14` |
| **uv** | latest | Python package and project manager (installs dependencies, runs the agent and tests) | macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh \| sh` · Windows: `powershell -c "irm https://astral.sh/uv/install.ps1 \| iex"` · Homebrew: `brew install uv` |
| **LiveKit CLI (`lk`)** | 2.15.0 or newer | Log in to LiveKit Cloud, write the env file, deploy, run simulations, browse docs | macOS: `brew install livekit-cli` · Linux: `curl -sSL https://get.livekit.io/cli \| bash` · Windows: `winget install LiveKit.LiveKitCLI` |
| **Git** | any | Clone the repository | [git-scm.com](https://git-scm.com/) |
| **Docker** *(optional)* | any | Build the production image yourself | [docker.com](https://www.docker.com/get-started/) |

Check what you have installed:

```bash
python3 --version
uv --version
lk --version
```

### Accounts and keys

- **LiveKit Cloud account.** Sign up for free at [cloud.livekit.io](https://cloud.livekit.io/).
- **OpenAI API key** wi-1` realtime model. Getone at [platform.openai.com](https://platform.openai.com/api-keys).

---

## Setup

### 1. Clone the repository

```bash
git clone https://githuPT-live-1.git
cd LiveKit-GPT-live-1/my-agent
```

### 2. Install dependen

```bash
uv sync
```

This creates a `.venv` folder and installs everything listed in `pyproject.toml` and `uv.lock`.

### 3. Set up environment variables

Log in to LiveKit Cloud and write your project credentials to
`.env.local`:

```bash
lk cloud auth
lk app env --write --destination .env.local
```

Then open `.env.local` and add your OpenAI key. The file should look like this:

```dotenv
LIVEKIT_URL=wss://<your-project>.livekit.cloud
LIVEKIT_API_KEY=<your-l
LIVEKIT_API_SECRET=<your-livekit-api-secret>
OPENAI_API_KEY=<your-openai-api-key>
```

> **Never commit `.env.ted in `.gitignore`.

### 4. Download model f

```bash
uv run python src/agent
```

### 5. Add your prompt

Open [`my-agent/src/age.py) and replace theplaceholder text in the `instructions` of the `Assistant` class with your
own system prompt.

---

## Running the agent

Run all commands from inside the `my-agent/` folder.

| Mode | Command | Use
|------|---------|------------|
| **Console** | `uv runle` | Talk to the agentright in your terminal, using your microphone and speakers |
| **Dev** | `uv run pytnnect to LiveKit Cloud(with hot reload) to use a frontend or phone calls |
| **Production** | `uv run python src/agent.py start` | Run in production
|

The agent registers as **`my-agent`** (`agent_name` in `src/agent.py`).
When you dispatch the aSIP/telephony rule, usethis name.

### Talk to it from a frontend

While the agent runs inct to it with:

- The **Agents Playgroudashboard](https://cloud.livekit.io/)
- A starter app, for ex](https://github.com/livekit-examples/agent-starter-react) (web), [agent-starter-swift](https://github.com/livekit-examples/agent-starter-swift) (iOS/macOS) or [agent-starter-android](https://github.com/livekit-examples/agent-starter-android)
- **Phone calls**, by following the [telephony
guide](https://docs.liv

---

## Testing

### Unit tests (pytest)

```bash
uv run pytest
```

The tests are in `my-agent/tests/test_agent.py`. The example in that file
is commented out; see tdocs](https://docs.livekit.io/agents/start/testing/).

### Simulations (LiveKi

Simulations run full multi-turn conversations between a simulated user and your agent, then judge each transcript. The scenarios are in `my-agent/scenarios.yaml`.

```bash
lk agent simulate --sce
```

> After you change the yaml` so the expectations match what your agent should do.

### Linting and formatting

```bash
uv run ruff format
uv run ruff check
```

---

## Deployment

### LiveKit Cloud

The project is already agent in`my-agent/livekit.toml`. Deploy a new version with:

```bash
lk agent deploy
```

Add your secrets (such as `OPENAI_API_KEY`) to the agent in the LiveKit Cloud dashboard or with `lk agent update-secrets`. For details, see the [deployment guide](https://docs.livekit.io/deploy/agents/).

### Docker

```bash
docker build -t my-agen
docker run --env-file .env.local my-agent
```

---

## GitHub Actions (CI)

The workflows are in `my-agent/.github/workflows/`:

- **ruff.yml**: checks formatting and linting
- **simulations.yml**: very push to `main` andon demand
- **template-check.yml*mplate maintenance

> GitHub only runs workflows from a `.github/workflows/` folder at the
**repository root**. Tot/.github` to the root of the repo and set the `working-directory` in the workflows to `my-agent`.

For the simulations to ry secrets** (Settings →Secrets and variables → Actions):

- `LIVEKIT_URL`
- `LIVEKIT_API_KEY`
- `LIVEKIT_API_SECRET`

---

## Project structure

```
LiveKit-GPT-live-1/
├── README.md                ← this file
└── my-agent/
    ├── src/
    │   └── agent.py   (prompt, model, voice,tools)
    ├── tests/
    │   └── test_agent.py    ← pytest tests
    ├── scenarios.yaml os
    ├── pyproject.toml       ← dependencies and tool config
    ├── uv.lock              ← locked dependency versions
    ├── livekit.toml   ect and agent ID
    ├── Dockerfile           ← production container
    ├── AGENTS.md            ← guide for coding agents (Claude Code, Cursor, Codex)
    └── .github/workflo
```

## Customization

Everything you'd usually change is in `src/agent.py`:

- **Prompt**: `instructions` in the `Assistant` class
- **Voice**: `voice="marin"` in `GPTLiveModel(...)`
- **Model**: `model="gp
- **Tools**: the `tools=[EndCallTool()]` list. Add your own `@function_tool` methods here.
- **Noise cancellation*ncement(...)` in`RoomOptions`

## Troubleshooting

- **`lk docs` or `lk ag*: update the LiveKit CLI to 2.15.0 or newer.
- **Authentication erro `.env.local` exists in`my-agent/` and contains all four variables.
- **Dependency install using Python 3.10–3.14,then run `uv sync` again.

## Useful links

- [LiveKit Agents docs]gents/)
- [LiveKit CLI](https://docs.livekit.io/intro/basics/cli/)
- [OpenAI realtime plugin](https://docs.livekit.io/agents/models/realtime/)
- [uv documentation](https://docs.astral.sh/uv/)

## License

MIT

Two things to check before you post it:
- OpenAI key: the READMAPI_KEY in .env.local,because the agent calls the OpenAI plugin directly. If the agent runs fine without one on your machine, remove that line and the "Accounts and keys" bullet.
- License: it ends with "MIT", but the repo has no LICENSE file. Add one
  or delete that sectio
