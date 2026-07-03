## ADDED Requirements

### Requirement: Prioritized recommendations
The system SHALL generate prioritized recommendations based on profile analysis results.

#### Scenario: Recommendations include priority
- **WHEN** analysis completes
- **THEN** each recommendation includes a priority or severity indicator

### Requirement: Rewrite suggestions
The system SHALL generate rewrite suggestions for profile sections where improved wording is useful.

#### Scenario: Suggest rewritten headline
- **WHEN** the headline section can be improved
- **THEN** the system provides a suggested rewritten headline

### Requirement: Local LLM execution path
The system SHALL support generating recommendations through a configurable locally hosted Llama-family LLM or compatible local model endpoint.

#### Scenario: Local model configured
- **WHEN** the agent service has a local model endpoint configured
- **THEN** the agent service uses that endpoint for recommendation generation

#### Scenario: Local model unavailable in development
- **WHEN** no local model endpoint is available in development mode
- **THEN** the agent service returns deterministic fallback recommendations suitable for testing the workflow
