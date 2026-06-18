<div align="center">

<img align="center" width="260" height="260" alt="qiklm-logo-128" src="https://github.com/user-attachments/assets/dbdfd717-feab-4165-90e9-98cf143968fa" />

# QikLM

### The qikest and easiest way to run local AI. A high-performance orchestration layer and web console for `llama.cpp` and `vLLM`.

**Free for home and work use. 100% private, local, and offline.**

<p>
  <img src="https://img.shields.io/badge/llama.cpp-Compatible-black?style=for-the-badge" alt="llama.cpp Compatible" />
  <img src="https://img.shields.io/badge/vLLM-Compatible-30A2FF?style=for-the-badge" alt="vLLM Compatible" />
  <img src="https://img.shields.io/badge/OpenAI_API-Compatible-412991?style=for-the-badge" alt="OpenAI API Compatible" />
  <img src="https://img.shields.io/badge/Anthropic_API-Compatible-d4a574?style=for-the-badge" alt="Anthropic API Compatible" />
  <img src="https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="Linux" />
  <img src="https://img.shields.io/badge/macOS-000000?style=for-the-badge&logo=apple&logoColor=white" alt="macOS" />
  <img src="https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="Windows" />

</p>

</div>

---

## What is QikLM?

> [!TIP]
> **Break free from the CLI.** Local AI doesn't have to be painful.

**QikLM** is a small, lightweight orchestration layer and web console for `llama.cpp` and `vLLM`. It manages your local model weights, handles engine lifecycle, and serves a full-featured web UI and premium chat workspace, all from a single binary with zero dependencies. No wrappers, no abstraction layers; just bare-metal access to your engine with a management layer on top.

Run `llama.cpp` or `vLLM`: use any build you want, or let QikLM install a managed `llama.cpp` release for you directly from Github. No lock-in, no bloat.

- **Privacy**: 100% local and offline. Zero cloud telemetry. Your models and data never leave your machine.
- **Orchestration**: Automatic model booting, VRAM management, lifecycle control, and real-time GPU monitoring (VRAM, temperature, power, load) via lmtop.
- **PromptUI**: A premium chat workspace with an interactive canvas, multimodal support, document handling (PDF, DOCX, Office), OCR for images and PDFs, Brave web search, workspace projects, and HTTP MCP integration.
- **Bring Your Own Binary**: Drop in your own `llama.cpp` or `vLLM` build, use a fork, or install up to 5 managed `llama.cpp` releases directly from the dashboard.
- **Network-Ready**: Access the full web UI from any pc or mobile device on your network. Already on Tailscale or a VPN? Enable LAN mode and you're done.
- **Convenience**: Studio-style ease of use with full engine flexibility and raw performance. No compromises.
- **Simplicity**: Get up and running in seconds with a [pre-compiled binary or native Linux package](#installation).

---

## What QikLM Is Not

- **Not** a standalone inference engine. It orchestrates your existing `llama.cpp` and `vLLM` installs.
- **Not** another bloated all-in-one desktop app. One binary, no Electron, no Docker, no Python.

---

## The Motivation: A Stateful Bridge

> [!IMPORTANT]
> **The Problem:** Inference servers like `llama-server` and `vLLM` are the gold standard for local AI performance, but they are inherently **stateless**. Every time you close a terminal, your engine flags and configurations vanish.
>
> **The Solution:** QikLM is the **stateful bridge**. It transforms raw CLI processes into a persistent, managed service that remembers exactly how every model should run, so you don't have to.

### The Friction
- **Manual Lifecycles**: Manually launching and killing processes to free up VRAM.
- **Configuration Fatigue**: Managing shell scripts for different GGUF engine allocations.
- **VRAM Zombies**: Leaving GPUs locked with inactive models because a terminal was left open.

### The Orchestration
QikLM sits between your inference engine and your applications, turning manual terminal work into a persistent background service.

- **Auto-Boot**: Send a prompt from PromptUI, any OpenAI-compatible frontend, or a coding harness, and QikLM automatically boots your engine (`llama-server` or `vLLM`) with the correct model and profile settings. No manual startup required.
- **Seamless Model Switching**: Switch models mid-session. QikLM handles swapping the background engine instance automatically.
- **Lifecycle Control**: Load and unload models with a single click from the dashboard.
- **Profile-Based Startups**: Save your flags once per model. Choose different `llama.cpp` builds per profile. Launch with a click.

---

## Features

- **Smart Model Profiles**: Save GPU layers, context size, threads, and custom CLI flags per model. Every profile keeps its own runtime recipe.
- **Reusable Profile Templates**: Create named presets for common launch settings and apply them when building new profiles.
- **Full CLI Passthrough**: The UI covers common flags, but every profile also has a raw CLI argument field for any `llama.cpp` or `vLLM` flag you need.
- **Multi-Binary Support**: Register up to 5 different `llama.cpp` builds, or point at a single `vLLM` install. Assign specific builds per model profile to test newer features or optimizations.
- **PromptUI Workspace**: Built-in chat GUI with an interactive canvas for live code editing and HTML previews, workspace projects, document handling (PDF, DOCX), image and PDF OCR, multimodal attachments, Brave web search, and MCP resource injection.
- **lmtop Telemetry**: Real-time GPU observability. Monitor VRAM, temperature, power, and load from the dashboard or PromptUI side panel.
- **Auto VRAM Unloading**: Automatically frees GPU memory after a configurable period of inactivity.
- **Model Library**: Automated file discovery, profile mapping, and one-click management for your entire weights collection.
- **Hugging Face Explorer**: Search the Hub, inspect quant file trees, and download models directly into your library.
- **Lightweight API Proxy**: Serves the OpenAI `/v1/models` endpoint so your profiles populate directly in any compatible client's model dropdown.
- **Cross-Platform**: Runs natively on Linux, macOS, and Windows.

---

## Integrations

One-click activation from the dashboard. QikLM generates the config and environment variables for each tool automatically.

- **Claude Code** (CLI & VS Code): Activate and QikLM sets up Anthropic-compatible API routing automatically.
- **Codex**: Activate and run `codex --profile qiklm` against any local model via the Responses API.
- **Hermes Agent**: Activate for instant provider and model routing through QikLM.
- **OpenCode**: Activate for zero-config model discovery with hot-swap profile support.

---

## Compatibility API

Any client supporting OpenAI or Anthropic standards can interface with QikLM's local endpoint at `http://localhost:8192`.

| Standard | Endpoints |
|---|---|
| **OpenAI** | `/v1/chat/completions`, `/v1/responses`, `/v1/models` (list available models) |
| **Anthropic** | `/v1/messages` |

---

## Architecture & Screenshots

### Control Center Overview

> <img width="1722" height="970" alt="Screenshot_20260616_023228" src="https://github.com/user-attachments/assets/1b2e2103-7d9a-44b7-b97f-965ab8be31b1" />

### Model Library & Profiles

> <img width="1722" height="847" alt="Screenshot_20260616_045724" src="https://github.com/user-attachments/assets/379f0e16-07c3-4ee7-a979-377f6e2a3103" />

### Hugging Face Explorer

> <img width="1722" height="847" alt="Screenshot_20260616_045621" src="https://github.com/user-attachments/assets/add166b5-6055-43a5-ac8e-bd8123e9141a" />

### PromptUI Workspace with LMTOP

> <img width="1928" height="1075" alt="Screenshot_20260517_032730" src="https://github.com/user-attachments/assets/0f7f334d-b044-4530-8d0e-84e6cbbef775" />

> <img width="1928" height="1075" alt="Screenshot_20260517_032344" src="https://github.com/user-attachments/assets/a88dc26a-215b-49c8-a7b5-eca54bae4adf" />

---

## Advanced GPU Monitoring (lmtop)

QikLM features a custom, GPU monitoring engine ("lmtop") that provides real-time Vram usage, load, power and temperature of the active model from the control center and PromptUI.

> [!NOTE]
> Telemetry accuracy depends on vendor drivers and OS-level support.

### Supported Lmtop Hardware

- **Intel Arc**: Xe (Battlemage/B-Series) and i915 (Alchemist/A-Series)
- **Nvidia**: Via built-in `nvidia-smi` parser (no CUDA SDK required)
- **AMD**: Linux and Windows

---

## Installation

### Option 1: Native Linux Packages (Recommended)

Native packages automatically install QikLM and set it up as a persistent background `systemd` service.

**Debian / Ubuntu**

1. Download the latest `.deb` file from the **[Releases](https://github.com/imprizza/qiklm-releases/releases)** page.
2. Install it:
   ```bash
   sudo dpkg -i qiklm_linux_amd64.deb
   ```

**Fedora / RHEL / Rocky / AlmaLinux**

1. Download the latest `.rpm` file from the **[Releases](https://github.com/imprizza/qiklm-releases/releases)** page.
2. Install it:
   ```bash
   sudo dnf install ./qiklm_linux_amd64.rpm
   ```

*(QikLM will automatically start in the background and run on boot.)*

### Option 2: macOS

**Recommended: `.pkg` installer**

1. Download the latest `.pkg` file for your chip from the **[Releases](https://github.com/imprizza/qiklm-releases/releases)** page (`qiklm_darwin_arm64.pkg` for Apple Silicon, `qiklm_darwin_amd64.pkg` for Intel).
2. Double-click it and follow the prompts.

**Alternative: `.dmg`**

1. Download the latest `.dmg` file from the **[Releases](https://github.com/imprizza/qiklm-releases/releases)** page.
2. Open the DMG and drag QikLM to your Applications folder.

### Option 3: Pre-Compiled Binary

1. Go to the **[Releases](https://github.com/imprizza/qiklm-releases/releases)** page and download the latest version for your OS.
2. Make it executable and run (Linux/macOS):
   ```bash
   chmod +x qiklm && ./qiklm
   ```
3. Windows:
   ```
   .\qiklm.exe
   ```

---

## First-Time Setup

On its first boot, QikLM generates a default configuration and starts listening on port `8192`.

Open the admin dashboard:
```text
http://localhost:8192/admin/
```

### 1. Engine Settings
On first launch, QikLM opens a setup modal to configure your core engine paths:
* The path to your local models directory.
* The path to your compiled `llama-server` executable or your `vLLM` install, or use **Managed Install** to let QikLM download and register a `llama.cpp` release for you.

**Pre-Run Initialization (optional):** This field runs any shell command before your engine starts, for any setup that needs to happen first. Chain it with `&&` so it runs ahead of the engine launch.

For **vLLM**, activate the Python environment it is installed in. For example:
```bash
source /path/to/.venv/bin/activate &&
```

For **Intel SYCL** builds of `llama.cpp`, source the oneAPI environment. For example:
```bash
source /opt/intel/oneapi/setvars.sh > /dev/null 2>&1 &&
```

### 2. Create Your First Profile
Go to the **Models** tab and create a profile for your first `.gguf` model. Configure GPU layers (`-ngl`), context size (`-c`), CPU threads (`-t`), and any custom CLI flags. Save and load.

---

## Remote Access

QikLM is a web application; any device that can reach your host can use the full Admin Dashboard and PromptUI.

**LAN / Home Lab**: Select **LAN** bind mode in Engine Settings. QikLM becomes accessible to every device on your local network.

**Tailscale / VPN**: Already running Tailscale, WireGuard, or ZeroTier? Set bind mode to LAN and QikLM is instantly available across your entire mesh, with no port forwarding, no tunnels, no extra configuration.

```bash
QIKLM_BIND_MODE=lan QIKLM_PROXY_PORT=8192 ./qiklm
```

**Custom Bind (Advanced)**: For dedicated servers, use environment variables:
```bash
QIKLM_BIND_MODE=custom QIKLM_BIND_ADDRESS=192.168.0.10 QIKLM_PROXY_PORT=8192 ./qiklm
```

---

## Run QikLM as a Service (systemd)

*(If you installed via a native Linux package, this is already done for you.)*

### 1. Create the service file
```bash
sudo nano /etc/systemd/system/qiklm.service
```

### 2. Add the configuration
Replace `your_username` and paths:
```ini
[Unit]
Description=QikLM (llama.cpp & vLLM orchestrator)
After=network.target

[Service]
Type=simple
User=your_username
WorkingDirectory=/home/your_username/qiklm
ExecStart=/home/your_username/qiklm/qiklm
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

---

## Why This Project Matters

QikLM exists for a very specific kind of user:
- someone who wants **real local performance**
- someone who already runs `llama.cpp` or `vLLM`, or wants QikLM to handle the llama.cpp setup
- someone who does **not** want to give up convenience just to keep that performance

This project is the missing middle layer between raw power and sane day-to-day use.
