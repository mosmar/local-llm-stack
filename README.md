# local-llm-stack

Docker Compose setup for running LLMs locally with [Ollama](https://ollama.com) and [Open WebUI](https://github.com/open-webui/open-webui).

## Layout

| Directory | Description |
|-----------|-------------|
| `ollama/` | Ollama server (GPU-enabled) on port `11434`. The Dockerfile pre-pulls `qwen3:14b`. |
| `openwebui/` | Open WebUI chat frontend on port `3000`, connected to Ollama on the host. |

## Prerequisites

- Docker with Compose
- NVIDIA GPU with the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

## Usage

Start Ollama first, then Open WebUI:

```sh
cd ollama
docker compose up -d

cd ../openwebui
docker compose up -d
```

Open WebUI is then available at <http://localhost:3000>.

Pull additional models with:

```sh
docker exec ollama ollama pull <model>
```

## Configuration

Ollama settings (in `ollama/docker-compose.yml`):

- `OLLAMA_CONTEXT_LENGTH=8192`: default context window
- `OLLAMA_KEEP_ALIVE=24h`: keep models loaded in memory for 24 hours
- `OLLAMA_MODELS=/root/.ollama/models`: model storage path (persisted in the `ollama` volume)

Open WebUI reaches Ollama through `host.docker.internal:11434`. Change `OLLAMA_BASE_URL` in `openwebui/docker-compose.yml` if Ollama runs elsewhere.

Data is stored in the named volumes `ollama` and `open-webui-data`.
