## ADDED Requirements

### Requirement: Guided results review
The system SHALL present analysis results in a guided UI that helps users understand profile strengths and improvement opportunities.

#### Scenario: View completed analysis
- **WHEN** a user opens a completed analysis job
- **THEN** the UI displays scores, recommendations, and rewrite suggestions

### Requirement: Compare original and suggested text
The system SHALL allow users to compare original profile content with suggested rewritten content.

#### Scenario: Compare rewrite
- **WHEN** a recommendation includes a rewrite suggestion
- **THEN** the UI shows the original section text and suggested replacement text together

### Requirement: Apply suggestion locally
The system SHALL allow users to mark suggestions as accepted within the enhancement workflow.

#### Scenario: Accept suggestion
- **WHEN** a user accepts a rewrite suggestion
- **THEN** the UI reflects the accepted suggestion in the improved profile draft
