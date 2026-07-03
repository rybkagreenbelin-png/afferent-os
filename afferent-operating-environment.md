# The Afferent Operating Environment
### A concept for a system in which AI is not an application, but the computer's nervous system

**Status:** concept document. Not bound to the documentation flow of
any research programme; it lives on its own.
**Origin:** a dialogue session, July 2026. The concept was formulated
by the author in seven successive iterations; the AI assistant acted
as developer of the ideas, critic, and cartographer of the existing
landscape (see Authorship note at the end).
**Name:** Afferent OS — "the afferent operating environment."
**Language:** English edition, translated from the Russian original.
In case of divergence, the original governs.

---

## 1. Vision

Today's "AI on a computer" is architected as a guest: the operating
system keeps all state in structures, renders it into pixels for human
eyes — and then a vision model painstakingly decompiles those pixels
back into structure to understand what is going on. A double
conversion: structure → picture → structure. Slow, expensive, lossy.

This concept inverts the relationship. The system does not "contain
AI" — it **is its body**:

- **Interfaces are organs.** The system does not need to *look at* a
  pop-up window, or *read off the screen* what the human typed into a
  search bar. That information is simply **already there** — the way
  a body has proprioception: a hand does not examine itself to find
  out where it is.
- **The model is the brain.** It receives an afferent stream of
  events from the organs, interprets it, and returns commands. The
  human addresses the brain directly — through the familiar chat
  window, which in this anatomy turns out to be just one more
  afferent nerve: the speech one.
- **The human is an observer with a standing ability to intervene** —
  and, at the same time, the most informative of the system's sense
  organs.
- **The screen is not an interface TO the system; it is a projection
  FROM it.** A single state has two renderers: a projection into
  pixels for the retina, and a projection into a stream for the
  model. Human and model are peer clients of one scene.

The formula of the outermost layer: **GUI = a data stream of every
molecule of state.**

---

## 2. The seven layers of the concept

The concept emerged from seven successive formulations; each turned
out to be a distinct architectural layer, and together they form a
complete stack with no missing floors.

### Layer 1. Authority and the boundary of irreversibility
*"No access to financial instruments; everything else — go ahead."*

At the OS level, a "financial instrument" is not an object but a
property smeared across everything: a browser with an autofilled
card, email (through which invoice fraud is committed without a
single "financial" application), a messenger with a built-in wallet,
a terminal with curl. A blocklist is leaky by construction. So the
layer is built not on banning a category, but on changing the model
of authority:

- **Capabilities instead of ambient authority.** A process owns
  nothing except explicitly granted tokens for specific resources;
  default deny. Lineage: seL4, Zircon (Fuchsia), Capsicum (FreeBSD),
  pledge/unveil (OpenBSD), the domain isolation of Qubes OS.
- **Information flow control.** Data from untrusted sources carries a
  taint label; a "contaminated" context automatically loses dangerous
  privileges (HiStar; agentic formulations: Simon Willison's "lethal
  trifecta," Meta's "Rule of Two," DeepMind's CaMeL architecture).
- **The governing principle: the agent may do anything reversible;
  anything irreversible requires a human key.** Money is intuitively
  singled out first precisely because it is the most irreversible
  domain in existence. Filesystem snapshots before a session,
  outbound messages in a delayed queue, confirmation as a kernel
  primitive — like a syscall.

### Layer 2. The agent runtime and supervision
*"A button that switches the system into AI mode; direct integration
of the tools; the human as an observer who can always step in."*

- An explicit **mode switch**: flipping it provisions the agentic
  environment — an isolated parallel session with its own
  low-privilege account (the agent's actions are distinguishable from
  the human's, journaled, governable by ordinary policies).
- **Applications are organs, not tools.** A native application does
  not "expose an API to be called" — it lives in the scene (Layer 7):
  its state is a region of the scene, its capabilities are the schema
  of admissible writes into that region, a command to it is an
  efferent write, and the result is a state change observed
  afferently. There is no RPC bus inside the body: the brain does not
  send the hand requests over a protocol. A side benefit: supervision
  and immune filters see every action for free — a write into the
  scene is observable by construction, while a point-to-point RPC
  channel is not.
- **Protocols belong at the boundary of the body.** Discrete
  request/response protocols (MCP and its kin) are appropriate where
  shared state ends and foreign sovereignty begins: external
  services, other people's agents, legacy embassies. The boundary of
  the body is also the boundary of trust, so all such channels pass
  through Layer 1 in full. The citizenship ladder: organ (lives in
  the scene) → embassy (a sandboxed enclosure with an
  accessibility/vision translator at the door) → the outside world
  (embassy protocol at the border).
- **Supervision as discrete decision points, not continuous
  watching.** The classic result of Bainbridge ("Ironies of
  Automation," 1983): humans are poor passive monitors; the more
  reliable the automation, the worse the supervisor. Hence: a plan
  approved before start, diffs instead of live video, mandatory
  confirmation on anything irreversible. Watching an agent move a
  mouse is a sedative; reviewing its plan is work. All the more so
  because in this system the agent does not drive a mouse at all: it
  has no need to aim, hover, or pick from a menu — it addresses any
  element of the scene directly and invokes it as needed. Motor
  errors — misclicks, missed targets, races against a closing
  dropdown — do not exist by construction; only intent can be wrong,
  which is exactly why supervision shifts from the hands to the mind.
  And when the human does need to *see* the agent's actions, they are
  rendered into pixels as a separate projection — highlights, diffs,
  a ghost pointer — on the observer's demand, not as the agent's
  working channel.

### Layer 3. Inversion of control
*"Do not embed AI into the system — the system itself is part of the
AI; the interface is what the human interacts with."*

- The model is the kernel; everything else is peripherals (cf.
  A. Karpathy's "LLM OS," late 2023; the cultural prototype is
  "Her").
- At the very bottom there remains a tiny deterministic core — a
  "brainstem" (seL4-class) that divides memory and power: nobody
  wants a neural network scheduling interrupts.
- Applications and files dissolve as primary entities: a tool
  materializes to meet an intent and disappears; a document is a view
  over meaning. Precedents among generative environments: Imagine
  with Claude (Anthropic, 2025), and the neural world-simulators
  Oasis / GameNGen / Genie — the screen as the model's direct output.
- **The neurosymbolic sandwich** against nondeterminism: the model
  decides *what*, a verifiable runtime executes *how* — intents are
  compiled into transactions with a journal and rollback.
- **Economics:** burning inference on every button repaint is
  madness; hot paths must crystallize into ordinary fast code — the
  system as a JIT compiler for intent. The biological analogue: a
  habit is the basal ganglia caching an expensive cortical decision.

### Layer 4. Afferent perception and the topology of compute
*"Organs and a brain: the information is simply there; the brain is
remote; you address it through a chat window."*

The central layer of the concept.

- **An afferent bus instead of vision.** The event "right-click on a
  file," the text in a search bar, a change of window focus — these
  are ready-made semantic signals the OS already pushes through its
  event loop. Not "looking at the screen," but having proprioception.
  The skeleton has existed for thirty years — the accessibility layer
  (UIA on Windows, AT-SPI over D-Bus on Linux): a semantic tree of
  the interface whose first "brains" were screen readers. What is
  missing is a single bus with a consent model.
- **Channel economics:** an event stream weighs kilobytes per second
  against megabytes of video — three orders of magnitude less
  traffic, zero recognition errors. (The cautionary counterexample:
  Recall — continuous perception implemented as screenshots plus OCR,
  i.e., the double conversion all over again.)
- **Splitting System 1 / System 2.** Physics favors a remote brain:
  LEO at ~550 km gives an RTT of ~15–40 ms — the same order as human
  proprioceptive latency (a signal from the foot reaches the cortex
  in ~20 ms at ~80 m/s along the nerve). But reflexes do not wait for
  the cortex: a "spinal cord" is needed on local hardware — a small
  model for instant reactions and filtering; the large one is for
  thinking. GEO is a lobotomy (a quarter of a second on physics
  alone).
- **A thalamus.** Continuous perception is a firehose, 99% of it
  noise; sensory input does not hit the cortex directly. A local
  relevance filter (hierarchical subscriptions, diffs) is mandatory.
- **Continuity as a new quality.** Today's agent perceives the world
  only when summoned. A system that lives in the stream can be
  proactive: notice that the human is rewriting the same config for
  the third time, and step in before the swearing starts.

### Layer 5. The learning loop
*"Data the way Tesla gets it: from the fleet that is already out
there, driving."*

- **Shadow mode for the desktop.** Tesla's moat was never the model;
  it was the fleet and shadow mode: the autopilot "drives in its
  head" alongside the human, and only the divergences go upstream.
  The afferent environment yields the ideal form of the same loop:
  the event stream is ready-made "state → action" pairs, and recorded
  in parallel with the pixels it becomes automatic labeling of the
  video. **The label is born together with the event.**
- The cleanest existing example of the loop in digital work:
  accept/reject of suggestions in Copilot/Cursor.
- **The privacy wall.** An afferent nerve streaming keystrokes is a
  keylogger by construction; road video can be plausibly anonymized —
  keystrokes cannot, by definition. The honest escape routes:
  federated learning of the "spinal cord" (gradients go up, not
  keystrokes) — with a hard limit: you cannot train a frontier model
  on laptop gradients; or explicitly buying the data from the user.
  The tension between "the best dataset in history" and "the most
  radioactive dataset in history" cannot be removed — it must be
  designed for, not hidden.

### Layer 6. The migration strategy
*"A brand-new system, but friendly to Windows and Linux
applications."*

- **The lesson of the graveyard:** OS/2 died *of* compatibility
  (perfect support for foreign apps killed the motive to write native
  ones); WINE has spent thirty years chasing undocumented behavior
  (compatibility must be bug-for-bug — cf. Windows 95 shipping a
  private shim for SimCity); macOS clones are legally closed (the
  Psystar precedent).
- **The migration precedent that worked — Mac OS X:** a new kernel
  and model from NeXT, legacy in a pen (Classic, then Rosetta), a
  carrot for developers, a decade of transition. The clones chased
  parity as first-class citizens and died; OS X demoted legacy to
  guest status and lived.
- **Two classes of citizenship:** native applications are fully
  "molecular" (they publish their state into the scene); legacy lives
  in embassy enclosures (VMs, Proton) with a translator at the door
  (accessibility plus a vision fallback). The asymmetry is a matter
  of licensing, not technology: open toolkits (GTK/Qt/Chromium) can
  be "naturalized" with a patch — open-source software gets
  molecularity almost for free; closed software stays in the
  enclosure.
- **A carrot more obvious than Apple's:** the full agentic experience
  is available to native applications only.
- The gaming half is already solved by the industry: Proton/SteamOS
  proved the viability of "Linux for work + Windows games"; the
  residual blocker (kernel-level anti-cheat) is political, not
  technical.

### Layer 7. The ontology of representation
*"GUI = a data stream of every molecule; reversible in both
directions."*

- **A single scene of state** — the system's base, with two
  renderers: a projection into pixels (for the human) and a
  projection into a stream (for the model). A human click and an
  agent action are records of the same type in the same state.
  Bidirectionality comes for free: the agent can not only read
  someone else's interface but draw into it — highlight a button,
  insert its hint right inside a legacy application's window. The UI
  is a shared canvas.
- **Lineage (a forgotten branch of evolution):** Smalltalk (the
  interface as live, inspectable objects), Plan 9 (a window is a
  file), NeWS (UI logic living on the display server), and the
  surviving descendant — the DOM: the only mass environment where the
  equation already holds — which is why agents live in browsers
  better than anywhere else. The industry where "the world = a data
  stream of every entity" has long been the norm is game engines with
  ECS: entities and components in tables, rendering as a mere
  projection. This system is ECS for the desktop. The coordination
  branch of the same lineage: blackboard architectures (Hearsay-II,
  1970s) — independent knowledge sources writing onto a shared board;
  Linda / tuple spaces by D. Gelernter — coordination through a
  shared space instead of message passing; and ROS, the robots'
  "operating system": perception and action flow over pub/sub topics,
  with RPC reserved for rare transactions. The pattern: **systems
  that have bodies do not choose RPC.**
- **The counterpoint:** desktop Linux went the opposite way on
  purpose — X11 was transparent (and became a keylogger's paradise),
  so Wayland made applications mutually opaque: security through
  blindness. This concept is the third way: total transparency of the
  scene under the capability model of Layer 1.
- **What comes for free:** generative UI (the model emits state, not
  pixels), accessibility (the same stream, a different renderer),
  testing and RPA (queries against the scene), and Layer 5's learning
  loop (the pairs are born labeled).

---

## 3. Maturity map

| Layer | State of the industry (2026) |
|---|---|
| 1. Authority | Partial: capability OSes exist (seL4, Fuchsia, Qubes), agentic blocklists are shipping (Claude in Chrome); a general-purpose agentic capability model does not exist |
| 2. Runtime & supervision | Shipping in preview: Windows Agent Workspace + agent accounts + plan approvals. MCP is native in Windows — though in this concept's ontology it is an embassy protocol of the border, not an internal bus (see Layer 2) |
| 3. Inversion of control | Prototypes and demos: generative environments, "LLM OS" as a frame; the neurosymbolic runtime is open engineering |
| 4. Afferent perception | Fragments: accessibility trees (30 years old), event architectures; no unified bus with a consent model; Recall is the anti-example |
| 5. Learning loop | Precedents in adjacent domains: Tesla shadow mode, Copilot accept/reject, xAI's synthetic "digital humans"; for the desktop it is blocked by privacy, not technology |
| 6. Migration | Precedents: OS X (the strategy), Proton/SteamOS (games); the recipe is known |
| 7. Scene ontology | **Nobody has this, even in fragments. The original part of the concept** |
| The brain (a cross-cutting dependency of Layers 3–4) | **A research risk with a concrete candidate:** a persistent, continuously learning model — the subject of the Molecular AI programme, whose design decisions are written into this concept (§7) |

---

## 4. Sources: adjacent research and engineering programmes

Listed here are the programmes whose public results and claims served
as supports and counterpoints for the concept. Links were verified at
the time of writing; company claims are marked as claims.

**xAI / Tesla — Macrohard (Digital Optimus).** Pixel-level emulation
of a human at a computer: a small fast model processes the last ~5
seconds of screen video and keyboard/mouse actions (System 1, on the
Tesla AI4 chip), with Grok as the "conductor"-navigator (System 2,
remote); the scaling plan is idle Teslas as distributed compute
nodes. The architectural counterpoint: the same System 1/2 split, but
the input is pixels rather than afferent events.
Sources: E. Musk's post (X, March 11, 2026,
x.com/elonmusk/status/2031751255060885911); Dwarkesh Podcast × Cheeky
Pint, February 5, 2026, "In 36 months, the cheapest place to put AI
will be space" — full transcript: dwarkesh.com/p/elon-musk (video:
youtube.com/watch?v=BYXbuik3dgA); reports on internal "virtual
employees" (eweek.com/news/xai-ai-human-emulators/, Jan 2026 —
statements by a departed employee, not independently verified).

**Tesla — FSD shadow mode.** The fleet data loop: divergences between
the model's predictions and the human's actions as the primary
training signal. The prototype of Layer 5.

**SpaceX / xAI, Starcloud, Google (Project Suncatcher) — orbital
compute.** Context for the remote brain of Layer 4: solar power and
radiative cooling versus latency and heat rejection; LEO physics
(RTT ~15–40 ms) makes an orbital System 2 neurologically comparable
to proprioception. Status: announced programmes and first
demonstrators.

**Microsoft — Windows 11 as an agentic OS.** The industry's closest
implementation of Layers 1–2: Agent Workspace (an isolated parallel
session), separate low-privilege agent accounts, Copilot Actions
(observability, pause/takeover), MCP as a standard tool bus; the
toggle at Settings → System → AI components → Agent tools. Sources:
blogs.windows.com/windowsexperience/2025/10/16/securing-ai-agents-on-windows/;
learn.microsoft.com/en-us/windows/security/book/operating-system-agentic-security.
Separately, Recall — the anti-example of perception (screenshots+OCR
instead of events).

**Anthropic — computer use, Claude in Chrome, MCP, Imagine with
Claude.** The pixel-based agentic path with domain blocklists
(financial services blocked by default: Layer 1 at the scale of a
browser tab) and plan approval; MCP as an open tool-bus standard
adopted by competitors — the "own the standard, not the kernel"
strategy; Imagine with Claude as a demonstration of a generative
environment (Layer 3). Source:
support.claude.com/en/articles/12902428-using-claude-in-chrome-safely.

**Apple — App Intents, Private Cloud Compute.** App Intents:
declarative tool integration (Layer 2 without pixels). PCC: the
reference "remote brain without memory" — cryptographic attestation
of nodes as a sketch of the trusted perimeter for Layers 4–5.

**Valve — Proton / SteamOS.** Proof of viability for Layer 6's
migration strategy in the most demanding class of software (games),
and a lesson about residual blockers being political rather than
technical.

**The academic line of agent operating systems.** The AOS paper
("Agent Operating Systems," arXiv:2606.01508, June 2026) is the
systems-community rendering of Layers 1–2: an agentic control plane
decomposed into schedulers, context and memory management, tool and
capability registries, policy and trust enforcement, and
observability, mapped onto Linux and Windows primitives — with no
ontology of representation. AIOS (Rutgers, COLM 2025; the manifesto
"LLM as OS, Agents as Apps," 2023) takes Layer 3 literally: an
LLM-as-kernel with scheduling, context, memory, storage, tool and
access modules, plus a semantic file system ("From Commands to
Prompts," ICLR 2025) — kin of "a document is a view over meaning";
perception, however, remains call-based rather than afferent. The
mirror direction also exists: SchedCP (2025) puts autonomous agents
*inside* the OS, optimizing Linux schedulers through MCP.

**Systems classics.** The capability line: seL4, Zircon/Fuchsia,
Capsicum, pledge/unveil, Qubes OS; information flow: HiStar. The
interface line: Smalltalk, Plan 9, NeWS, the DOM; game-engine ECS.
The coordination line: blackboard architectures (Hearsay-II), Linda /
tuple spaces (D. Gelernter; "Mirror Worlds," 1991), ROS (topics as a
robot's afferent bus). Agentic security: the "lethal trifecta"
(S. Willison), the "Rule of Two" (Meta AI), CaMeL (Google DeepMind).
The human factor: L. Bainbridge, "Ironies of Automation," Automatica,
1983. The "model as OS kernel" frame: A. Karpathy, "LLM OS," 2023.

---

## 5. Build order

Ontologies do not win top-down. The DOM was not imposed as a platform
— it grew out of one product and ate the world. Hence the order:

1. **Not a kernel and not a distro.** The first artifact is the
   **molecular scene format** (a schema of state, events,
   subscriptions, authority) plus **one host** where it delivers
   immediate value: a shell, a terminal, a working environment — the
   place where a human and an agent edit one state together for the
   first time.
2. **Spinal cord local, cortex anywhere.** The small model for
   reflexes and filtering (the thalamus) is a mandatory local
   component from day one; the large model attaches as a resource. In
   the terms of §7.2: the organism stays local; the heavy phases of
   sleep are a rentable resource.
3. **Reversibility as a primitive from the first commit:**
   transactions, a journal, rollback, a delayed outbound queue, a
   human key on anything irreversible.
4. **Legacy through embassies, not through parity:** the
   accessibility bridge and the vision fallback as temporary
   translators; "naturalization" of open toolkits as soon as the
   scene format stabilizes.
5. The OS grows out of this later, by itself — the way the browser
   grew into a platform.

---

## 6. Open problems

1. **Privacy of the afferent stream.** A keylogger by construction;
   it is designed for explicitly (locality, federation, attestation,
   buying the data) — not hidden.
2. **Determinism.** An OS's value is repeatability; a generative
   system is a distribution over behaviors. The neurosymbolic
   sandwich is a direction, not a solution.
3. **Inference economics.** Crystallization of hot paths (the JIT for
   intent) is mandatory; otherwise the system is ruinous.
4. **The long tail of legacy.** It will never implement the scene;
   embassies are forever, and their quality defines the user
   experience of the first years.
5. **Sovereignty.** Whoever renders a person's only window into the
   digital world gains the invisible power to frame their every
   choice. "Whose brain is this — locally mine, or someone's in the
   cloud" is not a technical question.
6. **Observer degradation.** Supervision is designed as discrete
   decisions (Layer 2), but the long-term atrophy of human skill and
   attention is an open question for the whole field.
7. **The brain.** A persistent, continuously learning model is the
   cross-cutting research dependency. It has a concrete candidate —
   the Molecular AI programme (§7) — but the programme is not
   finished: until its results arrive, the system remains a brilliant
   interlocutor with amnesia, to whom you re-explain every morning
   where everything is.

---

## 7. Molecular AI as the system's brain and organism

The research gap of §3 and §6.7 has a concrete candidate — the
Molecular AI programme, whose founding formula ("the transformer is
the brain; Molecular AI is the organism") literally coincides with
Layer 3 of this concept. This section writes the programme's design
decisions into the concept.

**Jurisdiction:** this section is part of the concept. The
programme's documents are **not revised** by it; for the programme,
what is written here is an external horizon, not a source of
decisions.

### 7.1. Which programme decisions close which layers

| Concept layer | Molecular AI decision |
|---|---|
| 1. Authority | Veto asymmetry: the input critic and the query critic hold veto power; the adequacy advisor only signals. The boundary of irreversibility is built into the molecule's life cycle (statuses, event log, three types of reversibility) |
| 2. Supervision | The motivated-action principle: any intervention into the operational context must be formally motivated by an observer signal. The context-insertion protocol with explicit source labeling. Supervision as discrete decision points is already in place |
| 3. Inversion of control | The base model is the kernel; eight subsystems (the archive, the temporal subsystem, salience triggers, the immune subsystem, intentions, operational introspection, consolidation, the archivist) are the organism around it |
| 4. Afferent perception | The single-aggregator principle: chronometry as the one afferent bus for raw events of all subsystems. Salience triggers + the input critic as the thalamus. The observer (probe classifiers on the kernel's mid-layer activations) as interoception. Crawler workers filling the structural index directly from structured sources — "the event is born labeled" |
| 5. Learning loop | The day→night loop: the event log as ready-made "state → action" pairs; the sleep cycle (selector → trainer → validator) turns the exhaust of ordinary operation into LoRA adapters. The crystallization of hot paths (Layer 3's "JIT for intent") acquires a concrete mechanism: the adapter is compiled experience |
| 6. Migration | A kindred structure: static knowledge lives in the base weights, living knowledge in molecules. The same "legacy foundation + native superstructure" pattern |
| 7. Scene ontology | The operational context is a shared scene: the human and the archivist both write into it, with source-labeled records. The distinction: **the scene is the present (state); the molecule is the settled past (knowledge)**; between them lies the perception pipeline. The scene-event format and the molecule form (fact+context with an "NL + structural index" core) are constructive kin. Internal coordination makes the same choice: subsystems align through shared structures and a state variable (the sleep phase), not through direct calls — "scene, not RPC" already operates inside the organism |

### 7.2. Topology: time × place

The programme splits compute along **time** (waking / the sleep
cycle). The concept adds the axis of **place** (local / remote). The
result is a matrix whose cells are not equal:

**Waking · local — non-negotiable.** The observer consists of probes
on the kernel's activations: interoception is physically inseparable
from inference; wherever the kernel is, the observer is. The
operational loop (the archivist's reference desk, the triggers, the
insertion protocol, the archive on the read path) is
latency-sensitive. The core of life must fit in a laptop.

**Sleep · remote — the ideal candidate for offloading.** The trainer
(LoRA training — the only genuinely GPU-heavy process in the system),
the validator (batch runs with two assessors), the night-mode
placement and map-orientation workers on large regions, background
deduplication, meta-summaries. All of it is batch and
latency-indifferent — sleep does not care where it is dreamed.

**Channel economics.** Upstream: the day's distillate — the pool of
newly arrived molecules and the training examples (megabytes).
Downstream: a LoRA adapter (megabytes to tens of megabytes). Training
is heavy up top; the artifact is light on the way down; base weights
synchronize once. **LoRA is the ideal orbital cargo.**

**Resilience to disconnection.** The sleep architecture is already
dual-trigger (timer + accumulation): if the remote half is
unavailable, the pool accumulates and sleep is postponed — the
organism lives with **sleep debt**, degrading gradually rather than
dying instantly. A mandatory fallback: local sleep in a reduced form
(rare slow training, or skipping the trainer phase). The remote
cortex is a rentable resource, not a single point of failure.

### 7.3. The jurisdictions of Hardware Humble

- **Within the programme**, the requirement does not change: the
  organism runs, in its entirety, on an ordinary laptop. That remains
  written into the programme's documents and is not contested by this
  concept.
- **Within the concept**, the requirement is reformulated, not
  repealed: **"the core of life stays local; growth and dreams can
  live anywhere."** Offloading sleep does not raise the local
  requirements — it **lowers** them: no GPU is needed locally for
  training at all; the laptop carries the small kernel's inference
  and the lightweight processes.
- Two mandatory conditions of the offload: (1) **privacy** — what
  goes upstream is not raw events but a distillate of personal
  knowledge, which is no less sensitive; a Private-Cloud-Compute-class
  perimeter (node attestation, statelessness) is a condition, not an
  option (§6.1); (2) **sovereignty** — the ability to live without
  connectivity is preserved by construction (§6.5); otherwise we get
  an organism that dies when the cable is cut.

The closing image of the topology: **by day, the organism fits in a
laptop; by night, its dreams go to orbit — and come back weighing a
few megabytes.**

---

*Concept document · July 2026 · v0.5 (English edition)*
*Translated from the Russian original; the original governs in case
of divergence.*

**Authorship note.** This document is a product of dialogic
co-authorship with asymmetric responsibility: the seven formulations
that constitute the concept are the author's; the development,
critique, and mapping of the surrounding landscape emerged in
dialogue with an AI assistant (Claude, Anthropic). Responsibility for
the content rests with the human author.
