# Tech Context: Veritarium on DGX Spark

## Target Platform

### Hardware: NVIDIA DGX Spark
- **CPU**: Grace (ARM64 / aarch64)
- **GPU**: NVIDIA Blackwell
- **Memory**: Unified memory architecture (CPU/GPU shared)
- **Storage**: NVMe SSD
- **Network**: High-speed networking for potential multi-node (future)

### Operating System
- **OS**: Ubuntu Linux (ARM64)
- **Shell**: Bash (primary), PowerShell Core (optional)
- **Container Runtime**: Docker with NVIDIA Container Toolkit

### Key ARM64 Considerations
- All container images must be ARM64-native or multi-arch
- Some Python packages may lack ARM64 wheels (fallback to source builds)
- NGC containers provide optimized ARM64 bases for AI workloads
- Unified memory allows larger model contexts than discrete GPU systems

---

## Development Environment

### NVIDIA AI Workbench
- Reproducible development capsules
- Projects are deletable and re-creatable
- Avoids OS churn during iteration
- Isolated from production runtime

### Cline + VS Code
- Engineering accelerant (not runtime dependency)
- Plan/Act mode workflow
- Quality gates enforced after each step
- Memory bank for context persistence

### Package Manager: uv (Recommended)
```bash
# Install uv (from Astral, same team as Ruff)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create venv and install
uv venv
uv sync

# Run commands
uv run python -m pytest
uv run ruff check .
```

**Why uv over pip:**
- 10-100x faster dependency resolution
- Lock file support (`uv.lock`)
- Native ARM64 support
- Replaces pip, pip-tools, virtualenv, pyenv

---

## Quality Gates

### Per-Service Gate
```bash
cd services/<service>
uv run ruff check .
uv run ruff format --check .
uv run pytest
```

### Full System Gate
```bash
./scripts/check.sh
```

This runs:
- All service linters (ruff)
- All service formatters (ruff format)
- All service tests (pytest)
- Container builds (optional)
- Integration smoke tests (if services running)

---

## Container Strategy

### Base Images (ARM64)
| Service | Base Image |
|---------|-----------|
| Python services | `python:3.12-slim-bookworm` (arm64) |
| vLLM | NGC vLLM image (ARM64 optimized) |
| Qdrant | `qdrant/qdrant:latest` (multi-arch) |
| Rust services | `rust:alpine` then `alpine:latest` (multi-stage) |

### Compose Profiles
```bash
# Development (with hot reload)
docker compose -f compose/compose.yaml -f compose/compose.dev.yaml up

# Production (hardened)
docker compose -f compose/compose.yaml -f compose/compose.prod.yaml up -d

# Offline deployment
docker compose -f compose/compose.yaml -f compose/compose.offline.yaml up -d
```

### Offline Bundle
```bash
# Create bundle (includes images + model weights)
./scripts/bundle-offline.sh

# Restore on air-gapped system
./scripts/restore.sh /path/to/bundle.tar.gz
```

---

## Model Serving (vLLM)

### Configuration
```yaml
# config/models.yaml
models:
  primary:
    name: "meta-llama/Llama-3.1-70B-Instruct"
    max_model_len: 32768
    tensor_parallel_size: 1  # Single GPU on DGX Spark
    gpu_memory_utilization: 0.85
```

### API Compatibility
- vLLM exposes OpenAI-compatible API
- Endpoint: `http://vllm:8000/v1`
- No external API calls in production

---

## Storage Paths

### Development
```
./data/
├── documents/          # Source PDFs
├── canonical/          # Processed canonical PDFs
├── text/               # Extracted per-page text
├── index/              # Deterministic finder index
├── qdrant/             # Vector store data
└── models/             # Cached model weights
```

### Production
```
/var/veritarium/
├── documents/
├── canonical/
├── text/
├── index/
├── qdrant/
└── models/
```

---

## Troubleshooting

### Level 1: Basic Health Check
```bash
# Service health
docker compose ps
docker compose logs --tail=50 orchestrator

# GPU availability
nvidia-smi

# Python environment
uv run python -c "import torch; print(torch.cuda.is_available())"
```

### Level 2: ARM64 Package Issues
**Symptoms**: pip install fails, "no matching distribution"

**Fix**:
1. Check if ARM64 wheel exists: `pip index versions <package>`
2. Try conda-forge: `conda install -c conda-forge <package>`
3. Build from source: `pip install --no-binary :all: <package>`
4. Use NGC container with pre-built packages

### Level 3: Memory Pressure
**Symptoms**: OOM kills, slow inference, model loading fails

**Fix**:
1. Check memory: `free -h`, `nvidia-smi`
2. Reduce `gpu_memory_utilization` in vLLM config
3. Reduce `max_model_len` (context length)
4. Use quantized model (AWQ, GPTQ)

### Level 4: Container Issues
**Symptoms**: Container won't start, image pull fails

**Fix**:
1. Verify ARM64 image: `docker manifest inspect <image>`
2. Check NVIDIA runtime: `docker run --rm --gpus all nvidia/cuda:12.0-base nvidia-smi`
3. Rebuild with `--platform linux/arm64`

---

## Key Commands Reference

### Development
```bash
# Start dev environment
make dev-up

# Run quality gate
make check

# Run single service tests
make test-orchestrator

# Build all containers
make build

# Create offline bundle
make bundle
```

### Deployment
```bash
# Deploy production
make deploy-prod

# Backup all data
make backup

# Restore from backup
make restore BUNDLE=/path/to/backup.tar.gz

# Update model weights
make update-model MODEL=meta-llama/Llama-3.1-70B-Instruct
```

---

*Update this file when technical context changes.*
*Keep troubleshooting ladder current with discovered solutions.*
