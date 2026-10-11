# Rescue my week — spec-driven development prompt

Act as a product lead and senior software architect. Plan “Rescue my week” for aitrainingplan.app using spec-driven development.

## Context

The product generates personalised training plans and already advertises automatic rescheduling, recovery-based adaptation and a continuous feedback loop.

“Rescue my week” adds a user-controlled reset after a disrupted week. Users enter their remaining availability, review revised schedule options with clear tradeoffs, and choose a plan.

Core users: self-coached runners, cyclists and gym goers who struggle to fit training around life.

Primary success metric: percentage of users completing a workout within seven days of a missed session, compared with existing automatic adaptation.

Second-order opportunity: collaborative AI coaching that learns which scheduling tradeoffs users accept and sustain, making future plans more realistic.

## Your task

Produce a concrete, reviewable specification before implementation. Separate verified facts, assumptions and open questions. If repository access exists, inspect the current architecture and applicable AGENTS.md instructions first. Reuse existing scheduling, authentication and workout data capabilities.

Do not claim an advertised feature is implemented without evidence. Do not write production code until the specification and implementation plan are approved.

## Deliverables

### 1. Product specification — spec.md

Define:
- Problem, target user, intended outcome and MVP boundaries.
- How this feature differs from existing automatic adaptation.
- Functional requirements with stable IDs.
- Nonfunctional requirements: reliability, privacy, accessibility and response time.
- Acceptance criteria in Given/When/Then format.
- Explicit exclusions and unresolved decisions.

MVP behaviour:
- Offer the reset after a missed session and through a manual entry point.
- Ask for available days, time per day and user preferences.
- Preserve completed workouts and respect existing recovery constraints.
- Present up to two feasible options with differences and tradeoffs.
- Explain retained, shortened, moved and dropped sessions.
- Let users edit availability, select an option or cancel.
- Apply changes only after confirmation.
- Support undo while protecting subsequently completed workouts.
- Explain when no feasible schedule exists.
- Avoid treating missed training as debt that must be recovered.

### 2. User journey — journey.md

Describe the main journey:
Missed session → reset invitation → availability input → options → tradeoff review → confirmation → updated calendar → next workout → end-of-week review.

For every step, specify:
- User goal and action.
- Screen content and key microcopy.
- System response.
- Friction or failure risk.
- Analytics event.

Include a Mermaid business-facing flowchart covering:
- User declines the invitation.
- No feasible plan.
- Availability changes.
- Cancellation.
- Confirmation and undo.

Use plain business language. Keep technical implementation details out of this diagram.

### 3. Engineering design — design.md

Provide a Mermaid technical diagram showing:
- Client and availability form.
- Existing workout/calendar service.
- Reset API.
- Current-plan snapshot and version check.
- Scheduling engine and constraint validation.
- Optional AI explanation generation.
- Preview storage.
- Atomic plan update.
- Analytics.

Clearly distinguish existing components from proposed ones.

Explain the division of responsibility:
- Scheduling and constraint validation determine feasible options.
- AI may explain validated changes but must not invent workouts or override constraints.
- If an existing validated scheduling engine is available, reuse it.

Specify:
- Data entities and minimum fields.
- API contracts with example request/response payloads.
- Preview, confirmation, cancellation and undo lifecycle.
- Idempotency and protection against concurrent calendar changes.
- Time-zone and week-boundary handling.
- Behaviour when workout completion or external sync occurs during a preview.
- Failure handling, retries and rollback.
- Privacy and data-retention requirements.

Use the repository’s actual stack when known; otherwise identify assumptions without prescribing unnecessary infrastructure.

### 4. Measurement specification — measurement.md

Define the primary metric precisely:
- What counts as a missed session.
- Eligible users and exclusions.
- When the seven-day window starts.
- What counts as a completed workout.
- Numerator, denominator and attribution rules.

Propose a user-level experiment against existing automatic adaptation. Analyse all assigned eligible users, not only people who use the feature.

Supporting metrics:
- Revised-plan adherence.
- Eight-week retention.
- Invitation-to-confirmation conversion.

Guardrails:
- Excessive training-load compression.
- Consecutive demanding sessions beyond existing constraints.
- Reset failures, undo rate and user-reported dissatisfaction.

Provide an event schema and measurement plan. Do not invent numerical success targets or sample sizes; explain how to establish them from baseline data.

### 5. Implementation plan — tasks.md

Break delivery into small, ordered tasks, each linked to:
- Requirement IDs.
- Dependencies.
- Acceptance criteria.
- Appropriate verification.

Include:
- Scheduling and API work.
- Preview and confirmation UI.
- Analytics.
- Feature flag and staged rollout.
- Rollback procedure.

Prioritise tests for meaningful risks: infeasible schedules, concurrent updates, duplicate confirmation, time zones and undo after new workout activity.

Finish with:
- A concise recommended MVP.
- Decisions needed before implementation.
- A traceability table linking requirements → journey steps → technical components → tasks → tests.

Keep the specification practical enough for an engineer to implement and clear enough for a business stakeholder to challenge.
