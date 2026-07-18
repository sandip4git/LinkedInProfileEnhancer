## ADDED Requirements

### Requirement: Containerized services
The system SHALL provide container definitions for the React UI, .NET API, .NET worker, Python agent service, and MongoDB profile persistence service.

#### Scenario: Build service containers
- **WHEN** container build commands are run for the MVP services
- **THEN** each application service has a corresponding container image definition and MongoDB is configured as a profile persistence dependency

### Requirement: Local container orchestration
The system SHALL provide a Docker Compose configuration for running the MVP services together locally.

#### Scenario: Start local stack
- **WHEN** Podman-compatible Docker Compose is started
- **THEN** the UI, API, worker, agent service, and MongoDB are configured as cooperating services with persistent MongoDB storage

### Requirement: Kubernetes-ready manifests
The system SHALL include cloud-neutral Kubernetes manifests or manifest scaffolding for deploying the MVP services.

#### Scenario: Inspect Kubernetes deployment
- **WHEN** an operator reviews the Kubernetes manifests
- **THEN** each application component has a deployment and service definition where appropriate, and MongoDB has a StatefulSet or managed-service configuration with persistent storage and secret-based connection configuration
