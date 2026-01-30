# PersonaPlex for RTX 5090 (Blackwell Architecture)

Custom Docker build of PersonaPlex with PyTorch compiled for NVIDIA RTX 5090 (sm_120 support).

## Why This Exists
As of January 2026, PyTorch stable releases don't support RTX 5090's Blackwell architecture (sm_120). This Dockerfile builds PyTorch from source with sm_120 support.

## Requirements
- NVIDIA RTX 5090 (or other Blackwell GPU)
- Docker Desktop with NVIDIA Container Toolkit
- 20GB+ disk space
- 2-4 hours build time

## Build Instructions
```bash
git clone https://github.com/yourusername/personaplex-rtx5090.git
cd personaplex-rtx5090
docker build -f Dockerfile.5090 -t personaplex:5090 .
```

## Deploy with Portainer
version: '3.8'

services:
  personaplex:
    image: personaplex:5090
    container_name: personaplex
    ports:
      - "8998:8998"
    environment:
      - HF_TOKEN=yourtokenhere
      - NO_TORCH_COMPILE=1
    volumes:
      - personaplex-data:/root/.cache
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
    restart: unless-stopped

volumes:
  personaplex-data:

## Credits 
- NVIDIA PersonaPlex: https://github.com/NVIDIA/personaplex
- Built with help from Claude (Anthropic)
