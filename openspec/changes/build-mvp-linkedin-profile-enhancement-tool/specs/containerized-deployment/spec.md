## ADDED Requirements

### Requirement: Containerized services
The system SHALL provide container definitions for the React UI, .NET API, .NET worker, and Python agent service.

#### Scenario: Build service containers
- **WHEN** container build commands are run for the MVP services
- **THEN** each service has a corresponding container image definition

### Requirement: Local container orchestration
The system SHALL provide a Docker Compose configuration for running the MVP services together locally.

#### Scenario: Start local stack
- **WHEN** Docker Compose is started
- **THEN** the UI, API, worker, and agent service are configured as cooperating services

### Requirement: Kubernetes-ready manifests
The system SHALL include cloud-neutral Kubernetes manifests or manifest scaffolding for deploying the MVP services.

#### Scenario: Inspect Kubernetes deployment
- **WHEN** an operator reviews the Kubernetes manifests
- **THEN** each MVP runtime component has a deployment and service definition where appropriate
