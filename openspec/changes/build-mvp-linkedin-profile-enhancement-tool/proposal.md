## Why

LinkedIn profile improvement is hard to do consistently because users need to evaluate positioning, clarity, keyword strength, credibility, and section-level writing quality at the same time. This MVP creates a guided web application that turns a raw profile into actionable AI-assisted recommendations while establishing the core React, .NET, Python agent, local LLM, and containerized deployment foundation for future enhancements.

## What Changes

- Introduce a React web UI where a user can enter or upload LinkedIn profile content, view analysis results, review prioritized recommendations, and accept rewritten suggestions.
- Add a C#/.NET API layer for profile submission, analysis job orchestration, result retrieval, recommendation persistence, and frontend contracts.
- Add .NET background worker support for long-running profile analysis and agent execution workflows.
- Add a Python agent service using LangGraph-style orchestration to analyze profile sections, identify gaps, score quality signals, and generate rewrite suggestions through a locally hosted Llama-family LLM.
- Add shared contracts between the React UI, .NET API, worker process, and Python agent service.
- Add a local development and deployment foundation using Docker containers, with Kubernetes-ready structure for future cloud deployment.
- Include privacy-conscious handling of profile content and basic observability for analysis jobs and agent execution.

## Capabilities

### New Capabilities

- `profile-ingestion`: Accept LinkedIn profile content from the user and normalize it into structured profile sections for analysis.
- `profile-analysis`: Analyze profile sections for completeness, clarity, positioning, keyword relevance, credibility, and improvement opportunities.
- `ai-recommendations`: Generate prioritized recommendations and rewrite suggestions using Python agent workflows backed by a local Llama-family LLM.
- `enhancement-workflow`: Present analysis results in a guided React experience where users can review, compare, and apply suggested improvements.
- `analysis-jobs`: Manage asynchronous profile analysis jobs across the .NET API, background workers, and Python agent service.
- `containerized-deployment`: Provide Docker-based local execution and Kubernetes-ready deployment structure for the MVP services.

### Modified Capabilities

- None.

## Impact

- Affected application areas: React UI, .NET API/services, .NET background workers, Python agent runtime, local LLM runtime, shared contracts, Docker/Kubernetes deployment, observability, and data privacy controls.
- New dependencies will likely include a React build stack, .NET web API and worker projects, Python agent dependencies such as LangGraph-compatible packages, a local LLM runtime adapter, container build files, and service orchestration configuration.
- The MVP should establish clear service boundaries so the UI talks to the .NET API, the .NET backend owns jobs and application state, and the Python agent service owns AI workflow execution.
