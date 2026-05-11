# QikLM

QikLM is a local AI control layer for people who want the speed, privacy, and flexibility of running their own models without living in shell scripts.

It sits in front of model servers such as `llama.cpp`, gives you a browser-based dashboard, exposes OpenAI-compatible and Anthropic-compatible API routes, and helps desktop apps and coding tools connect to the local model you actually want to use.

QikLM does not replace `llama.cpp`. It makes `llama.cpp` easier to operate, easier to share with other apps, and easier to upgrade independently. If you already run `llama-server` directly, QikLM gives you a cleaner way to manage models, profiles, launch settings, logs, API keys, integrations, and a built-in chat workspace from one place.

## Why QikLM Exists

Many AI apps embed or assume a specific engine build. That is convenient until you want a newer `llama.cpp` feature, a different launch flag, a remote server, a bigger context setting, a vision projector, or a model that the app has not caught up with yet.

QikLM solves that by separating the model runtime from the apps that use it:

- Bring your own `llama-server` binary and update it when you want.
- Save model-specific launch profiles instead of rebuilding long commands by hand.
- Point tools at one stable local endpoint while changing the model behind it.
- Use local GGUF models from a managed library or connect to a remote `llama.cpp` endpoint.
- Keep privacy-sensitive local workflows local, while still supporting LAN and remote access when you choose to expose it.
- Give tools such as Claude Code, Codex, Hermes Agent, OpenCode, and OpenClaw a consistent way to reach your selected local profile.

## Main Surfaces

### Admin Dashboard

The admin dashboard is the control center for QikLM. It is available at `/admin/` after the server starts.

Use it to:

- Run first-time setup and choose where QikLM stores models and settings.
- Start, stop, and monitor the active engine.
- Create saved model profiles with per-model runtime settings.
- Browse and manage a local GGUF library.
- Search Hugging Face GGUF repositories and download models into your library.
- Import models from a remote `llama.cpp` endpoint.
- Preview the effective launch command before running a profile.
- Manage API access, LAN/remote binding, admin authentication, account lock behavior, and logs.
- Configure app integrations from one place.

### PromptUI Chat GUI

PromptUI is a built-in premium chat GUI and workspace for QikLM. It supports
persistent chats, projects, model selection, runtime settings, image
attachments, optional web context through Brave Search, and user-selected HTTP
MCP context resources.

It is also meant to be useful while you are working, not just a place to send
messages. The Canvas can open code blocks from a chat, let you edit them inline,
preview HTML pages, copy or save the result, and send the edited code back into
the conversation for another pass. With Brave Search enabled, the selected local
model can answer with fresh web context in manual, automatic, or always-on
search modes. PromptUI also keeps chat history searchable and organized by
project, so different model experiments and work threads can stay separate.

### Compatibility API

QikLM exposes API routes that common AI clients already understand:

- `/v1/models`
- `/v1/chat/completions`
- `/v1/completions`
- `/v1/responses`
- `/v1/messages`

That lets QikLM behave as the stable local endpoint while the selected model profile, runtime process, and integration-specific routing stay under your control.

## Integrations

QikLM includes first-class setup flows for common local AI tools:

- **Claude Code**: uses QikLM's Anthropic-compatible route and prepares the needed VS Code environment values.
- **Codex**: writes a dedicated QikLM provider/profile so the Codex VSCode extension and CLI can run with `codex --profile qiklm`.
- **Hermes Agent**: writes a dedicated custom provider and model profile so Hermes Agent can use your selected QikLM profile.
- **OpenCode**: adds QikLM models to OpenCode without taking over unrelated settings.
- **OpenClaw**: adds QikLM as a dedicated provider while preserving existing models and custom providers.

The goal is not to trap you in one client. QikLM is meant to be the local model hub that multiple apps can share.

## Model And Runtime Flexibility

QikLM's first-class GUI workflow is centered on `llama.cpp` and GGUF models. You can use it with:

- A managed local `llama-server` executable.
- A remote `llama.cpp` server.
- Local GGUF files discovered from your model library.
- Hugging Face GGUF downloads.
- Saved profiles for different models, hardware settings, API behavior, and launch flags.

This is the main advantage over apps that bundle a fixed engine. QikLM lets the engine evolve independently. When `llama.cpp` adds a feature you care about, you can update your engine and expose the new behavior through QikLM instead of waiting for every downstream app to ship support.

## Managing llama.cpp Builds

In QikLM, a build is the local `llama-server` program that actually runs a
model. Think of it like the engine in a car: the model is what you drive, but
the build is the thing that makes it move.

You can keep up to five different `llama.cpp` builds registered at the same
time. This is useful when one build is your dependable daily driver, another
has a newer feature you want to test, and another is tuned for a different GPU
or machine setup. External apps still talk to QikLM the same way, so you can
try a newer engine without redoing every client configuration.

Use the admin dashboard to manage builds:

1. Open `/admin/` and go to Engine Settings.
2. In the `llama.cpp Builds` section, choose `Install Build` if you want QikLM
   to download a release for you.
3. Choose the release and archive that matches your computer.
4. Start the install and let QikLM download, unpack, find `llama-server`, and
   register it as a build.
5. Pick which registered build should be the default, then save Engine
   Settings.

If you already have a `llama-server` binary somewhere on disk, use `Add Build`
instead. Give the build a name you will recognize, choose the executable path,
and optionally add notes or an environment command. This only tells QikLM where
the build lives; it does not move or replace your existing files.

The default build is what QikLM uses when a profile does not choose a specific
build. You can also pick a build inside an individual local `llama.cpp` profile.
That means a small fast model can use your stable build, while a newer model can
use a newer build with the feature it needs. Remote `llama.cpp` profiles do not
use local builds, because the model is already being served somewhere else.

When you delete a build entry, QikLM only removes the registration from its
settings. It does not delete the executable from disk. QikLM will also stop you
from deleting the default build or a build that is still used by a profile, so
you do not accidentally break a setup you still rely on.

## How Profiles Work

A profile is a saved way to run or reach a model. Instead of remembering a long
command, a file path, a context size, GPU settings, and special flags, you give
that setup a name and let QikLM keep the details.

For example, you might have one profile called `fast-chat` for quick everyday
questions, another called `coding-large` for slower but stronger coding work,
and another called `vision` that includes a vision projector file. Apps such as
PromptUI, Claude Code, Codex, Hermes Agent, OpenCode, and OpenClaw can ask for
the profile by name through QikLM's stable API endpoint.

Profiles can describe different kinds of model sources:

- A local GGUF file in your QikLM model library.
- A remote `llama.cpp` server that is already running elsewhere.

For local `llama.cpp` profiles, the profile can also choose which registered
build to use. If the profile leaves the build set to `Use engine default`, QikLM
uses the default build from Engine Settings. If the profile chooses a specific
build, QikLM uses that one for the profile instead.

Profiles also control the practical launch choices for that model: context
size, GPU layers, threads, batch size, cache choices, memory mapping, reasoning
mode, extra flags, optional environment setup, and whether blank runtime fields
inherit the global defaults. In plain terms, global defaults are the house
settings, and a profile can either borrow those settings or carry its own.

When a client asks QikLM for a model, QikLM looks up the matching profile. If
the profile is local, QikLM starts or switches the managed engine as needed,
builds the launch command from the profile and engine settings, waits for the
engine to be ready, and then forwards the request. If the profile points to a
remote engine, QikLM routes the request to that remote endpoint instead.

## Typical Workflows

### Local GGUF Daily Driver

1. Install or add a `llama.cpp` build and choose your model directory.
2. Scan your local GGUF library.
3. Create profiles for your preferred chat, coding, reasoning, and lightweight models.
4. Start the active profile from the dashboard.
5. Use Prompt, Claude Code, Codex, or another OpenAI-compatible client against QikLM.

### Keep Apps Stable While Upgrading The Engine

1. Configure external apps to call QikLM's stable `/v1` endpoint.
2. Upgrade or replace your `llama-server` binary whenever you need newer engine behavior.
3. Adjust profile flags in QikLM.
4. Keep client configuration unchanged.

### Remote Or LAN Model Host

1. Run QikLM on the machine with the model runtime.
2. Choose local-only, LAN, or custom bind settings.
3. Require bearer auth for public API access when exposing QikLM beyond localhost.
4. Connect clients from another machine using the QikLM base URL and API key.

## Getting Started

Requirements:

- A `llama-server` executable from `llama.cpp`
- GGUF models, or access to GGUF models you want to download

Run QikLM:

```bash
qiklm
```

By default QikLM listens on port `8192`.

Open:

- Admin dashboard: `http://localhost:8192/admin/`
- PromptUI: `http://localhost:8192/prompt/`
- API base URL: `http://localhost:8192/v1`

## Configuration And Data

QikLM uses platform-standard per-user directories by default. Run `qiklm paths`
to see the exact paths for the current machine and environment.

On Linux:

- Config: `~/.config/qiklm/`
- Data: `~/.local/share/qiklm/`
- Logs and state: `~/.local/state/qiklm/`

Linux package/systemd installs set `QIKLM_HOME=/var/lib/qiklm`, so package
data lives under:

- Config: `/var/lib/qiklm/config/`
- Data: `/var/lib/qiklm/data/`
- Logs and state: `/var/lib/qiklm/state/`

On macOS:

- Config: `~/Library/Application Support/qiklm/`
- Data: `~/Library/Application Support/qiklm/data/`
- Logs and state: `~/Library/Logs/qiklm/`

On Windows:

- Config: `%AppData%\qiklm\`
- Data: `%LocalAppData%\qiklm\`
- Logs and state: `%LocalAppData%\qiklm\state\`

Set `QIKLM_HOME` to keep config, data, and state under one custom root.

Useful environment variables:

- `QIKLM_HOME`: custom QikLM home directory.
- `QIKLM_PROXY_PORT`: server port, default `8192`.
- `QIKLM_PROXY_BIND_MODE` or `QIKLM_BIND_MODE`: `local_only`, `lan`, or `custom`.
- `QIKLM_PROXY_BIND_ADDRESS` or `QIKLM_BIND_ADDRESS`: bind address when using custom binding.
- `QIKLM_LOG_LEVEL`: logging verbosity.

Example remote/LAN-style launch:

```bash
QIKLM_BIND_MODE=custom QIKLM_BIND_ADDRESS=0.0.0.0 QIKLM_PROXY_PORT=8192 qiklm
```

## Offline Account Recovery

If a local account password is lost, stop QikLM first, then reset the password
from the machine that owns the QikLM database. The reset command updates the
local password hash and revokes existing sessions.

Check the active paths:

```bash
qiklm paths
```

Linux package install:

```bash
sudo qiklm account reset-password --home /var/lib/qiklm --username admin
```

Linux or macOS direct binary:

```bash
qiklm account reset-password --username admin
```

Windows PowerShell:

```powershell
.\qiklm.exe account reset-password --username admin
```

Custom database path:

```bash
qiklm account reset-password --db /path/to/qiklm.db --username admin
```

For scripted recovery, pass the new password on stdin:

```bash
printf '%s\n' 'new-strong-password' | qiklm account reset-password --username admin --password-stdin
```

## Security Model

QikLM is local-first. The default bind mode is localhost-only.

When you expose QikLM on a LAN or remote host, use the admin account and public API access controls. The dashboard can show or regenerate the shared API key used by compatible clients and integrations.

