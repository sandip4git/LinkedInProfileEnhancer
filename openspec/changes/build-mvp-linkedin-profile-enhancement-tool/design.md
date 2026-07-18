## Context

The repository currently contains OpenSpec planning artifacts for a new LinkedIn Profile Enhancer product. The MVP needs to establish the first production-oriented slice across four deployable areas:

- React web UI for profile input, analysis review, and improvement workflow.
- C#/.NET API for contracts, URL and contact-identity validation, profile/job retrieval, and application state.
- C#/.NET background worker for permitted public-profile retrieval, normalization, and asynchronous analysis orchestration.
- Python agent service for LangGraph-style profile analysis and local Llama-family LLM interaction.
- MongoDB for profile context, workflow state, analysis results, recommendations, and improved drafts.

The design must keep these components independently deployable while still simple enough for local development through Docker Compose and future Kubernetes deployment.

## Goals / Non-Goals

**Goals:**

- Establish a runnable MVP skeleton with clear boundaries between UI, API, worker, and agent service.
- Support submitting a public LinkedIn profile URL with email and mobile number, preventing duplicate profiles, and tracking asynchronous analysis jobs.
- Produce structured analysis output, prioritized recommendations, and rewrite suggestions through a Python agent workflow.
- Use local LLM integration as the default model path, with a mock/fallback mode for development when a model is unavailable.
- Provide Docker-based local execution and Kubernetes-ready manifests for future cloud deployment.
- Include basic privacy, observability, and validation practices around profile content and job processing.

**Non-Goals:**

- Authentication, billing, teams, or user account management.
- Retrieval of content that is not publicly accessible, permitted by the source, or legally available to the system.
- Distributed queues or cloud-specific infrastructure.
- Fine-tuning LLMs or managing model hosting beyond a local-compatible adapter contract.
- Full resume import, PDF parsing, or third-party enrichment integrations.

## Decisions

### Separate Deployable Services

Use separate deployable components for React UI, .NET API, .NET worker, and Python agent service.

Rationale: This matches the planned stack and keeps frontend, backend orchestration, background work, and AI logic independently testable and deployable.

### .NET Owns API Contracts and Profile Workflow State

The .NET API will expose profile submission, profile retrieval, job status, and result endpoints. A valid submission includes a public LinkedIn URL, email address, and mobile number. It creates or reuses a profileId after duplicate detection. The worker retrieves permitted public content, normalizes it, and processes pending jobs. MongoDB persists the profile document and job state; repositories provide controlled access to that document.

Rationale: The API remains the stable contract for the React UI, long-running work stays out of request/response execution, and profile context survives service restarts.

### Python Agent Service Owns AI Workflow

The Python service will receive a profileId, load only the required profile and analysis context through a controlled MongoDB persistence interface, and persist structured analysis and recommendation output against that profileId. Its internal workflow should be organized around LangGraph-compatible nodes such as scoring, gap analysis, recommendation generation, and rewrite generation.

Rationale: Python gives the agent runtime the ecosystem needed for LangGraph and LLM orchestration while keeping the .NET side focused on product/application behavior.

### Local LLM Adapter with Development Fallback

The agent service will call a configurable local LLM endpoint, such as an Ollama-compatible Llama endpoint, through a small adapter. When no local model is available, the service can return deterministic mock suggestions for development and tests.

Rationale: This lets the MVP run locally without making hosted model access a hard requirement, while preserving a clean adapter boundary for future providers.

### Contract-First Data Shapes

Define shared DTOs around profileId, analysis jobs, analysis results, recommendations, and rewrite suggestions. The React UI should depend on API response shapes, not MongoDB or agent internals. Email and mobile number are never included in analysis or recommendation DTOs.

Rationale: Stable contracts reduce coupling and make it easier to replace storage, queues, and model execution later.

### Container-First Local Development

Add Dockerfiles for the UI, API, worker, and agent service plus a Docker Compose file for local orchestration. Add Kubernetes manifests as a baseline deployment shape without binding to a specific cloud platform. Keep in mind that I will be using PODMAN desktop for local container hosting platform as its freely available without license on windows 11

MongoDB MUST run as a configured service with persistent storage in the local stack and as a StatefulSet or managed equivalent in Kubernetes. Connection strings and the fingerprint key MUST be supplied through configuration or secrets.

Rationale: The MVP should be deployable in a cloud-neutral way and easy to run locally with service boundaries intact.

### MongoDB Profile Document and Duplicate Detection

MongoDB SHALL use a `profiles` collection. Each document contains an internal `_id`, a public UUID `profileId`, source URL and retrieval metadata, ingestion and analysis status, raw content, normalized sections, unmapped content, analysis results, recommendations, improved draft state, timestamps, and a schema version. The document contains a protected `identity` object with normalized email, normalized E.164 mobile number, and an `identityFingerprint` calculated with a keyed hash of both values.

The logical document schema is:

```text
profiles
  _id: ObjectId
  profileId: UUID
  identity:
    email: protected normalized string
    mobileNumber: protected E.164 string
    identityFingerprint: keyed hash string
  source:
    linkedInPublicUrl: string
    retrievedAt: UTC timestamp
  ingestion:
    status: pending | fetched | normalized | failed
    failureReason: safe string?
  content:
    rawContent: protected string
    normalizedSections:
      headline: string?
      about: string?
      experience: array
      skills: array
      education: array
      recommendations: string?
    unmappedContent: string?
  processing:
    analysisJobId: UUID?
    analysisStatus: pending | processing | completed | failed
    analysisResult: object?
    recommendations: array?
    improvedDraft: object?
  createdAt: UTC timestamp
  updatedAt: UTC timestamp
  schemaVersion: integer
```

MongoDB SHALL have a unique index on `identity.identityFingerprint`, an index on `profileId`, an index on `processing.analysisJobId`, and a status/timestamp index for worker polling. A re-ingestion that matches an existing fingerprint reuses the existing profileId and refreshes its source and ingestion context instead of creating a duplicate document.

Rationale: The profileId provides a non-sensitive workflow reference, while the fingerprint enables idempotent ingestion without using raw contact values for lookups or logs.

## Risks / Trade-offs

- Local LLM availability and performance can vary by machine -> Provide a mock mode and keep model endpoint configuration externalized.
- Public LinkedIn URL retrieval can be unavailable or prohibited by source terms, robots controls, rate limits, or access controls -> Retrieve only permitted public content, use allow-listed URL validation and rate limiting, and return safe failures when retrieval is unavailable.
- URL retrieval can introduce SSRF risk -> Restrict schemes and hosts, resolve and reject private or link-local addresses, limit redirects, and set strict timeouts and response-size limits.
- Contact details and raw profile content are sensitive -> Normalize and protect contact data, use a keyed identity fingerprint for duplicate checks, do not log raw profile text/email/mobile values, and define retention/deletion behavior before production.
- Worker polling is simpler than a real queue but less scalable -> Keep job orchestration isolated so a queue can replace polling later.
- Generated recommendations may be low quality or overly generic -> Structure agent prompts around concrete profile sections and expose editable suggestions in the UI.
- Profile content is sensitive -> Avoid logging raw profile text, keep inputs scoped to the local system, and document privacy expectations.
- Kubernetes manifests may be generic -> Keep manifests cloud-neutral and avoid provider-specific assumptions until deployment target is selected.

## Migration Plan

1. Add the initial repo structure and service skeletons.
2. Run MongoDB and services locally without containers to validate profile ingestion, duplicate detection, and profileId-based processing.
3. Run the same components through Podman-compatible Docker Compose.
4. Validate Kubernetes manifests syntactically where tooling is available.
5. Rollback by reverting the new service directories and OpenSpec change if the scaffold proves unsuitable.

## Open Questions

- Which local LLM runtime should be the default documented path: Ollama, llama.cpp server, or another OpenAI-compatible local gateway?
Answer: Use Ollama for local llm runtime as its open source and freely avaialable
- Which persistence layer should store MVP profile context and workflow results?
Answer: MongoDB, using a versioned profile document and protected contact-identity fingerprint for idempotent re-ingestion.
- Should the first production deployment use raw Kubernetes manifests, Helm, or Kustomize?
Answer: Lets get started with raw Kubernetes manifests for first PROD deployment
