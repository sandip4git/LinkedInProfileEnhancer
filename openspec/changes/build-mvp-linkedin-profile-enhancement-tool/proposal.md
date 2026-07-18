## Why

LinkedIn profile improvement is hard to do consistently because users need to evaluate positioning, clarity, keyword strength, credibility, and section-level writing quality at the same time. This MVP creates a guided web application that retrieves permitted public LinkedIn profile content from a submitted URL and turns it into actionable AI-assisted recommendations, while establishing the core React, .NET, Python agent, MongoDB, local LLM, and containerized deployment foundation for future enhancements.

## What Changes

- Introduce a React web UI where a user submits a publicly accessible LinkedIn profile URL, email address, and mobile number; views analysis results; reviews prioritized recommendations; and accepts rewritten suggestions.
- Add a C#/.NET API layer for input validation, duplicate-aware profile submission, profile and job retrieval, recommendation persistence, and frontend contracts.
- Add .NET background worker support for long-running profile analysis and agent execution workflows.
- Add MongoDB persistence for profile context, normalized and unmapped content, job state, analysis results, recommendations, and improved drafts.
- Add a Python agent service using LangGraph-style orchestration to analyze profile sections, identify gaps, score quality signals, and generate rewrite suggestions through a locally hosted Llama-family LLM.
- Add shared contracts between the React UI, .NET API, worker process, and Python agent service.
- Add a local development and deployment foundation using Docker containers, with Kubernetes-ready structure for future cloud deployment.
- Include privacy-conscious handling of profile content and basic observability for analysis jobs and agent execution.

## Capabilities

### New Capabilities

- `profile-ingestion`: Validate a submitted URL and contact identity, retrieve permitted public profile content, deduplicate re-ingestion, and persist normalized profile context in MongoDB.
- `profile-analysis`: Load profile sections by profileId and persist quality signals, scores, and gaps in MongoDB.
- `ai-recommendations`: Load profile and analysis context by profileId and persist prioritized recommendations and rewrite suggestions.
- `enhancement-workflow`: Present MongoDB-backed profile results in a guided React experience where users can review, compare, and apply suggested improvements.
- `analysis-jobs`: Manage profileId-based asynchronous analysis jobs across the .NET API, background workers, MongoDB, and Python agent service.
- `containerized-deployment`: Provide Docker-based local execution and Kubernetes-ready deployment structure for the MVP services.

### Modified Capabilities

- None.

## Impact

- Affected application areas: React UI, .NET API/services, .NET background workers, Python agent runtime, MongoDB, local LLM runtime, shared contracts, Docker/Kubernetes deployment, observability, and data privacy controls.
- New dependencies will likely include a React build stack, .NET web API and worker projects, MongoDB drivers and configuration, Python agent dependencies such as LangGraph-compatible packages, a local LLM runtime adapter, container build files, and service orchestration configuration.
- The MVP should establish clear service boundaries so the UI talks only to the .NET API, profileId is the workflow identifier, MongoDB is the source of truth for profile context, and the Python agent service owns AI workflow execution.
