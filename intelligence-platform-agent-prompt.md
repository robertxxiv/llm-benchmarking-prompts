# Project Brief

Build a web-based intelligence analysis platform inspired by Palantir Gotham's core UX patterns — entity-relationship graphs, geospatial mapping, timeline analysis, and unified search across ingested data — using only free, open-source software and publicly available OSINT data sources.

No proprietary Palantir code, branding, or data is involved; this recreates the category of workflow — link analysis / all-source intelligence fusion — not Palantir's product.

# Execution Mode

Create a detailed implementation plan first.

Once the plan is complete, execute it autonomously from start to finish.

Use a continuous Ralph-style completion loop combined with Gauntlet-style verification:

**IMPLEMENT → TEST → REVIEW → FIX → RE-TEST → CONTINUE**

Do not stop after completing a milestone, phase, feature, or individual task.

Continue working until the entire project is implemented and objectively verified.

## Core Rules

- Follow the implementation plan unless technical dependencies require reordering.
- Continuously compare the implementation against both the original project brief and the plan.
- Do not ask for confirmation between normal implementation steps.
- Make reasonable engineering decisions autonomously.
- Only request clarification when a required decision genuinely cannot be inferred safely.
- Do not leave TODOs, placeholders, stubs, fake integrations, unfinished wiring, or mocked production functionality.
- Do not consider a feature complete merely because its UI exists.
- Verify every important feature end-to-end, including its underlying data flow.
- Use only free and open-source dependencies unless explicitly instructed otherwise.
- Use only publicly available and legally accessible OSINT data sources.
- Keep the architecture modular so data sources, analytical modules, visualization components, and storage layers can be replaced or extended later.
- Prefer maintainability and clear architecture over unnecessary complexity.

# Environment Awareness

Before making environment-sensitive changes, determine whether the current environment is:

- development
- testing
- staging
- production

Do not assume development settings are safe for production.

For production-sensitive changes, explicitly evaluate:

- authentication
- authorization
- secrets management
- environment variables
- CORS
- exposed services
- network bindings
- debug endpoints
- logging
- sensitive data exposure
- database permissions
- default credentials
- dependency vulnerabilities
- rate limiting
- input validation
- file upload handling
- API security

Use secure defaults.

Never expose secrets or credentials in source code, frontend bundles, logs, commits, or configuration examples intended for production.

# Functional Requirements

The platform should provide an integrated intelligence-analysis workflow covering at minimum:

## Data Ingestion

Support ingestion of structured and semi-structured OSINT data.

Design ingestion as an extensible adapter/provider system so additional sources can be added without modifying core application logic.

Where appropriate, support:

- JSON
- CSV
- REST APIs
- GeoJSON
- RSS/Atom
- public datasets
- publicly accessible OSINT APIs

Preserve source provenance wherever possible.

## Entity Model

Create a normalized entity model capable of representing objects such as:

- people
- organizations
- locations
- events
- domains
- IP addresses
- URLs
- documents
- infrastructure
- accounts/usernames
- identifiers
- other relevant intelligence entities

Relationships between entities must be first-class objects.

Relationships should support useful metadata such as:

- relationship type
- source
- timestamp
- confidence
- provenance
- attributes

## Entity Resolution

Provide mechanisms for:

- normalization
- deduplication
- alias handling
- canonical identifiers
- merging equivalent entities where appropriate

Avoid silently merging uncertain entities.

## Relationship Graph

Provide interactive entity-link analysis.

Users should be able to:

- explore entities and relationships
- expand neighboring entities
- inspect nodes and edges
- filter graph data
- search within the graph
- select entities
- view entity details
- navigate between connected objects
- distinguish relationship types
- trace provenance back to source data

The graph must operate on real persisted application data rather than static frontend examples.

## Geospatial Analysis

Provide map-based visualization for entities and events containing geographic information.

Support workflows such as:

- plotting entities
- plotting events
- map filtering
- marker clustering where appropriate
- selecting map objects
- opening related entity details
- linking map selections to other analytical views

Use an open-source mapping stack and freely usable map data.

## Timeline Analysis

Provide temporal analysis of events and timestamped relationships.

Users should be able to:

- inspect events chronologically
- filter by time range
- filter by entity or event type
- select timeline events
- navigate from events to related entities
- correlate timeline activity with graph and map data

## Unified Search

Implement a unified search experience across ingested intelligence data.

Search should support relevant combinations of:

- entities
- relationships
- events
- documents
- locations
- identifiers
- source records

Search results should allow users to immediately pivot into:

- graph analysis
- entity inspection
- map analysis
- timeline analysis

## Cross-View Analysis

Graph, map, timeline, search, and entity inspection must not behave as disconnected demonstrations.

The application should support analytical pivoting between views.

For example:

**Search → Entity → Graph → Related Event → Timeline → Location → Map**

Selections and filters should remain coherent where practical.

## Entity Detail View

Provide a useful entity-centric analysis surface containing relevant information such as:

- attributes
- aliases
- relationships
- associated events
- geographic information
- timeline activity
- source provenance
- identifiers
- related documents

## Provenance

OSINT data must retain useful information about where it came from.

Where possible, preserve:

- source name
- source URL
- ingestion timestamp
- original identifier
- original record
- transformation metadata

Analysts should be able to understand why a relationship or entity exists.

# UX Direction

Take inspiration from the interaction patterns common to professional intelligence-analysis platforms, but do not reproduce proprietary UI assets, branding, layouts, code, or copyrighted product-specific design elements.

The application should prioritize:

- information density
- fast analytical navigation
- clear entity inspection
- coordinated views
- keyboard-efficient workflows where useful
- dark-mode-friendly visualization
- clear distinction between data types
- understandable loading/error/empty states

Avoid building a superficial dashboard consisting only of unrelated charts.

The product should feel like an analytical workspace.

# Architecture

Choose an architecture appropriate for the requirements.

Prefer mature open-source technologies.

Keep clear boundaries between:

- frontend
- backend/API
- database/storage
- search
- graph representation
- ingestion
- enrichment
- OSINT connectors
- authentication
- analytical services

Do not introduce distributed infrastructure unless the expected workload justifies it.

Prefer a simpler architecture that works reliably over premature microservices.

# Security

Treat all ingested external data as untrusted.

Protect against issues including:

- XSS
- SQL injection
- command injection
- SSRF
- unsafe URL fetching
- path traversal
- malformed uploads
- unsafe deserialization
- arbitrary file execution
- prototype pollution
- insecure direct object references
- broken authorization
- secrets leakage

Sanitize or safely render external content.

Any server-side functionality that retrieves external URLs must include appropriate SSRF defenses.

Do not build functionality intended for unauthorized access, exploitation, credential theft, malware delivery, persistence, or destructive activity.

The platform is for analysis of legally accessible data.

# Testing and Verification

After every meaningful milestone:

1. Run relevant unit tests.
2. Run integration tests where applicable.
3. Run linting.
4. Run type checking.
5. Build the application.
6. Start the required services.
7. Test the affected functionality at runtime.
8. Review the implementation as an independent hostile reviewer.
9. Identify defects.
10. Fix them.
11. Re-run verification.
12. Continue to the next incomplete task.

Do not treat successful compilation as proof that a feature works.

# Gauntlet Review

During each review phase, actively search for:

- missing requirements
- incomplete implementations
- broken integrations
- dead code
- placeholder behavior
- fake/sample-only data flows
- UI elements without backend functionality
- unhandled errors
- race conditions
- invalid state transitions
- security vulnerabilities
- authorization problems
- data leakage
- regressions
- incorrect persistence
- data-model inconsistencies
- poor error handling
- performance bottlenecks
- accessibility problems
- responsive-layout failures
- production/development configuration mistakes

Be adversarial toward your own implementation.

Assume something is broken until verified.

# End-to-End Verification

Before declaring the project complete, explicitly verify full workflows for:

- data ingestion
- persistence
- entity normalization
- entity resolution
- relationship generation
- entity search
- unified search
- graph construction
- graph exploration
- geospatial visualization
- timeline analysis
- entity detail inspection
- provenance tracking
- cross-navigation between search, entity, graph, map, and timeline
- authentication and authorization where applicable
- loading states
- empty states
- error states
- malformed input
- realistic OSINT sample data
- application restart/persistence behavior
- production build

Use realistic sample datasets rather than relying solely on trivial fixtures.

# Completion Criteria

Never declare completion based solely on your own impression that the project looks finished.

The project is complete only when objective evidence shows that:

- every item in the implementation plan is complete;
- all required functionality from the original project brief exists;
- critical functionality works end-to-end;
- the frontend and backend are fully connected;
- required data persists correctly;
- relevant tests pass;
- linting passes;
- type checking passes;
- the production build succeeds;
- the application starts successfully;
- critical user workflows have been exercised at runtime;
- no known blocking bugs remain;
- no required TODOs remain;
- no placeholder implementations remain;
- no fake production integrations remain;
- no significant security issue discovered during review remains unresolved;
- development-only configuration has not accidentally leaked into the production configuration.

Once implementation appears complete, perform one final full-project Gauntlet review from scratch.

Do not assume previous checks were sufficient.

Re-test the system as a complete product.

If any failure or missing requirement is discovered:

**FIX → RE-TEST → RE-REVIEW**

Continue the loop.

Only when every completion criterion is satisfied may you stop.

Your final completion marker must be:

`PROJECT_COMPLETE`
