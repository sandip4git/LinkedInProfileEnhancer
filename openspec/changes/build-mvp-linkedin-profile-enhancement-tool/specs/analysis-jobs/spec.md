## ADDED Requirements

### Requirement: Asynchronous analysis job creation
The system SHALL create asynchronous analysis jobs for profile submissions.

#### Scenario: Job created on submission
- **WHEN** a valid profile submission is received
- **THEN** the API returns an analysis job identifier and initial job status

### Requirement: Job status tracking
The system SHALL allow clients to retrieve the current status of an analysis job.

#### Scenario: Retrieve pending job
- **WHEN** a client requests a job that has not completed
- **THEN** the API returns the current pending or processing status

#### Scenario: Retrieve completed job
- **WHEN** a client requests a completed job
- **THEN** the API returns completed status and the analysis result

### Requirement: Background processing
The system SHALL process analysis jobs outside the initial API request.

#### Scenario: Worker processes queued job
- **WHEN** a new analysis job is pending
- **THEN** a background worker processes the job and records the result or failure
