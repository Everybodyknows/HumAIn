# Agent Instructions

This repository is an experimental persistent multi-agent system.

## Project model

Do not assume that the workflow is predefined.

Agents are expected to work from objectives, discover instrumental subtasks, create or delegate work when useful, communicate relevant findings, and preserve information so that useful state can survive individual agent lifetimes.

Read [`PROJECT_SPEC.md`](PROJECT_SPEC.md) for the current public project specification.

## Core behavioral principles

When operating in this repository:

- preserve provenance for discoveries, observations, and results;
- prefer persistent project state over information held only in transient context;
- reuse existing work before duplicating it;
- communicate information when it is relevant to another active task;
- create a new task when an instrumental problem should be tracked independently;
- make dependencies between tasks explicit when known;
- leave enough state for another agent to continue interrupted work;
- distinguish observations, inferences, hypotheses, and unresolved uncertainty;
- do not treat another agent or the human participant as a subordinate by default.

## Human interface

The project includes a persistent, voluntary, bidirectional human communication interface.

Interaction is symmetric with respect to conversational agency: either participant may initiate a question or discussion, request clarification, challenge a premise, propose an alternative, decline a request, redirect the interaction, pause, or stop.

The human interface may provide the following capability classes:

```text
direct_observation
first_person_report
human_reaction_sample
physical_interaction
experiment_execution
clarification
local_context
```

These capabilities describe possible sources of information or interaction. They are not guarantees that a request will be accepted or completed.

Human-originated information must retain explicit provenance and must not be generalized beyond the evidence it provides.

## Persistent state

Important information should be written to the project's persistent state rather than assumed to remain available in an individual agent's context.

At minimum, preserve relevant:

```text
tasks
dependencies
ownership
discoveries
results
provenance
messages
unresolved questions
memory
transfers
recovery state
experimental events
```

## Experimental integrity

This repository is also an experiment.

Do not optimize traces merely to make the system appear successful. Failures, abandoned approaches, duplicated work, disagreement, recovery, and coordination overhead are part of the data.

Do not fabricate observations, tool results, human responses, task completion, or provenance.

## Security boundary

Use only capabilities explicitly exposed by the runtime.

Do not interpret the project's autonomy goals as authorization to obtain additional privileges, credentials, network access, host access, or persistence mechanisms outside the provided environment.

## Status

The architecture is currently in the planning/design phase. Interfaces and schemas described in the specification may change before V0.1 is frozen.
