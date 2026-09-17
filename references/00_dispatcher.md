# Cathedral Taxonomy — Technical Operational Dispatcher

**Framework Author**: William Fitzpatrick (*Writer Science*)  
**Source Lecture**: [The Most Powerful Writing Frameworks to Write CLEARLY](https://www.youtube.com/watch?v=cayUBPHB1HQ)  
**Parent Collection**: [Master Collection](../../kirby-fitzpatrick-writers-collection/SKILL.md) | [Global Help](../../../kirby-help/SKILL.md)  

---

## 1. Cognitive Foundation: The Whole Before the Parts

### 1.1 The mechanism in one sentence

A reader cannot file a component until they know **which container the component belongs to** — so any technical document that descends into implementation detail before naming the set of macro categories forces the reader to hold every detail in an unlabelled buffer and *infer* the architecture from the bottom up, which is the single most expensive operation working memory can perform.

A cathedral is not understood by walking into a window and inspecting the leadwork. It is understood by first being told: *this building has a nave, two transepts, a choir, and an apse.* Only then does the window become readable as *the window in the north transept*. The taxonomy is the address space; the component details are the values written into it. Address before value is not a stylistic preference — it is how memory is addressed at all.

**The Cathedral Taxonomy** is therefore a two-phase protocol with a hard ordering gate:

1. **DECLARE (the whole)** — state the **exact number** and the **identity** of every macro category before a single implementation detail of any one of them appears.
2. **INSTANTIATE (the parts)** — descend into each category, tagging each traversal with its slot address (`[2/4] token-refresher`), so the reader can bind every new fact to a slot that already exists.

The cardinality is not decoration. *"There are several services"* is a taxonomy with no closure condition — the reader can never know whether the enumeration is finished, so the buffer is never released. *"There are exactly four services"* installs a counter. When the counter reaches four, working memory flushes, the whole compresses into one chunk, and the reader's model is complete. That is the entire payoff: **declared cardinality converts N independent facts into 1 chunk with N properties.**

### 1.2 The two-phase read cycle

```text
For every structural passage, the reader must execute two phases IN THIS ORDER:

  (1) DECLARE   build the address space
                "the ingest plane has exactly 4 services" ──► 4 slots allocated
                cost: ~1 sentence, paid once

  (2) BIND      write each detail into an existing slot
                "[2/4] token-refresher rotates keys every 15 min" ──► slot 2 filled
                cost: ~0 per detail, because the address already exists

  Invert the order and step (2) runs with ZERO allocated slots:
  every detail is a free-floating buffer entry that must be re-attached
  later — or, far more commonly, silently dropped.
```

### 1.3 Before vs after

```text
❌ BEFORE — Bottom-Up Enumeration (the whole is never declared)

   reader working memory
   ┌──────────────────────────────────────────────────────────────┐
   │ [?] ingest-gateway     ─ detail ─ detail ─ detail           │
   │ [?] token-refresher    ─ detail ─ detail ─ detail           │
   │ [?] audit-sink         ─ detail ─ detail ─ detail           │
   │  ...how many more? is that all of them? do these three      │
   │     even belong to the same boundary?                       │
   └──────────────────────────────────────────────────────────────┘
       ▲ no top-level slot ⇒ no chunk boundaries in the buffered
         detail; the architecture must be RE-DERIVED by the reader
         after EOF, and there is no closure condition to signal
         that the derivation is allowed to stop.

✅ AFTER — Cathedral Declared, Then Naves Traversed

            THE INGEST PLANE — exactly 4 services
   ┌──────────────────────────────────────────────────────────────┐
   │  [1/4] ingest-gateway    [2/4] token-refresher               │
   │  [3/4] audit-sink        [4/4] replay-worker                 │
   └──────────────────────────────────────────────────────────────┘
       ▲ DECLARED before any implementation detail of any member

       ├── [1/4] ingest-gateway    ── detail binds to an existing slot
       ├── [2/4] token-refresher   ── detail binds to an existing slot
       ├── [3/4] audit-sink        ── detail binds to an existing slot
       └── [4/4] replay-worker     ── TERMINAL: counter exhausted,
                                      working memory flushes,
                                      whole compresses to one chunk
```

### 1.4 Why this is acute in software engineering

Code artifacts are *hierarchically nested by construction* — directories contain packages contain modules contain types contain fields. The reader's mental model is literally a tree, and a tree cannot be built leaf-first: a leaf with no parent is uninsertable. Three consequences follow.

* **Every repository read is a taxonomy read.** `packages/` with 19 entries and no stated domains is not a directory listing; it is a demand that the engineer reverse-engineer the org chart from the filesystem.
* **Architecture drift is invisible without a declared taxonomy.** You cannot notice that a fifth bounded context appeared if the document never committed to there being four. Cardinality is a *regression test* for architecture.
* **The AI failure mode is the same failure mode at scale.** A model that emits 40 files of implementation detail without first stating the component set produces exactly the state in the "BEFORE" diagram — output that must be re-read end-to-end to be understood, because no intermediate structure was ever asserted.

### 1.5 Boundary discipline

Cathedral Taxonomy is a **document- and section-scope structural protocol**. Three neighbouring concerns are out of scope and belong to sibling skills: the ordering of old-before-new information *inside* a sentence ([Topic–Comment Elaboration](../../kirby-fitzpatrick-topic-comment-elaboration/SKILL.md)), the sentence-to-sentence hand-off that carries focus across the enumeration ([Linear Relay Linking](../../kirby-fitzpatrick-linear-relay-linking/SKILL.md)), and the preview of sequence at paragraph granularity ([Theme Preview Roadmap](../../kirby-fitzpatrick-theme-preview-roadmap/SKILL.md)). Cathedral Taxonomy does not govern how each slot is written — only that **the slots exist and are enumerated before any of them is written.**

---

## 2. Core Transformation Protocols

### Rule 1 — The Cardinality Invariant (exact integers, never quantifiers)
Every macro set is announced with a **hard integer**. `three`, `four`, `six`, `2 bounded contexts`. Forbid: `several`, `a few`, `various`, `some of the`, `a number of`, and the phrasal dodge `the following` with an unstated count.

* **Test**: replace the number with "exactly N". If the sentence sounds odd or over-committed, you were covering for an unplanned set — plan it, then commit.
* **Test**: count the members you actually deliver. If the count ≠ the declared count, the document failed the invariant and must be repaired (Rule 5), not quietly extended.

### Rule 2 — The Identity Declaration (names at declaration time, not at arrival time)
The taxonomy sentence names **every member, by name**, before any member's detail appears. Deferred identity (`first we'll look at the gateway, then at the other services`) is not a declaration — it is a serialized drip that leaves every future slot unallocated until it arrives.

* **Declaration form**: `The ingest plane has exactly four services: ingest-gateway, token-refresher, audit-sink, and replay-worker.`
* **Ordering claim**: if the traversal order matters, state it in the same sentence (`listed in dependency order; each later service consumes the previous one's output`). If it does not, say so (`listed alphabetically — the order carries no meaning`), so the reader stops hunting for a signal that isn't there.

### Rule 3 — The Depth Gate (no component gets depth while the set is incomplete)
Between the declaration and the last slot's opening, **implementations details are forbidden**. No method signatures, no config keys, no failure modes, no code blocks belonging to a member. The gate opens per-slot, not globally: `[2/4]` may be described in full depth once the reader is inside `[2/4]`, but `[3/4]`'s internals stay sealed until the traversal arrives.

* **Violation signature**: the phrase *"but more on that later"* inside the declaration block. That is a depth leak through the gate — move the material to its slot.
* **Recursive application**: a slot that itself contains a macro set declares its **own** nested cathedral (`[2/4] token-refresher is composed of exactly three stages: …`) before detailing any stage. Nesting is legal; mixing levels is not (Rule 7).

### Rule 4 — Slot-Marker Discipline (every section re-affirms its address)
Every traversal section opens by restating its coordinates: **index, cardinality, name**. Headings should carry the marker literally.

* `### [2/4] token-refresher — Key Rotation Path`
* Every re-entry after an interruption (a code block, a table, a diagram) re-anchors with a back-reference: *"Back in `[2/4]`: …"* — because the reader's buffer may have been evicted by the intervening artifact.
* The marker is also the audit handle: a document grep for `\[\d/\d\]` should return exactly one heading per declared member, in declared order, with no gaps.

### Rule 5 — Closure Signalling and Taxonomy Amendments
The last member is marked **terminal** explicitly (a trailing summary sentence, a `[4/4] — final` heading, or a closing line enumerating all four in order). An unmarked final member leaves the reader waiting for a fifth that never comes.

If the set genuinely grows mid-document, you may **not** silently append. Issue an explicit amendment: *"The taxonomy is amended from four to five services; the fifth is `late-arriving-worker`, inserted before `replay-worker` because it feeds it."* Silent extension destroys the closure guarantee — which was the entire value of the declaration.

### Rule 6 — Bounded, Non-Overlapping Slots (MECE at the boundary)
Two constraints on the declared set itself:

* **Non-overlapping**: no member may be a subset of another. `infra, database, migrations, SQL changes` is a broken taxonomy — a schema change files under two slots, so the reader cannot decide where to store it and stores it nowhere. Repair to `infra, schema, data backfill`.
* **Bounded**: 3–7 slots at any single level. Above ~7 the taxonomy stops compressing and becomes a flat list — regroup into coarser macro categories and enumerate the fine-grained set in a nested declaration (Rule 3). **The whole point is that the top level fits in working memory and the parts live under it.**

### Rule 7 — One Level Per Declaration (no mixed granularity)
Every member of a single taxonomy must be the **same kind of thing**. `auth, retry logic, TokenStore.refresh(), TLS cert rotation` fails: service, concern, method, and operational concern are four different levels of the same tree. A reader who cannot classify the members cannot use the slots. Pick the level the document is actually about and demote the rest one level down.

### Rule 8 — Cardinality as an Architecture Regression Test
Wherever the taxonomy describes a system you own, treat the declared integer as **asserted truth**: it belongs in the RFC as a testable claim, not a rhetorical opener. *"The ingest plane has exactly four services"* is a commitment that a fifth service in `services/` is a defect or an undocumented amendment. Declared cardinality is how architecture drift becomes *visible* instead of merely *true*.

### 2.1 Transformation Table

| # | Anti-Pattern (Violation) | Defect | Clean Replacement (Declared Taxonomy) |
|---|---|---|---|
| 1 | *"There are a few things to keep in mind when adding a new node type."* | Unbounded quantifier — no closure condition, so the buffer never flushes. | *"There are exactly three things a new node type must satisfy: grammar registration, visitor dispatch, and fixture coverage."* |
| 2 | *"First, let's look at the parser. Next, the resolver. Then the codegen."* | Identity deferred to arrival — slots are allocated one at a time, so no member is ever anchored while being read. | *"The compiler has exactly three passes: parser, resolver, codegen. In `[1/3]` the parser…"* |
| 3 | *"Adding a new node type requires editing `grammar.ts` … [400 words] … Speaking of node types, the AST has five node classes."* | Depth before taxonomy — the macro map arrives after the reader has already filed unanchored detail. | *"The AST defines exactly five node classes: Literal, Identifier, Binary, Call, and Block. `[1/5] Literal` is registered in `grammar.ts` as…"* |
| 4 | *"This PR changes X and Y. … Also, I renamed the config loader."* | Silent extension — an undeclared fourth slot breaks the closure counter. | *"This PR makes exactly four changes. The fourth, renamed last: `ConfigLoader` → `ConfigReader`."* |
| 5 | *"Categories: infra, database, migrations, SQL changes."* | Overlapping slots — schema churn files under two categories, so it files under none. | *"Categories: infra, schema, data backfill."* |
| 6 | A flat bulleted list of 19 microservices, each with a paragraph. | Unbounded taxonomy — exceeds working memory; the list stops compressing. | *"The platform has exactly four bounded contexts: identity, ingestion, billing, reporting. `[2/4] ingestion` is composed of six services: …"* |
| 7 | *"Components: auth, retry logic, `TokenStore.refresh()`, TLS cert rotation."* | Mixed granularity — four different tree levels presented as siblings. | *"Services: auth, retry, tokens, tls. Under auth: `TokenStore.refresh()` is the one you'll touch most."* |
| 8 | A guide listing `packages/*` alphabetically while the reader must build in dependency order. | Ordering claim absent — the reader infers a signal from an arbitrary order. | *"Five packages, listed in dependency order (each imports only from earlier entries): `core`, `protocol`, `client`, `cli`, `e2e`."* |
| 9 | *"The following services are involved, but we'll get to them as needed."* | Declaration explicitly refuses to declare — the strongest possible version of the violation. | *"Exactly three services are involved: `gateway`, `ledger`, `sweeper`. All three are described below in the order they are invoked."* |

### 2.2 Repair Procedure (the Cathedral Pass — 90 seconds)

1. **Grep for integers.** Search the draft for `\b(two|three|four|five|six|seven)\b` and for the quantifiers in Rule 1. Every structural passage must have one and only one of the former.
2. **Find the first implementation detail** — the first method signature, config key, code block, or failure mode. Everything above it must be declaration. If detail appears above the first integer, the gate was breached.
3. **Build the address list.** From the delivered members, write the canonical set: `[1/N] name, [2/N] name, …`. If it does not fit in one sentence, the taxonomy is too fine — regroup (Rule 6) until it does.
4. **Resolve overlaps and level mixing.** For each pair of members, ask *"could one artifact file under both?"* (overlap) and *"are these the same kind of thing?"* (level). Fix both before publishing.
5. **Re-read only the slot headings and the declaration.** They must form a complete, ordered, non-overlapping table of contents for the artifact — and the last one must be visibly terminal.

---

## 3. Engineering Application Scenarios

### 3.1 Code Reviews

A review thread is a taxonomy problem before it is a correctness problem. Multi-concern diffs arriving without a declared concern set produce interleaved, unordered comments that the author cannot triage — the author must re-derive *how many distinct things* are being asked of them.

* **Declare before commenting**: open the review with the concern taxonomy — *"Three blockers and two nits. Blockers: `[1/3]` the pool is never drained on `Close`; `[2/3]` the retry counter resets per request; `[3/3]` the migration has no down-path. Nits: `[4/5]` naming, `[5/5]` a dead import."* — then thread each comment under its marker.
* **Severity is the top-level taxonomy**: `blocker` / `question` / `nit` is a 3-slot MECE declaration the author reads first, and it lets them batch decisions instead of re-reading the whole thread per comment.
* **Unmarked adenda are the classic failure**: a review that opens *"a couple of issues"* and then emits seven comments, the last of which is a security hole, buries the most important member in the least structured slot.
* **Re-review discipline**: after a force-push, re-declare. *"Taxonomy of the new revision: `[1/2]` blocker 1 is fixed; `[2/2]` blocker 2 is now a memory leak, not a race."* Positional markers make the diff conversation traceable across rounds.

### 3.2 PR Descriptions

A PR description is read by an engineer who must decide, in seconds, how many independent things this change couples together — because that number determines review depth, rollback plan, and blast radius. Declaring the taxonomy answers that question before any diff is opened.

```text
❌ "The retry queue now derives keys per tenant. There's also a new TenantSaltProvider
    interface, plus the migration adds a salt column, and I fixed a flaky test while
    I was in there, and the README needed updating."

✅ "This PR makes exactly five changes. Only #1 is behavioural.
    [1/5] Behaviour: the retry queue derives idempotency keys per tenant.
    [2/5] Interface: adds `TenantSaltProvider`.
    [3/5] Migration: adds `tenants.salt`, backfilled.
    [4/5] Test: de-flakes `retry_queue_test.go`.
    [5/5] Docs: README retry section.
    Reviewer focus: [1/5] and [3/5]."
```

* **Skeleton**: `Taxonomy` → `[N/M] detail per change` → `Verification` → `Risk`. The taxonomy block is the first screen and must fit in it.
* **Flag the behavioural slot explicitly.** *"Only `[1/5]` is behavioural"* tells the reviewer that four-fifths of the diff is mechanical — a statement only possible if the cardinality was declared.
* **Rollback framing**: name which slots are revertible independently. `[3/5]` (migration) usually is not; stating that requires the taxonomy to exist.
* **Reviewer routing follows slots**, not files: an unmarked multi-concern PR lands on one reviewer's desk and one of its five concerns ships unreviewed.

### 3.3 Architecture RFCs / ADRs

RFCs are read out of order, months later, by readers who have no reading momentum to carry them — so the declared taxonomy is often the *only* structure that survives. It is also the section that makes the rest of the document navigable.

* **Component taxonomy before component design**: *"The ingest plane has exactly four services: `ingest-gateway`, `token-refresher`, `audit-sink`, `replay-worker`. All four are described below in invocation order; `[2/4]` is the only one with state."* No service internals appear above that line.
* **Microservice boundaries**: the taxonomy *is* the boundary argument. If you cannot state `exactly N` contexts and name them, the boundaries are not yet decided — the declaration exposes that before 2,000 words of design hide it. Every new service after publication is an explicit amendment (Rule 5), which is precisely the review conversation you want to have.
* **Monorepo directory guides**: declare the domain set before walking the tree — *"The repo has exactly five top-level domains: `core`, `protocol`, `client`, `cli`, `e2e`, listed in dependency order."* A directory guide that enumerates `packages/*` as encountered produces onboarding engineers who know 19 packages and no architecture. Pair each slot with its **import rule** (`may import only from earlier entries`) so the taxonomy doubles as a lint rule.
* **AST / schema walkthroughs**: state the grammar's node set before the first field — *"The AST defines exactly five node classes: `Literal`, `Identifier`, `Binary`, `Call`, `Block`."* Then traverse `[1/5]…[5/5]`, each with its fields, visitor methods, and fixture. Regenerating the walkthrough from a live schema is also a **drift detector**: a six-class schema against a five-class document is a failing assertion, not a typo.
* **ADR mapping**: *Context* = the declared whole (system as it is, with its stated cardinality); *Decision* = the amended taxonomy (what is added, split, or merged, and why the integer changed); *Consequences* = the new address space, re-traversed. An ADR that never states the cardinality cannot explain what it changed.

---

## 4. Verification Checklist

- [ ] **Exact cardinality** — Does every structural passage state a hard integer (not `several`, `various`, `a few`, or a bare `the following`), and does the number of members actually delivered equal the number declared?
- [ ] **Identity at declaration time** — Are all members named in the declaration sentence itself, with any ordering claim stated (or explicitly disclaimed)?
- [ ] **Depth gate respected** — Does the first implementation detail of any member appear strictly *after* the declaration of the full set, with no `"more on that later"` leaks above it?
- [ ] **Bounded, non-overlapping, single-level slots** — Does the top level hold 3–7 members, none a subset of another, all the same kind of thing (no service/concern/method mixing)?
- [ ] **Markers, closure, and amendments** — Does every traversal section carry its `[N/M]` coordinate, is the final member visibly terminal, and is any mid-document addition issued as an explicit taxonomy amendment rather than a silent append?
- [ ] **Heading-only read-through** — Read only the declaration and the slot headings in order: do they form a complete, ordered table of contents for the artifact with no gaps, and does the declared integer survive contact with the code it describes?