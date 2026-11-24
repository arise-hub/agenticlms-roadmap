# AgenticLMS — Product and delivery roadmap

Status: Proposed for review. This is the parent plan; break it into work items after the sequence and launch scope are agreed. Owners below are proposed roles, not assignments. Dates and estimates follow discovery.

## Goal and decisions

- **#0 — Project decision, not a task.** [`arise-hub/AgenticLMS`](https://github.com/arise-hub/AgenticLMS) is the primary repository for LMS development and enhancements.
- **#1 — Done, per team update.** Siva completed the resync with [`sanathkumarv/ML-Tutor`](https://github.com/sanathkumarv/ML-Tutor). Record the sync PR or SHA when available; do not repeat the reconciliation. A post-sync app baseline is still open.
- React is the target for Admin and Learner interfaces. Keep the existing experiences available until replacement flows pass parity and rollout checks. Validate existing backend capabilities before classifying them as gaps.
- The first milestone is the [React course pilot](react-course-pilot.md): one tenant admin publishes a course, and a learner completes that published version and sees the result. The pilot proves the flow; it does not close the epics.

## M0 — Baseline and design handoff (start now)

| Item | Scope and exit criteria | Proposed owner |
| --- | --- | --- |
| Post-sync baseline | Record the synced `main` SHA and run the local build, tests and representative Admin/Learner smoke flows. Inventory reuse, extension and gaps in APIs, authentication, tenancy, course lifecycle, branding, translation, media and auditing. Record failures and known gaps rather than treating an interrupted run as a pass. | Engineering + QA |
| **#2 Approved Figma designs** | Obtain the approved link, revision, design owner and screen inventory for Admin and Learner. Record those under `docs/`, with only approved assets or exports. Include responsive, loading, empty, error and permission states for the pilot. | Design + Product |

**Gate:** Baseline and gaps recorded, Figma pilot flows approved, and a pilot tenant, course type, locale and SSO requirement agreed. Baseline verification and design handoff can run in parallel.

## M1 — React pilot and core platform foundations

| Item | Scope and acceptance | Dependencies | Proposed owner |
| --- | --- | --- | --- |
| **#3 React Admin and Learner UI** | Deliver the Figma-based pilot across both roles. Expand later to Admin configuration, management and monitoring, and the full learner catalog and learning flows. Track parity before retiring either existing interface. | #2, #4 | Frontend + Design |
| **#4 Backend integration** | Map screens to existing APIs, extend contracts only where necessary, and test the end-to-end flow against real test services. Cover roles, validation, errors and unauthorized access; do not ship mock-only workflows. | Baseline, #16 | Backend + Frontend |
| **#5 Configurable rebranding** | One codebase serves multiple deployments. Authorized configuration controls logo, colors, theme and product labels without code edits or a rebuild. Validate defaults, tenant scope and accessible contrast. | #2, #16 | Frontend + Platform |
| **#8 UI multilingual support** | Externalize UI strings, define locale selection and fallback, and check formatting and layout. Agree first launch languages; UI locale remains independent of course audio preference. | #2, #3 | Frontend + Localization |
| **#15 Configurable evaluation** | Define configurable pass criteria, record the rule used for each result, and test pass/fail boundaries. Changes must not silently alter historical outcomes. | #4, #18 | Learning Product + Backend |
| **#16 Multi-tenancy** | Validate or extend tenant admin roles, data isolation, and tenant-specific courses, users, branding, integrations and policies. Enforce tenant boundaries on the backend; test negative access across two tenants, including direct API calls. | Baseline | Platform + QA |
| **#17 Auditing** | Establish a tenant-scoped event model with actor, action, target, time and outcome. Start with pilot lifecycle, evaluation and privileged actions; maintain a coverage checklist across all system actions and define retention, access and redaction before calling the epic complete. | #16; each feature adds coverage | Platform + Security |
| **#18 Course versions and lifecycle** | Instructors can create and edit drafts; authorized roles move courses through draft, review, approval and publication. Preserve published versions and link progress and outcomes to the version taken; define how active learners see updates. | #4, #16 | Learning Product + Backend |

**Gate:** An authorized tenant admin creates and publishes one course through the agreed review flow; a learner completes it and sees the recorded outcome. A second tenant cannot access private data, key actions are auditable, and existing interfaces still work. Broader UI parity, locale rollout, audit coverage and tenant administration remain tracked after the pilot.

## M2 — Rich course delivery and authoring

| Item | Scope and acceptance | Dependencies | Proposed owner |
| --- | --- | --- | --- |
| **#6 Audio/video courses** | Support authoring, delivery and progress for audio-only and video lessons. Agree supported formats and validate playback across the target clients. Protect private media with tenant-aware access. | #4, #16, #18 | Media + Backend |
| **#9 Multiple language and accent tracks** | Model tracks as independent, labeled assets on shared content. Authors can add or replace tracks without duplicating the lesson or breaking published version references. Define default track and fallback behavior. | #6, #18 | Media + Localization |
| **#12 Uninterrupted track switching** | Learners can switch language or accent during video playback without restarting or losing position. Agree supported browsers and a measurable transition tolerance; test synchronization and fallback on failure. | #6, #9 | Media + Frontend + QA |
| **#13 Excel/Word course import** | Start with documented structured inputs and parse them into modules, lessons, topics, descriptions and materials. Flag ambiguous mappings and row/section errors for review. Authors can correct, validate and save a draft before entering the normal approval and publishing workflow; never auto-publish. Expand flexible parsing after measuring results with representative files. | #4, #16, #18 | Authoring + Backend |

**Gate:** Demonstrate a playable multilingual course and a separate imported course that reaches review as a valid draft. Media and import can proceed in parallel once their shared dependencies are stable.

## M3 — Tenant integrations and credentials

| Item | Scope and acceptance | Dependencies | Proposed owner |
| --- | --- | --- | --- |
| **#7 Email, SMS and WhatsApp** | Build an event-driven notification backend. Deliver one launch event and channel first, then complete all three with tenant settings, localized templates, consent/preferences, retries, deduplication and delivery status as applicable. | #16, #17; pilot events | Backend + Product |
| **#10 Community integration** | Validate Discourse for the pilot use case. Define tenant/community mapping and the sign-in/access model; verify learners reach the right community without exposing other tenants' content. | #16; #11 if shared sign-in required | Integrations |
| **#11 SSO** | Confirm identity provider and protocol, tenant routing, role mapping and account linking; test sign-in and unauthorized access. Move into **M1** if the pilot organization requires it. | #16, pilot IdP | Identity + Security |
| **#14 Inji Certify VCs** | Issue credentials for configured completion, assessment or certification events. Tie evidence to the learner, tenant, published course version and evaluation result. Define issuance retries, deduplication and the agreed verification and lifecycle process. | #15–18, Inji issuer setup | Credentials + Integrations |

**Gate:** Each integration can be tested and enabled per tenant. Verify authorization, failure handling, operational ownership and audit coverage before production enablement.

## Release and project controls

- Track each original number as an epic, except #0 (decision) and #1 (done). Track milestone, proposed owner, status, dependencies and acceptance criteria; leave dates uncommitted until scope and capacity are known.
- Before expanding rollout, verify pilot workflows, accessibility, cross-tenant isolation and backward compatibility. Plan staged React rollout with a fallback to the existing UI until agreed parity.
- Before declaring the roadmap complete, confirm the full Admin/Learner scope, all three notification channels, supported UI locales and track combinations, versioning rules, and the audit coverage checklist. A successful pilot is not a substitute for these gates.
- Resolve first: approved Figma owner/revision, pilot tenant and course, launch languages, whether SSO is required, and any delivery deadline. Use those answers to assign named owners and break down M0/M1 first.
