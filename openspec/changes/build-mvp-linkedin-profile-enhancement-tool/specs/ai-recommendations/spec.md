## ADDED Requirements

### Requirement: ProfileId-based prioritized recommendations
The system SHALL load profile and analysis context from MongoDB using profileId and generate prioritized recommendations.

#### Scenario: Recommendations include priority
- **WHEN** analysis completes for a profileId
- **THEN** each recommendation includes a priority or severity indicator and is stored with that profile

### Requirement: Rewrite suggestions
The system SHALL generate rewrite suggestions for profile sections where improved wording is useful.

#### Scenario: Suggest rewritten headline
- **WHEN** the headline section for a profileId can be improved
- **THEN** the system stores a suggested rewritten headline with that profile

### Requirement: Local LLM execution path
The system SHALL support generating recommendations through a configurable locally hosted Llama-family LLM or compatible local model endpoint.

#### Scenario: Local model configured
- **WHEN** the agent service receives a profileId and has a local model endpoint configured
- **THEN** the agent service loads the required MongoDB context and uses that endpoint for recommendation generation

#### Scenario: Local model unavailable in development
- **WHEN** no local model endpoint is available in development mode
- **THEN** the agent service stores deterministic fallback recommendations suitable for testing the workflow against the profileId
