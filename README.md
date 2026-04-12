## LlamaOrch

<div align="center">
<!-- <img width="580" alt="logo" src="https://github.com/user-attachments/assets/c31f7b83-0ce0-4df7-9b55-e027f80fd537" /> -->

<img src="demo/demo.gif" alt="llamaorch demo" width="780"><br><br>

[![Bash](https://img.shields.io/badge/Bash-CLI-red.svg)](https://www.gnu.org/software/bash/)
[![fzf](https://img.shields.io/badge/fzf-optional-purple.svg)](https://github.com/junegunn/fzf)
[![Linux](https://img.shields.io/badge/Linux-supported-brightgreen.svg)](https://www.kernel.org/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/alasgarovs/openserv/blob/main/LICENSE)

<br>

</div>

LlamaOrch is simple Bash-based CLI Orchestrator for managing LLMs in llama.cpp server. It lets you start, stop, list and monitor LLMs in llama-server with ease.

## Features

- **Interactive model selection** with fzf (or numbered fallback)
- **Start, stop and monitor** llama-server instances
- **Per-model config files** — customize any llama-server flag
- **Live status** with port detection, PID tracking and clickable URLs
- **Log tailing** for real-time output
- **Zero dependencies** beyond Bash (fzf is optional)

## Installation

### For End Users

The first pre-built package is now released. You can find the release [here](https://github.com/alasgarovs/llamaorch/releases)

or quick install with `curl`

```bash
curl -fsSL https://raw.githubusercontent.com/alasgarovs/llamaorch/main/install | bash
```

### Manual install

```bash
git clone https://github.com/alasgarovs/llamaorch.git
cd llamaorch
sh src/configure
```

This installs the `llamaorch` command to `~/.local/bin/llamaorch` and sets up the config directory at `~/.llamaorch/`.

## Commands

| Command | Description |
|---------|-------------|
| `run` | Launch a model with interactive selection |
| `stop` | Gracefully stop a running model |
| `restart` | Stop and start a running model automatically |
| `ps` | Show status of all configured models (ports, PIDs, URLs) |
| `ls` | List all available model configs |
| `log` | Tail the log file for a model (`Ctrl+C` to exit) |
| `create <name>` | Create a new model config file and open it in `nano` |
| `edit` | Edit an existing model config in `nano` |
| `rm` | Delete a model config, PID file and log |
| `help` | Display command reference |

## Configuration

Model configs live in `~/.llamaorch/config/` as individual `.sh` scripts. Each script is a standard Bash file that launches `llama-server` with your desired flags.

### Example config (`~/.llamaorch/config/my-model.sh`)

```bash
#!/bin/bash

llama-server \
	-m ~/.llamaorch/models/example-model.gguf \
	-ngl 28 \
	-c 6144 \
	-t 6 \
	-b 192 \
	--ubatch-size 64 \
	--flash-attn off \
	--cont-batching \
	--port 18080 \
	--host 0.0.0.0
```

> The `--port` flag is required for live status detection. LlamaOrch parses it automatically.

### Directory structure

```
~/.llamaorch/
├── bin/
│   └── llamaorch          # main executable
├── config/
│   ├── default            # example config
│   └── my-model.sh        # your model configs
├── pids/
│   ├── my-model.pid       # PID files
│   └── my-model.log       # log files
└── models/                # place your .gguf files here
```

## Requirements

- **llama-server** (from [llama.cpp](https://github.com/ggml-org/llama.cpp)) installed and in `$PATH`
- **fzf** (optional — provides fuzzy-finder UI; falls back to numbered menu)
- **lsof** (for port/PID detection)

---

<div align="center">

⭐ Star us on GitHub if you find this project helpful!

</div>
