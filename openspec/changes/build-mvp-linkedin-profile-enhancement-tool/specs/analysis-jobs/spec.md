## ADDED Requirements

### Requirement: ProfileId-based asynchronous analysis job creation
The system SHALL create asynchronous analysis jobs for profile submissions using profileId as the job context reference.

#### Scenario: Job created on submission
- **WHEN** a valid profile submission is received
- **THEN** the API returns the profileId, analysis job identifier, and initial job status

### Requirement: Job status tracking
The system SHALL allow clients to retrieve the current status of an analysis job.

#### Scenario: Retrieve pending job
- **WHEN** a client requests a job that has not completed
- **THEN** the API returns the current pending or processing status

#### Scenario: Retrieve completed job
- **WHEN** a client requests a completed job using its profileId and job identifier
- **THEN** the API returns completed status and the analysis result stored for that profileId

### Requirement: Background processing
The system SHALL process analysis jobs outside the initial API request.

#### Scenario: Worker processes queued job
- **WHEN** a new analysis job is pending
- **THEN** a background worker loads the profile context from MongoDB using profileId and records the result or failure against that profile
