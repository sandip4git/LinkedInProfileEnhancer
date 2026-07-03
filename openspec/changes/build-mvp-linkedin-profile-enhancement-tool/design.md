## Context

The repository currently contains OpenSpec planning artifacts for a new LinkedIn Profile Enhancer product. The MVP needs to establish the first production-oriented slice across four deployable areas:

- React web UI for profile input, analysis review, and improvement workflow.
- C#/.NET API for contracts, validation, job creation, result retrieval, and application state.
- C#/.NET background worker for asynchronous analysis orchestration.
- Python agent service for LangGraph-style profile analysis and local Llama-family LLM interaction.

The design must keep these components independently deployable while still simple enough for local development through Docker Compose and future Kubernetes deployment.

## Goals / Non-Goals

**Goals:**

- Establish a runnable MVP skeleton with clear boundaries between UI, API, worker, and agent service.
- Support submitting LinkedIn profile content and tracking asynchronous analysis jobs.
- Produce structured analysis output, prioritized recommendations, and rewrite suggestions through a Python agent workflow.
- Use local LLM integration as the default model path, with a mock/fallback mode for development when a model is unavailable.
- Provide Docker-based local execution and Kubernetes-ready manifests for future cloud deployment.
- Include basic privacy, observability, and validation practices around profile content and job processing.

**Non-Goals:**

- Browser automation or direct LinkedIn account scraping.
- Authentication, billing, teams, or user account management.
- Production-grade persistence, distributed queues, or cloud-specific infrastructure.
- Fine-tuning LLMs or managing model hosting beyond a local-compatible adapter contract.
- Full resume import, PDF parsing, or third-party enrichment integrations.

## Decisions

### Separate Deployable Services

Use separate deployable components for React UI, .NET API, .NET worker, and Python agent service.

Rationale: This matches the planned stack and keeps frontend, backend orchestration, background work, and AI logic independently testable and deployable.

### .NET Owns API Contracts and Job State

The .NET API will expose profile submission and job result endpoints. The worker will process pending jobs and call the Python agent service. For the MVP, in-memory storage can be used behind repository interfaces so persistent storage can be added later without changing the UI contract.

Rationale: The API remains the stable contract for the React UI, while long-running work is kept out of request/response execution.

### Python Agent Service Owns AI Workflow

The Python service will expose an HTTP endpoint that accepts normalized profile sections and returns structured analysis. Its internal workflow should be organized around LangGraph-compatible nodes such as section extraction, scoring, gap analysis, recommendation generation, and rewrite generation.

Rationale: Python gives the agent runtime the ecosystem needed for LangGraph and LLM orchestration while keeping the .NET side focused on product/application behavior.

### Local LLM Adapter with Development Fallback

The agent service will call a configurable local LLM endpoint, such as an Ollama-compatible Llama endpoint, through a small adapter. When no local model is available, the service can return deterministic mock suggestions for development and tests.

Rationale: This lets the MVP run locally without making hosted model access a hard requirement, while preserving a clean adapter boundary for future providers.

### Contract-First Data Shapes

Define shared DTOs around profile sections, analysis jobs, analysis results, recommendations, and rewrite suggestions. The React UI should depend on API response shapes, not agent internals.

Rationale: Stable contracts reduce coupling and make it easier to replace storage, queues, and model execution later.

### Container-First Local Development

Add Dockerfiles for the UI, API, worker, and agent service plus a Docker Compose file for local orchestration. Add Kubernetes manifests as a baseline deployment shape without binding to a specific cloud platform. Keep in mind that I will be using PODMAN desktop for local container hosting platform as its freely available without license on windows 11

Rationale: The MVP should be deployable in a cloud-neutral way and easy to run locally with service boundaries intact.

## Risks / Trade-offs

- Local LLM availability and performance can vary by machine -> Provide a mock mode and keep model endpoint configuration externalized.
- In-memory job state will not survive restarts -> Hide storage behind interfaces and document persistent storage as a follow-up.
- Worker polling is simpler than a real queue but less scalable -> Keep job orchestration isolated so a queue can replace polling later.
- Generated recommendations may be low quality or overly generic -> Structure agent prompts around concrete profile sections and expose editable suggestions in the UI.
- Profile content is sensitive -> Avoid logging raw profile text, keep inputs scoped to the local system, and document privacy expectations.
- Kubernetes manifests may be generic -> Keep manifests cloud-neutral and avoid provider-specific assumptions until deployment target is selected.

## Migration Plan

1. Add the initial repo structure and service skeletons.
2. Run services locally without containers to validate each component.
3. Run the same components through Docker Compose.
4. Validate Kubernetes manifests syntactically where tooling is available.
5. Rollback by reverting the new service directories and OpenSpec change if the scaffold proves unsuitable.

## Open Questions

- Which local LLM runtime should be the default documented path: Ollama, llama.cpp server, or another OpenAI-compatible local gateway?
Answer: Use Ollama for local llm runtime as its open source and freely avaialable
- Which persistence layer should replace in-memory job storage after the MVP: PostgreSQL, SQL Server, or another option?
Answer: PostgreSQL as its freely available, no licence cost attached to it
- Should the first production deployment use raw Kubernetes manifests, Helm, or Kustomize?
Answer: Lets get started with raw Kubernetes manifests for first PROD deployment
