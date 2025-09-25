### AI Talent Hub × 2GIS Hackathon — Open WebUI Based AI Assistant

Local-first AI assistant built on top of Open WebUI with a modern Svelte frontend and FastAPI backend. The project is containerized, easy to run with Docker or Docker Compose, and supports both local LLMs via Ollama and OpenAI‑compatible APIs.

### Navigation Assistant Functionality (2GIS-powered)
- **Landmark-based step-by-step guidance**: the assistant generates human-friendly directions that reference recognizable points of interest (POIs) near each turn.
- **Live 2GIS integration**:
  - Catalog API (`/3.0/items`, `/3.0/items/geocode`) to resolve free-form queries and addresses into precise coordinates and to discover nearby POIs around turn points.
  - Routing API (`/routing/7.0.0/global`) to build walking routes and extract maneuver geometry and comments.
- **POI enrichment**: for every maneuver, the system searches nearby places, filters those roughly in front of the user, scores them by proximity and importance, and picks the best landmark(s).
- **Instruction polishing**: if an OpenAI-compatible key is configured, the assistant rewrites the maneuver comment using the most relevant landmark for a short, natural instruction.
- **Sessioned navigation**: a route is cached under a `route_id`; the chat supports commands like “next” to advance through steps.


---


### Highlights
- **Self-hosted & offline-capable**: keep data local; run with or without external APIs
- **LLM flexibility**: use Ollama models or point to any OpenAI‑compatible endpoint
- **RAG & tools**: document ingestion, web search providers, function-calling tools
- **Clean UX**: responsive Svelte UI with PWA support for mobile
- **One-command start**: Docker Compose recipes for CPU/GPU and optional Playwright




Available tools (via the tools workspace):
- `start_route(start: string, destination: string)` → builds a route, returns `route_id`, total steps, and the first instruction.
- `next_step(route_id: string)` → returns the next instruction until arrival.

Environment configuration:
- 2GIS API key and optional OpenAI key should be provided via environment variables when running in production.

---

### Quick Start

Option A — Docker (single container, CPU):

```bash
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

Option B — Docker Compose (Open WebUI + Ollama):

```bash
cd UI/AI-Talent-Hub-x-2Gis-Hackaton
./run-compose.sh --webui[port=3000]
```

Notes:
- Data persists in the `open-webui` volume; Ollama models in `ollama` volume.
- To enable GPU in Compose: `./run-compose.sh --enable-gpu[count=1]` (requires proper drivers/toolkit).
- To expose only OpenAI API workflow: set `OPENAI_API_KEY` and run WebUI container without Ollama.

---

### Project Structure (key parts)
- `src/` — SvelteKit frontend, Tailwind styling, i18n
- `backend/` — FastAPI service and tooling (`requirements.txt` provided)
- `docker-compose.yaml` — WebUI + Ollama services, volumes and ports
- `run-compose.sh` — helper script to enable GPU/API/data options quickly
- `Dockerfile` — container build for Open WebUI image

---

### Tech Stack
- Frontend: SvelteKit, TailwindCSS, Vite
- Backend: FastAPI, Uvicorn, Pydantic
- LLM Runtimes: Ollama (local), OpenAI‑compatible APIs (external)
- Packaging: Docker, Docker Compose
- Node tooling: TypeScript, ESLint, Prettier, Vitest, Cypress

---

### Typical Use Cases
- Local AI assistant for demos and product pitches
- Private RAG chat over PDFs, docs, and web pages
- Team knowledge base with RBAC and multi-model comparison

---

### How To Develop

Frontend dev server:
```bash
cd UI/AI-Talent-Hub-x-2Gis-Hackaton
npm install
npm run dev
```

Backend dependencies (for local Python tooling):
```bash
cd UI/AI-Talent-Hub-x-2Gis-Hackaton/backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

---

### Why This Project Works Well For a Hackathon
- Rapid setup: run locally or with Compose in minutes
- Strong baseline features: chat, RAG, tools, voice/video, images
- Extensible: plugins/pipelines to add custom logic and integrations

---

### License
This repository builds on Open WebUI (BSD-3-Clause variant). See the original `LICENSE` in the project root for details.