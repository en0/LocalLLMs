# Local AI Crap

Run local LLMs using OpenwebUI and Ollama.

This repo contains a docker compose file to spin up OpenwebUI and Ollama with the SearXNG search
aggregator availble for web searches within open web ui.

## Quick Start

### Prerequisites

- Docker
- Docker Compose
- jq*

*jq is only required to run the script that enables json support in SearXNG. You can simply use your
favorite text editor.

### Compose up

Start the compose services

```bash
docker compose up -d
```

### SearXNG JSON Support

If you want to use the websearch functionality, you must enable json format support in SearXNG. You
only need to run this once. Changes are saved in your Docker volume.

```bash
PROJ=$(basename $PWD | tr '[:upper:]' '[:lower:]')
sudo sed -i '/\ \ formats:/a \ \ \ \ - json' $(docker volume inspect ${PROJ}_searxng | jq '.[].Mountpoint' | cut -d'"' -f2)/settings.yml
```

Then, restart the SearXNG service

```bash
docker compose restart searxng
```

### Install A Model

You can install a model from OpenwebUI in the admin panel. The easy way to do it is from the CLI.

```bash
docker run ollama ollama pull gemma3
```

## Services

- OpenwebUI: http://localhost:3000
- SearXNG: http://localhost:3001
