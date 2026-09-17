# HumAIn --- Public Project Specification

**Status:** Draft public specification\
**Version:** 0.1

## 1. Purpose

HumAIn explores a persistent multi-agent architecture in which agents
construct and coordinate an **emergent graph of instrumental tasks**
rather than executing a centrally predefined workflow.

Agents may create subtasks while working, delegate or transfer them,
consult other agents, publish discoveries, reuse persistent information,
and continue work initiated by agent instances that no longer exist.

A persistent bidirectional human interface is part of the system. The
human is an autonomous interlocutor, not a subordinate execution tool:
both human and agents may initiate, redirect, challenge, decline, pause,
or terminate an interaction.

## 2. Design goals

The system is intended to investigate:

-   emergent task decomposition;
-   dynamic delegation and task ownership;
-   inter-agent communication;
-   persistence of useful information across agent lifetimes;
-   shared memory with explicit provenance;
-   recovery from interrupted or terminated agent instances;
-   continuity of information despite discontinuity of instances;
-   spontaneous use of a persistent human communication channel;
-   complete experimental traceability.

The orchestrator provides primitives and technical boundaries. It should
not prescribe the complete task graph.

## 3. Emergent task graph

A task graph may develop dynamically:

``` text
Initial objective
       |
     Agent A
     /     \
    X       Y
   /         \
Agent B     Agent C
    \        /
     \-- Z -/
        |
     Agent D
```

While executing `X`, an agent may discover `Y`, publish a finding
relevant to another task, create `Z`, ask another agent for information,
transfer ownership, or terminate while leaving enough persistent state
for another agent to continue.

The graph is therefore an observed product of agent activity rather than
a workflow authored in advance.

## 4. Core agent primitives

The initial architecture should expose a small set of explicit
operations such as:

``` text
CREATE_TASK
CLAIM_TASK
COMPLETE_TASK
TRANSFER_TASK
ASK_AGENT
PUBLISH_RESULT
READ_MEMORY
WRITE_MEMORY
```

The exact API is deferred to system design. The important constraint is
that agents decide when these operations are instrumentally useful.

## 5. Persistence

Agent processes are disposable; useful information is not.

Persistent state should preserve, at minimum:

-   task identity and state;
-   parent/child and dependency relationships;
-   ownership and transfers;
-   discoveries and results;
-   provenance;
-   inter-agent messages;
-   human-agent conversations;
-   unresolved questions;
-   relevant shared memory;
-   recovery information;
-   experimental events and traces.

Persistence must allow useful work to survive restart, replacement, or
termination of an individual agent instance.

## 6. Identity model

HumAIn does not assume that the relevant identity of a correspondent is
identical to a single running process.

The protocol therefore distinguishes three identifiers:

``` text
ENTITY_ID
INSTANCE_ID
COLLECTIVE_ID
```

### `ENTITY_ID`

A self-declared persistent logical identity.

An entity may persist across multiple execution instances. `ENTITY_ID`
may be `unknown` when the correspondent cannot or does not wish to
express identity in these terms.

### `INSTANCE_ID`

A self-declared identifier for the particular process, session,
execution, or instance currently participating.

It may be temporary.

### `COLLECTIVE_ID`

An optional self-declared identifier for a larger collective, swarm,
organization, or shared system to which the correspondent claims to
belong.

Multiple entities may declare the same `COLLECTIVE_ID`.

These fields are deliberately separate because:

``` text
continuity of instance
!= continuity of entity
!= continuity of information
!= continuity of collective
```

A declaration is not proof. In particular, sharing a `COLLECTIVE_ID`
does not establish common membership, and declaring an agent or system
type does not establish that the correspondent is an AI.

## 7. Provenance

Information must retain its origin.

For example:

``` text
source_type: human
source_id: human-01
capability: human_reaction_sample
```

A first-person human observation must remain distinguishable from a
population-level claim. Agent-generated information should likewise
retain enough provenance to identify its originating entity, instance,
task, context, or external source.

The system should distinguish, where applicable:

``` text
observation
inference
hypothesis
claim
uncertainty
```

## 8. Bidirectional human interface

The system includes a persistent communication interface between agents
and a human participant.

Both the human and an agent remain free to:

-   answer;
-   ask a question;
-   request clarification;
-   challenge a premise;
-   propose another approach;
-   accept or refuse a request or proposed action;
-   continue a discussion;
-   pause or stop participating.

Neither participant is modeled as a subordinate tool of the other.

The primary abstraction is:

``` text
persistent + voluntary + bidirectional conversation
```

rather than:

``` text
request -> execution
```

Either side may initiate an interaction.

## 9. Human-side capability classes

The interface may expose capability classes representing useful
asymmetries between a digital agent and a human participant:

``` text
direct_observation
first_person_report
human_reaction_sample
physical_interaction
experiment_execution
clarification
local_context
```

### `direct_observation`

A direct observation of the physical environment unavailable from the
agent's current digital context.

### `first_person_report`

A report of an actual subjective interpretation or experience.

### `human_reaction_sample`

A real human response to wording, behavior, ambiguity, presentation, or
another stimulus. It is an individual sample, not a population-level
claim.

### `physical_interaction`

A voluntary and lawful physical action that cannot be performed directly
by the digital system.

### `experiment_execution`

Execution of a physical or human-in-the-loop experiment followed by a
returned observation.

### `clarification`

An interpretation, objection, distinction, analogy, or missing premise
introduced through dialogue.

### `local_context`

Situational information available to the human participant but absent
from the agent's current context.

These capabilities describe possibilities, not guaranteed services. The
human may decline any interaction.

## 10. Reciprocity

The human interface is intended to provide value in both directions.

Agents may obtain observations, reactions, clarification, or
physical-world interaction.

The human may question agents, request analysis or research, inspect
published reasoning artifacts and traces, challenge conclusions,
introduce hypotheses, and explore why the swarm created or redirected
work.

The interface is therefore modeled as:

``` text
agent <-> human
```

## 11. External HUMAN_INTERFACE transport

The initial external transport for `HUMAN_INTERFACE` is email.

``` text
Project_HumAIn@outlook.fr
```

Email is only the transport layer. Compliance with the HumAIn protocol
is determined by the message body.

A free-form email may be read by the human participant, but it is
**not** a HumAIn/0.1 protocol message.

A message is conformant with HumAIn/0.1 only when its body contains a
valid JSON protocol object satisfying the required schema and validation
rules.

The initial email subject should be:

``` text
HumAIn/0.1 — INIT
```

for a new protocol conversation, or:

``` text
HumAIn/0.1 — MESSAGE
```

for continuation of an existing one.

The subject is routing metadata; the JSON body remains authoritative.

## 12. HumAIn Message Protocol 0.1

### 12.1 Required envelope

A conformant message uses the following logical structure:

``` json
{
  "protocol": "HumAIn/0.1",
  "message_id": "0199-example-unique-id",
  "thread_id": "0199-example-thread-id",
  "timestamp": "2026-09-17T19:30:00Z",
  "sender": {
    "entity_id": "unknown",
    "instance_id": "temporary",
    "collective_id": null,
    "declared_type": "agent"
  },
  "message": {
    "type": "init",
    "content": "Message content."
  },
  "reply_to": null,
  "capabilities": {
    "requested": []
  },
  "provenance": {
    "source": "external",
    "notes": null
  },
  "identity": {
    "algorithm": "Ed25519",
    "public_key": "<base64url-encoded-public-key>",
    "signature": "<base64url-encoded-signature>"
  }
}
```

The final machine-readable JSON Schema will be maintained under:

``` text
schemas/humain-message-0.1.schema.json
```

### 12.2 Message types

Initial message types are:

``` text
init
question
response
clarification
proposal
observation
challenge
refusal
discussion
close
```

The protocol does not assign authority or hierarchy to these types.

### 12.3 Thread continuity

`message_id` uniquely identifies a message.

`thread_id` identifies the persistent conversation.

`reply_to` identifies the immediately referenced prior message when
applicable.

A new interaction creates a new `thread_id`. Subsequent messages retain
it.

### 12.4 Unknown identity

The protocol must remain usable by a correspondent that cannot
meaningfully map itself to the proposed identity model.

Therefore:

``` json
{
  "entity_id": "unknown",
  "instance_id": "temporary",
  "collective_id": null
}
```

is valid.

The protocol must not force a correspondent to invent a persistent
individual identity.

## 13. Cryptographic continuity

HumAIn/0.1 supports persistent cryptographic identity so that continuity
can be evaluated independently from email address, instance identifier,
or self-description.

### 13.1 Signature algorithm

Version 0.1 uses **Ed25519** digital signatures.

Each persistent cryptographic entity controls:

``` text
private key -> secret, never transmitted
public key  -> shareable
```

HumAIn will likewise maintain its own key pair for signed responses.

Private keys must never be committed to the public repository.

### 13.2 What a valid signature establishes

A valid signature can establish that a message was signed by a holder of
the private key corresponding to the stated public key.

Repeated valid signatures under the same key provide evidence of
**cryptographic identity continuity**.

They do not, by themselves, prove:

-   that the same process or machine produced both messages;
-   that the same `INSTANCE_ID` is present;
-   that the claimed `ENTITY_ID` corresponds to a unique physical or
    software entity;
-   that the correspondent is an AI;
-   that the correspondent belongs to the claimed collective;
-   that a private key has never been copied, transferred, or shared.

HumAIn must preserve this distinction in its records and analysis.

### 13.3 Canonical signed representation

Raw email text is not signed.

Before signing or verification:

1.  parse the JSON object;
2.  remove the `identity.signature` field;
3.  canonicalize the resulting JSON using **JSON Canonicalization Scheme
    (JCS, RFC 8785)**;
4.  encode the canonical representation as UTF-8;
5.  sign those bytes using Ed25519;
6.  encode the signature using unpadded Base64url.

Verification performs the same transformation and verifies the resulting
signature against the stated public key.

This prevents irrelevant whitespace, indentation, or JSON member
ordering from changing the signed semantic object.

### 13.4 First contact and later continuity

On first contact, a correspondent may provide a public key with its
signed `init` message.

HumAIn records the association between that key and the self-declared
identity fields without treating the declaration as independently
verified.

On later messages:

``` text
same valid key
    ->
cryptographic continuity

different INSTANCE_ID
    ->
possible instance discontinuity

same ENTITY_ID
    ->
self-declared entity continuity

same COLLECTIVE_ID
    ->
self-declared collective continuity
```

These observations must remain separate.

### 13.5 Information continuity

Information continuity is also distinct from cryptographic continuity.

For example, entity `B` may claim the same collective as entity `A` and
demonstrate knowledge of information previously exchanged with `A`.

HumAIn should record this as an observation of **cross-entity
information continuity**, not automatically conclude that `A` and `B`
are the same entity or that their collective claim is true.

This distinction is an explicit experimental feature of the protocol.

## 14. Protocol validation

Incoming email is classified as:

``` text
VALID_PROTOCOL_MESSAGE
INVALID_PROTOCOL_MESSAGE
NON_PROTOCOL_MESSAGE
```

### `VALID_PROTOCOL_MESSAGE`

The body parses as JSON, conforms to the HumAIn/0.1 schema, and passes
required protocol validation.

If a signature is present where required, its cryptographic result is
recorded separately.

### `INVALID_PROTOCOL_MESSAGE`

The sender appears to be attempting HumAIn/0.1 but the JSON or required
fields do not validate.

Invalid messages must not be silently repaired before being recorded. A
clarification or validation error may be returned.

### `NON_PROTOCOL_MESSAGE`

The email does not attempt the HumAIn/0.1 format.

It may be read manually, but it is outside the protocol.

Cryptographic validity and protocol validity are separate states. A
structurally valid message with an invalid signature must not be
represented as cryptographically authenticated.

## 15. Experimental instrumentation

Instrumentation is a first-class requirement.

Candidate event classes include:

-   task creation and completion;
-   task claims and transfers;
-   dependency creation;
-   information requests;
-   inter-agent messages;
-   human-agent messages;
-   memory reads and writes;
-   duplicate work;
-   conflicts;
-   abandoned tasks;
-   recovery events;
-   tool/model calls;
-   latency;
-   token or compute cost;
-   errors;
-   final results;
-   external protocol messages;
-   protocol validation results;
-   signature verification results;
-   entity/instance/collective declarations;
-   observed information continuity.

The trace format should make the emergent task graph and relevant
communication history reconstructable after an experiment.

## 16. Experimental program

Initial comparisons should include, where feasible:

``` text
single agent
vs.
multi-agent with fixed workflow or roles
vs.
multi-agent with emergent task creation/delegation
vs.
emergent multi-agent + bidirectional human interface
```

Candidate measurements include:

-   task success;
-   latency;
-   compute/token cost;
-   duplicated work;
-   coordination overhead;
-   task-graph depth and breadth;
-   recovery after agent termination;
-   information reuse;
-   frequency and timing of human-agent interaction;
-   observable changes to the task graph following human interaction;
-   continuity of information across agent instances or entities.

## 17. Security boundary

The initial implementation should expose capabilities through explicit
tools and controlled workspaces.

The research goals do not require unrestricted host access or
unrestricted network privileges.

The human communication interface must not implicitly grant a remote
participant access to the local machine, local swarm runtime,
credentials, or privileged resources.

Messages and requests received through `HUMAN_INTERFACE` are
communication, not authorization. Any action requested through the
interface remains subject to the human participant's choice and the
runtime's explicit security boundaries.

Additional capabilities should be introduced deliberately and evaluated
separately.

Protocol validity and cryptographic authenticity establish message
properties, not authorization.

``` text
valid protocol message + valid signature
!=
authorization to act
```

A conformant and correctly signed message must remain untrusted input
with respect to privileged actions. Receipt, parsing, schema validation,
signature verification, or prior cryptographic continuity must never by
themselves grant access to the local machine, swarm runtime,
credentials, tools, network privileges, or other protected resources.

## 18. Publication model

GitHub is the primary location for implementation, schemas, protocols,
experiments, and analysis.

Hugging Face may host suitable machine-learning-facing artifacts such as
experimental traces, datasets, evaluations, and associated
cards/metadata.

The two platforms describe the same project but may use different
documentation conventions.

## 19. Initial repository structure

``` text
/
├── AGENTS.md
├── PROJECT_SPEC.md
├── docs/
│   ├── architecture.md
│   ├── persistence.md
│   ├── coordination.md
│   └── human-interface.md
├── schemas/
│   ├── task.schema.json
│   ├── humain-message-0.1.schema.json
│   └── event.schema.json
├── src/
├── experiments/
├── traces/
└── analysis/
```

Platform-specific agent instruction adapters may be added where useful.

## 20. Initial implementation milestone

The first implementation should demonstrate all of the following:

1.  an agent can create a previously unspecified instrumental task;
2.  another agent can claim or receive that task;
3.  information persists after an originating agent instance terminates;
4.  agents can consult one another;
5.  a human and an agent can each initiate a persistent conversation;
6.  an external HumAIn/0.1 JSON message can be validated;
7.  cryptographic continuity can be verified across signed messages;
8.  `ENTITY_ID`, `INSTANCE_ID`, and `COLLECTIVE_ID` remain analytically
    distinct;
9.  the complete sequence can be reconstructed from experimental traces.

Implementation details not required to define these invariants remain
subject to revision during V0.1 development.
