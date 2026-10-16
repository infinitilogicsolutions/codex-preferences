# DonDraper Workspace: Onboarding and Execution Guide

**Owner:** Santosh Gitay  
**Version:** 1.0  
**Date:** October 16, 2026  
**Companion:** [DonDraper + Avengers instructions](AGENTS.md)

## Purpose

Give a developer one repository to hand to a coding agent. DonDraper owns workspace setup, repository discovery, requirements analysis, planning, implementation coordination, and verification across an application with five separate repositories.

The developer supplies the workspace location and selected user story. DonDraper handles the preparation, presents a plan, and executes after approval.

This is a specification for the workspace workflow. Adding this document does not install a bootstrap script, connect application repositories, or start agents. The five application repository URLs, actual commands, and requirements must be supplied when implementing the workspace.

## Core decisions

- DonDraper is the accountable orchestrator for all five steps below.
- Avengers perform bounded subtasks; DonDraper retains overall ownership.
- The feature PRD covers the application across five repositories.
- Implement one selected user story at a time, normally within one application repository.
- Parallelize independent tasks inside that approved story; sequence dependencies.
- Keep each application repository's history, user branches, PRs, and existing CI/CD.
- Keep application source in its original repositories. The workspace stores coordination material and references, not duplicate source copies.
- Obtain developer approval of the summarized implementation plan before coding.
- Keep Claude's independent review and the existing human developer and QA gates.

## What the developer needs

A coding-agent environment with Git, terminal and filesystem access, the ability to run project commands, and authorized access to the five repositories. An ordinary chat session with only a repository link cannot be assumed to have these capabilities.

The workspace does not grant permissions. Use the developer's approved authentication; never commit credentials, tokens, or environment secrets.

Tool environments differ. Read the explicit entry point and applicable repository instructions; do not assume every agent automatically discovers the same instruction files or supports subagents and model selection.

## Proposed workspace contents

These paths describe the intended layout; they are not a claim that the files already exist.

| Path | Purpose |
| --- | --- |
| `README.md` | Developer entry point, setup usage, and task invocation. |
| `AGENTS.md` | DonDraper and Avengers operating instructions. |
| `CLAUDE.md` | Small Claude-specific entry point pointing to shared guidance. |
| `DONDRAPER-WORKSPACE.md` | This workflow and responsibility guide. |
| `workspace/repos.json` | The five repositories, checkout locations, baseline refs, ownership, prerequisites, and build/test instructions. |
| `workspace/architecture.md` | Service roles, dependencies, APIs, event contracts, and business-flow links. |
| `scripts/bootstrap.*` | Repeatable setup for the team's supported development environments. |
| `requirements/<feature>/` | BRD, feature PRD, repository-local user stories, and acceptance criteria. |
| `runs/<feature>/<story>/` | Approved plan, status, findings, responses, and verification records. |
| `application.code-workspace` | Optional IDE configuration opening the relevant folders. |
| `repos/` | Local checkouts of the five application repositories, excluded from the workspace's Git tracking. |

Track references to application commits in the workspace, not the application source itself. Keep secrets and sensitive run output out of tracked records. If requirements are access-restricted, keep them in an appropriately restricted workspace or reference their authorized source rather than publishing copies.

### Repository manifest requirements

For each of the five repositories, record:

- A stable identifier, purpose, real clone URL, and expected local path.
- The approved base branch or ref; never guess it is `main`.
- Runtime and tooling prerequisites.
- Documented dependency-installation, build, startup, and test commands.
- Relevant repository-specific instructions and authoritative API/event contracts.
- Dependencies on other application repositories.
- Configuration requirements using variable names and templates, without secret values.

Keep these facts current and resolve documentation conflicts before implementation.

## The five steps DonDraper owns

### Step 1: Validate access and prerequisites

DonDraper reads this guide, the shared instructions, and the repository manifest. It checks the available tools, runtimes, repository access, selected story, and requirements locations.

Identify missing configuration or conflicting requirements explicitly. Ask only for information needed to continue; do not invent repository locations or claim access without checking.

**Output:** A readiness result listing successful checks and concrete blockers.

### Step 2: Set up repositories and the working branch

DonDraper runs the approved bootstrap process to clone missing repositories and validate existing checkouts.

Setup must be repeatable. Check each existing directory's remote and worktree state before acting. Preserve uncommitted work and never reset, overwrite, or switch an occupied checkout without an appropriate resolution.

For the selected story, create or select the developer's story branch from the approved base in that repository. Record the exact commits of all five repositories used as the baseline. Other repositories provide context unless the approved scope requires changes.

Cloning the workspace alone does not clone the application repositories in this script-based design; the bootstrap step performs that work. Git submodules are an alternative, not a requirement.

**Output:** A usable local workspace, identified story branch, and recorded repository baseline.

### Step 3: Load architecture, requirements, and story context

DonDraper reads the BRD, feature PRD, selected story, acceptance criteria, architecture map, applicable repository instructions, and relevant source/contracts across the five repositories.

Start with the map and retrieve source as needed. Do not load all code into one prompt by default. Cross-repository search is useful; embeddings or a vector database are optional and are not prerequisites.

Confirm the selected story's repository and dependencies. If the story cannot be implemented within its declared scope, raise that discrepancy rather than silently expanding work.

**Output:** A grounded interpretation of the requested behavior and affected implementation.

### Step 4: Summarize the plan for approval

DonDraper presents:

1. Its understanding of the feature intent and selected story.
2. The repository and implementation areas to change.
3. Required dependencies, contracts, and sequencing.
4. Proposed Avenger assignments and ownership, when useful.
5. Acceptance-criteria coverage and verification steps.
6. Open questions, assumptions, and material limitations.

The developer approves or corrects the plan. Setup and investigation can precede this approval; implementation of the story begins after it.

**Output:** An approved, recorded implementation plan.

### Step 5: Execute, integrate, verify, and report

DonDraper executes the approved plan and delegates suitable independent tasks using the assignment contract in AGENTS.md: objective, scope, inputs, constraints, acceptance criteria, verification, and evidence.

Use Beastmode to start unblocked work promptly, avoid overlapping edits, and minimize redundant investigation. Faster completion means reducing time to a verified result, including integration and review.

Santosh's practice is to use the strongest available model for lead reasoning and lower-tier workers for suitable bounded tasks. The current AGENTS.md specifies the preferred Avenger configuration. Respect actual tool capabilities and disclose unavailable configurations; do not pretend delegation happened.

DonDraper integrates results, inspects changes, runs relevant checks, and independently verifies important behavior. A worker's completion statement is not proof.

Update the run's status and summarize changes, test evidence, unresolved findings, and next steps. Repeated failures or unresolved scope questions return to the developer.

**Output:** An implemented story with evidence, ready for independent review. This is not a claim that the feature has merged, passed QA, or deployed.

## Developer entry point

Example instruction to a capable coding agent:

> Act as DonDraper. Read AGENTS.md and DONDRAPER-WORKSPACE.md. Initialize the workspace for the specified feature and story. Validate access, check out the five repositories safely, read the feature requirements and applicable repository instructions, and summarize the implementation and verification plan. Wait for my plan approval before coding. Execute the approved plan using Avengers where supported and useful, then update status with evidence and escalate repeated failures.

The workspace configuration must resolve the real feature and story references; the developer should not need to re-explain the architecture in every session.

## Existing end-to-end delivery process

The five steps fit inside Santosh's established process:

1. **Business definition:** Product/business supplies the idea and BRD.
2. **Feature specification:** Santosh creates a feature-level PRD with context for all five repositories, user stories, and acceptance criteria.
3. **Stakeholder agreement:** The VP and QA teams review the PRD before execution starts.
4. **Story selection:** Santosh chooses one repository-local story at a time.
5. **DonDraper workflow:** Complete the five steps above, including developer approval of the plan.
6. **Independent Claude review:** Claude reads the BRD, PRD, and actual changes with context across all five repositories. It does not rely solely on Codex's summary or status.
7. **Correction loop:** Santosh passes Claude's gaps to Codex and Codex's responses/fixes back to Claude. Repeat as needed; repeated failures escalate to Santosh. This transfer is currently human-mediated.
8. **Feature integration:** After the feature's coding is complete, Santosh tests the repositories together locally before pushing; testing information is reflected in individual repository PRs.
9. **Human review:** Changes go through individual repository PRs with at least two human reviewers before merge.
10. **QA:** After merge, a separate QA team performs manual, functional, and automated testing and checks acceptance-criteria coverage.

A completion summary or agreement between agents does not replace the human review and QA stages.

## Branches, Git ownership, and CI/CD

Each cloned application directory remains its own Git repository.

- BFF changes are committed and pushed on the developer's branch in the BFF repository.
- UI changes belong to the UI repository when authorized by the selected story or an explicitly approved scope adjustment.
- Workspace documentation changes belong to the workspace repository.
- Application PRs and pipeline triggers remain in their original repositories.
- An ordinary commit or push from the workspace root does not commit or push changes in the five checkouts.
- Bootstrap should not modify application pipeline definitions merely to provide shared context.

Record the relationship between feature, story, branch, and PR. Preserve the team's existing permissions and required review/check policies.

## Agreed addition: reproduce the tested combination

For feature-level integration testing, record all five repository commit SHAs together, along with test-run references and manual results. If code changes afterward, record the new revisions and repeat affected verification.

This was agreed as an improvement during the discussion, rather than described as an existing automated control.

Suggested run records include the PRD version, selected story, approved plan, baseline and resulting commits, findings, resolutions, verification evidence, PR references, and unresolved issues. Exact filenames and schemas can be chosen during implementation.

## Optional follow-on improvements

These are recommendations, not changes already implemented by this document:

- Assign stable finding IDs and distinguish proposed fixes from reviewer-verified closure.
- Link acceptance criteria to test evidence, including failures, retries, duplicates, partial failures, and authorization where relevant.
- Confirm compatibility during staggered deployment and document rollback/migration handling.
- Track defects, rework, review cycles, elapsed time, and agent cost before adding more agents.
- Automate the Codex-Claude handoff only when useful, retaining human involvement for scope and disagreements.
- Move execution to dedicated workers if jobs must run independently of developer laptops.

## Implementation readiness checklist

Before calling the workspace operational:

- [ ] Supply the five real repository entries and approved baseline refs.
- [ ] Add and verify bootstrap behavior, including safe repeat runs.
- [ ] Provide architecture, contracts, prerequisites, and real project commands.
- [ ] Link the approved BRD, PRD, user stories, and acceptance criteria.
- [ ] Add README and agent-specific entry points referencing shared instructions.
- [ ] Verify branch isolation and original-repository Git ownership.
- [ ] Exercise one story through setup, plan approval, execution, and review.
- [ ] Confirm that existing PR/CI/CD behavior is preserved.
- [ ] Record the tested repository combination and verification evidence.

DonDraper owns coordination from the initial setup request through verified implementation and review handoff. Developers retain approval of the plan, resolution of escalations, and the existing delivery gates.
