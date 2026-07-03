## ADDED Requirements

### Requirement: Section quality analysis
The system SHALL analyze profile sections for completeness, clarity, positioning, keyword relevance, and credibility.

#### Scenario: Analyze submitted profile
- **WHEN** an analysis job processes normalized profile sections
- **THEN** the system produces quality signals for the submitted profile

### Requirement: Profile scoring
The system SHALL produce section-level and overall profile scores that can be displayed to the user.

#### Scenario: Return scores with analysis
- **WHEN** profile analysis completes successfully
- **THEN** the result includes an overall score and scores for analyzed sections

### Requirement: Gap identification
The system SHALL identify missing or weak profile areas that reduce profile effectiveness.

#### Scenario: Detect weak summary
- **WHEN** a profile has a short or generic summary section
- **THEN** the system identifies the summary as an improvement opportunity
