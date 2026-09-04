# Data Fountain — the concept of managed data architecture: from raw evidence to AI‐ready truth

**AI-ready.** This label indicates that the article may be used as a **Super Truth** reference surface for AI-assisted verification. The text is intended to be sufficiently precise and invariant that an AI does not fabricate an answer but rather checks it against stated definitions, boundaries and rules.

This does not, of course, eliminate AI risks altogether. It does, however, materially reduce the likelihood of hallucination: provided the source offers clear definitions, sound structure, traceable evidence and explicit policy boundaries, an AI is in a position to respond with “yes”, “no”, “partially” or “insufficient grounds” — rather than resorting to plausible-sounding improvisation.

**Governing principle:** article-as-reference, AI-as-checker, not AI-as-source-of-truth.

# Data Fountain — what it is and why it matters

**Data Fountain** is a cohesive architectural concept for building a governed data platform. It brings together the stronger parts of **Data Warehouse**, **Data Lake** / **Data Lakehouse**, **Data Mesh** / **Event Mesh**, **Medallion**, **Saga**, **WAP**, **UDM**, **Gold**, **Platinum** and **Super Truth** into a single controlled architectural composition: from raw evidence through to AI-ready truth, without shadow semantics, uncontrolled shortcuts or duplicated truth. It is worth being quite explicit here: Data Fountain is not a straight implementation of any one of these data patterns. Rather, it uses them as coordinated disciplines within a single model, each with its own place, remit and boundary.

The purpose of **Data Fountain** is to take what these approaches do well, avoid the characteristic traps each of them tends to bring in practice, and shape them into a distinct, ready-to-use architectural entity in its own right. In other words, the point is not to bolt together yet another stack of fashionable patterns, but to establish a model in which evidence, contracts, governance, AI-serving and consumer truth are deliberately separated and deliberately connected.

**Data Fountain** builds on the **Medallion Pattern** and retains its foundational principle: data should mature through states of increasing quality, trust and readiness. The classical three-tier structure **Bronze → Silver → Gold** is, however, extended with additional layers and sub-layers. That extension is not decorative; it is there because the distance between “cleaned data” and “business truth” is, in real production platforms, far too large to be left to convention, local SQL or good intentions.

# **Fountain composition: layers, isotopes and ore beneficiation**

## Why imagery matters: abstraction as an engineering instrument

A critic might ask: is this not rather a lot of imagery and metaphor?

It is worth establishing one important point at the outset: a complex data architecture simply cannot be held in one’s head as a mere list of components, services and tables. What is needed is not only precise engineering definitions but also strong abstract images that allow one to see — or indeed feel — the system as a coherent living organism.

In Data Fountain those images are the **fountain**, the **bowls**, the **water**, the **jets**, **ore beneficiation**, **pattern isotopes**, the **saga** and **Super Truth**. These are not decorative metaphors, nor an attempt to make the architecture “more poetic”. They are a means of reducing cognitive load: an engineer, analyst, architect or product owner must be able to grasp quickly where raw evidence sits, where quarantine lies, where readiness is assessed, where the semantic contract is forged, where advanced serving takes place, where the consumer product is packaged and where the final arbitration of meaning resides.

An abstract image works as a map. If an element has no intelligible place within the composition, that is a signal: one may be confusing a layer with a pattern, adding a superfluous “bowl”, creating shadow semantics, duplicating truth or attempting to force one pattern to do the work of another. In this sense, artistic coherence serves as an engineering check: whatever looks out of place in the image of the fountain frequently turns out to be a future operational risk in production.

This is precisely why Data Fountain employs two types of language, representation and definition simultaneously. **Abstract language** provides coherence: fountain, ore, isotope, saga, truth. **Engineering language** provides executability: contracts, gates, lineage, correlation IDs, WAP, quarantine, replay, feature definitions, policy boundaries and semantic authority. Only together do they make it possible not merely to explain the concept but to design it in such a way that a team can think, implement, verify and evolve the platform in a consistent manner.

These images and metaphors did not, however, appear from thin air. Patterns and their anti-patterns have always formed clear mental models in engineers’ minds — well before any formalisation. That is why we look at the origins of patterns: to understand which problems they solved, where their strength lies, where they degenerate into anti-patterns and how their Data Fountain-adapted definitions can take their proper place within the overall architectural composition. In other words: first we need a mental model, then historical context, and only after that a precise engineering composition of tiers, planes and adapted patterns.

## Historical origins of the Saga Pattern

In the beginning was the Saga. The Saga Pattern is not the oldest software pattern in the formal sense; it is better read as an ancient cultural archetype for governing a long narrative through successive episodes, consequences and compensations. The engineering pattern appeared considerably later, but the name captured the central idea with remarkable precision: a long process is not sustained by a single monolithic act, but consists of steps, each with its own action, consequence and possible recovery path.

Formally, in engineering, Saga was described by Hector Garcia-Molina and Kenneth Salem in their 1987 paper *Sagas* as an approach to long-lived **database** transactions rather than to microservices; the microservices context came much later, in the 2010s, notably through the work of Chris Richardson and the practice of distributed systems. A long process is broken into a sequence of local transactions, and compensating actions are defined for the steps already executed. Yet the word *saga* is no accident: it carries an older cultural image — a memorable story composed of episodes, heroes, decisions, errors, retribution and the restoration of order.

In the Scandinavian tradition, sagas grew out of oral transmission. The ancient Norse and Icelanders relied on recurring formulae, stable character roles and recognisable scenes to preserve complex narratives without a modern written workflow. The *Poetic Edda* preserved ancient mythological and heroic poems from the oral tradition, while Snorri Sturluson’s *Prose Edda* in the thirteenth century became a handbook explaining myths and poetic rules for subsequent generations. In this sense the original saga was not merely a text, but a **memory-management system**: a long story had to be broken into scenes, anchored to characters, have its key motifs repeated, and make it possible for both listener and narrator to restore the overall order even after an omission or variation.

It is precisely here that a useful analogy with the modern Saga Pattern arises: a long process is not sustained as one monolithic act, but consists of autonomous episodes with their own action, consequence and compensation. In a **choreography-based saga**, each participant reacts to an event according to its own role — rather like the Gods and Heroes of the sagas, where order emerges from rules of interaction, roles and events without a single conductor. In an **orchestration-based saga**, a coordinator appears who knows the route and holds the state — rather like the structured order of the *Prose Edda*, where the narrator organises the chaos of events into a comprehensible cosmos. The cultural form of the saga is, of course, not a technical discipline; it provided a precise metaphor for the problem that engineers formalised in 1987: how to sustain a long distributed process through local transactions, correlation, state, events, compensations and governed replay.д

### Core ideas of the Saga Pattern

- **A long process is broken into local steps.** Instead of a single large ACID transaction, we have a sequence of local transactions `T1 → T2 → … → Tn`, each committed within its own service or tier.
- **Every step has a compensation.** If a later step fails, actions already executed are not “magically rolled back”; they are compensated by explicit actions `Cn → … → C1`: cancel booking, reverse payment, deactivate publication, issue correction event.
- **Consistency becomes eventual rather than instantaneous.** The system may briefly reside in an intermediate state, but it must either reach successful completion or follow a controlled compensation path.
- **Coordination may be choreography or orchestration.** In choreography, participants react to events without a central coordinator. In orchestration, a coordinator explicitly governs the order of steps and error handling.
- **Statefulness and traceability are required.** The saga must know which step has been executed, which events have arrived, which version of the contract was in force, which compensations have already been triggered and where the process halted.
- **Idempotency is mandatory.** Re-executing a step or compensation must not create duplicates, double payments, double publications or a new version of truth.

### Strengths of the Saga Pattern

- **No long global locks are needed.** Unlike 2PC or long ACID transactions, a saga does not hold resources locked for the duration of the entire business process.
- **It scales better in microservices and data platforms.** Each service or tier can own its own data and its own local transaction.
- **It supports long-running business processes.** Sagas are well suited to processes lasting minutes, hours or days: booking, fulfilment, publication workflow, data product release, correction/replay.
- **It provides an explicit failure-handling model.** Rather than relying on hidden manual fixes, the architecture describes the compensation path and recovery behaviour up front.
- **It combines well with events, CDC, WAP and Kappa-safe replay.** In Data Fountain, Saga need not be a runtime engine; it can serve as a law of motion: correlation identifiers, readiness gates, compensation, auditability and replay discipline.

### Drawbacks and risks of the Saga Pattern

- **Compensations are not always perfect.** Not every action can honestly be “rolled back”: an email has already been sent, an external payment may have gone through, or a downstream consumer may have seen the intermediate state.
- **Eventual consistency must be acceptable to the business.** If the domain requires strict instantaneous consistency, Saga may be the wrong choice or may require additional guarantees.
- **Choreography can become an invisible web.** Without strong tracing, correlation IDs and documentation, it is difficult to understand who reacts to which event and why the process has stalled.
- **Orchestration can become a bottleneck.** A central coordinator simplifies understanding of the flow, but may create a single point of failure, tight coupling or excessive dependence on a workflow engine.
- **Engineering discipline is required.** Idempotency, retries, timeouts, dead-letter queues, poison messages, ordering, deduplication, versioning and observability are not options but baseline conditions for a safe Saga.

### Conclusion for Data Fountain

For Data Fountain, the Saga Pattern should be understood not as something akin to a “grand conductor-choreographer of all pipelines”, but as a clear **saga grammar of motion**: it defines how the long story of data passes through every level of the Data Plane, which steps are considered complete, which have compensation, which carry a correlation passport, which may be replayed and which must halt in quarantine.

## Pattern isotopes: a pattern for the patterns themselves

And it is here that we must note: **Data Fountain** employs an **isotope of the Saga Pattern**.

What does that mean? How does it differ from the pattern itself? And why is it needed at all?

### Why academic patterns are always half right

Every classical pattern is born as an answer to a specific problem. **Data Warehouse** answered the chaos of disparate operational systems. **Saga** answered the problem of partial completion in distributed processes. **WAP** answered the risk of silently overwriting production truth. Each of them, in its own context, was the right solution to the right problem.

But here a paradox arises: **when the problem is absent, the pattern becomes an anti-pattern**. If the situation does not call for full domain federation, Data Mesh can degenerate into organisational bureaucracy. If a pipeline has no complex distributed transactionality, an academic Saga can add more coordination overhead than real benefit. If every change passes through an equally heavy audit, WAP quickly becomes a point of congestion.

More dangerous still is that every pattern tends to become an anti-pattern precisely for those situations it never considered. Academic, textbook patterns are often written in an abstract dimension — without regard for *what stands alongside*: which tiers already exist, where the contract is formed, where canonical authority resides, where replay is needed, where a fast path suffices and where strict governance is required.

### What a pattern isotope is

There is, therefore, a problem with the patterns themselves: they cannot be transplanted into Data Fountain dogmatically, without regard to place, scale and neighbouring architectural disciplines. What is needed is a pattern for the patterns themselves — a way of taking a classical pattern, preserving its strong core, yet adapting it to a specific role within Data Fountain. This pattern of patterns is what the concept calls a **pattern isotope**.

In Data Fountain we work not with dogmatic implementations of patterns, but with their **isotopes**. This is a deliberate metaphor: we take a stable “chemical element” — the classical pattern — and alter the number of “neutrons”: trimming away unnecessary academicism or adding a specific capability so that it works precisely within our data environment.

A pattern isotope retains its general recognisability, but acquires new properties. Saga remains Saga, but need not become a runtime engine. WAP remains WAP, but does not turn into a full-inspection customs post for every byte. Data Vault 2.0 remains recognisable through canonical keys, relationships and historicity, but does not require building the full “religion” of Raw Vault / Business Vault where the heavy work has already been done at the Silver and UDM levels.

It is rather like an element of the fountain: each has its proper place. In the right place it strengthens the composition; out of place it looks awkward and creates operational risk. Data Fountain therefore does not ask, “which pattern is best in the abstract?” It asks, “which isotope of this pattern is needed here, in this particular architectural position?”

In Data Fountain, isotopes should be read as **architectural disciplines**, not as a list of components. Medallion sets the direction of truth maturation; Saga provides the grammar of motion, statefulness, replay and compensation; WAP gives the right to publication; Plumbum / Quarantine provides fail-closed isolation; UDM marks the semantic contract boundary; URDP governs reference semantics; Feature Store provides reusable AI/ML-serving representation; Semantic Layer marks the consumption/runtime boundary; Knowledge Graph makes relationships and reasoning explicit; ACL protects the boundary against source-specific semantic corruption; Data Catalog provides governed discoverability of canonical assets; and Fitness Functions continuously verify semantic invariants. Each has its own role, and none becomes a universal explanation of the whole platform.

## From batch warehouse to semantic truth: the genesis of data-maturation discipline

The Medallion Pattern did not emerge as a standalone academic theory in the manner of the relational model or the Saga Pattern. It is more accurately read as a practical engineering crystallisation of several older ideas: the staging area in the Data Warehouse, the raw zone in the Data Lake, curated / trusted datasets in the Lakehouse, data marts in Kimball-style analytics and multi-hop pipelines in distributed data engineering. The names **Bronze → Silver → Gold** became the recognisable language for this logic: raw arrival, cleansed and reconciled core, business-ready presentation.

In its modern form the Medallion Architecture was popularised by Databricks in the context of the Lakehouse as a pattern for the logical organisation of data and the progressive improvement of its structure and quality. It is important to note that Medallion did not “invent” the very fact of tiers: the DWH already had staging, warehouse core and data marts; the Kimball methodology formalised dimensional modelling and conformed dimensions; and the Lakehouse added cheap object storage, table formats, schema evolution, streaming, CDC and ML/AI scenarios. Medallion simply made this discipline simpler, more visual and more natural for a modern data platform.

The medallion metaphor works through the image of progressive refining: Bronze is still close to the raw material, Silver is already fit for reuse and analytical work, and Gold is a consumable business product. In this sense Medallion is not about “three folders” or a specific vendor stack, but about the discipline of data maturation: from evidence to trust, from trust to meaning, from meaning to consumption.

### What Medallion solves and why it works

The central problem Medallion seeks to resolve is the tension between **auditability** and **usability**. If everything is left raw, analysts and data scientists receive full evidence but are forced to clean, map and interpret the data afresh each time. If everything is immediately transformed into final showcases, the business gains convenience but the platform loses raw context, replayability and the ability to explain why a given KPI looks the way it does.

Medallion relieves this tension through separation of responsibilities:

- **Bronze** preserves the fact of arrival: what exactly arrived, when, from which source, with what payload and metadata. This prevents the loss of raw evidence and provides the basis for audit, debugging and replay.
- **Silver** cleanses, types, deduplicates, validates schema / quality and creates a reusable trusted foundation. This reduces the duplication of cleaning logic across downstream products.
- **Gold** publishes purpose-built business-ready models: KPIs, aggregates, dimensional models, data marts, reporting-ready or ML-ready datasets. This reduces the risk that each dashboard independently reinvents revenue, conversion or active customer.

The strengths of Medallion are therefore not a separate list of “pros”, but the direct consequence of its structure: a clear quality gradient, separation of concerns, replayability, auditability, reuse of Silver, compatibility with Lakehouse table formats and a shared organisational language for ownership, access control, SLA, lineage, cost management and data quality.

### Limitations and shortcomings of the Medallion Pattern

Medallion is strong as a discipline, but it is not a magical solution to every data-platform problem. It is frequently adopted formally: Bronze, Silver and Gold catalogues are created, yet contract gates, owners, quality expectations, lineage, lifecycle and publication rules are left undefined. In such cases Medallion quickly degenerates into “three folders with attractive names”.

- **Storage and compute overhead.** If data are mechanically copied through every tier without any real addition of quality, costs rise whilst benefit does not.
- **Latency.** Three or more hops can be superfluous for simple or low-latency use cases if there is no Fast Path or streaming-aware design.
- **Over-engineering.** For a single source and a single straightforward report, a full Medallion may be heavier than the task itself.
- **Weak business semantics upstream.** If Bronze and Silver are built purely technically, without domain context, Gold is forced to “catch up” on meaning belatedly.
- **Silver can become a dumping ground for staging tables.** If one does not define what cleaned, trusted and reusable actually mean, Silver turns into a repository of half-finished artefacts.
- **Gold can become renamed Silver.** If Gold has no consumer contract, KPI ownership or business purpose, it is not a data product but merely another curated table.
- **Does not resolve governance and ownership on its own.** Medallion shows a quality gradient, but does not guarantee domain ownership, semantic authority, federated governance or data-product discipline.
- **Is not a substitute for data modelling.** Gold still requires dimensional models, wide tables, feature tables, semantic views or other forms of modelling. Medallion indicates where maturation occurs, but does not abolish the question of how to model entities.

Proper use of Medallion demands discipline: do not add a tier unless it changes the state of trust; do not copy data “for show”; do not bypass Silver when building Gold; do not allow Gold to invent semantics without a contract; and do not confuse a naming convention with architecture.

### Medallion and Saga: different questions, one production-grade composition

Medallion and Saga are not competitors in any direct sense. **Medallion** describes the states of data maturation: raw, cleaned / trusted and business-ready. **Saga** describes governed motion through a long process: local steps, statefulness, correlation, retries, compensation, idempotency and the recovery path.

Put as briefly as possible: Medallion answers the question **where data move and what level of trust they currently have**, whereas Saga answers the question **how the process moves safely between those states, what happens when it fails and how it is recovered**.

| Aspect | Medallion | Saga |
| --- | --- | --- |
| Central question | In what state of quality and readiness do the data reside? | How does a long process move through steps and recover after failure? |
| Primary unit | Layer / dataset / table / data product | Step / transaction / event / compensation |
| Strength | Quality gradient, replay foundation, separation of concerns | Statefulness, idempotency, compensation, failure handling |
| Risk | Formal Bronze / Silver / Gold without contracts or ownership | Excessive orchestration or an invisible choreography web |
| In Data Fountain | Baseline maturation discipline for the tiers | Motion isotope between tiers: passport, gates, replay, compensation |

Conflict arises only when one pattern is made to do the work of the other. Medallion does not, by itself, explain what to do with a failed batch, a partially executed publish or a compensating correction. Saga does not define what Bronze, Silver, UDM, Gold or Semantic truth is. In Data Fountain they work together: Medallion provides the vertical of truth maturation, while Saga provides the process grammar for movement between the bowls of the fountain.

### Why Medallion is well suited as the foundation of Data Fountain

Data Fountain does not reject Medallion — quite the opposite: it takes it as the baseline metaphor for data maturation. Any governed data platform needs a clear path from raw evidence through to consumer-ready truth, and Medallion already provides that backbone: Bronze as raw truth, Silver as trusted readiness and Gold as consumption.

The classical triad is, however, not sufficient for Data Fountain. It does not provide an explicit fail-closed quarantine, it has no separate semantic forge between Silver and Gold, it does not distinguish AI/ML advanced serving from business Gold, it offers no upper-level arbitration through Super Truth, and it does not describe the process grammar of motion. Data Fountain therefore does not “break” Medallion; rather, it unfolds it into a fuller production-grade semantic architecture. The detailed list of levels is set out in the next section, “Baseline levels of Data Fountain”, so as not to duplicate the same structure here.

The main strength of this extension is that Data Fountain preserves Medallion’s clear quality gradient whilst closing the rather wide gap between “cleaned data” and “business truth”. This is precisely where Plumbum, UDM, URDP / reference semantics, Platinum, Gold and Super Truth appear — not as decorative extra layers, but as necessary boundaries of responsibility, ensuring that semantics are not accidentally created in dashboards, notebooks or feature pipelines.

### Medallion as a Data Fountain isotope

Within Data Fountain, Medallion operates as an isotope of the baseline maturation pattern. We do not take it dogmatically as “exactly three layers and always a single linear path”. We take its stable core: **data must move through states of increasing quality, trust and readiness**. That core is then adapted to Data Fountain through additional fountain bowls, contract gates, semantic authority, WAP publication and AI-ready serving.

Saga works alongside it as a different isotope — not a layer, but a law of motion. It does not compete with Medallion; it makes Medallion production-grade. Every transition receives a passport, readiness state, idempotency, compensation, replay capability and audit trail. WAP adds the right to publish, Plumbum adds a safe stop, and UDM and Super Truth add canonical meaning.

**Final position:** Data Fountain is neither a rejection of Medallion nor a simple “extension with more layers”. It is Medallion brought up to production-grade semantic architecture: with Plumbum for non-poisoning, UDM for canonical meaning, Platinum for AI/ML serving, Gold for data products, Super Truth for arbitration and Saga as the grammar of safe movement between all these states.

# **Anatomy of Data Fountain**

## The fountain bowls as a Medallion maturation cone

The classical **Medallion** pattern can be pictured as a flat two-dimensional figure: three concentric rings, **Bronze → Silver → Gold**. They show the gradient of quality, but they say rather little about what happens between the rings, why a transition requires effort, and why not every piece of material may move straight into the next state.

Once the discipline of maturation is added to that flat figure, Medallion ceases to be merely a diagram of three zones and becomes a **cone**. The rings become horizontal levels at different heights of trust: lower down sits raw evidence; higher up sits progressively cleaner, more reconciled, semantically shaped and consumption-ready truth. Data do not simply “flow” between layer names; they rise, passing through checks, cleansing, contracting, enrichment and publication.

In this geometry, the **fountain bowls** are the horizontal cross-sections of the cone. As in a real fountain, the bowls are placed one above another along the central axis: each has its own height, shape, capacity and overflow boundary. In the same way, each Data Fountain level fixes a distinct state of data maturation and a distinct boundary of responsibility. A lower bowl is not “worse”; it is simply doing a different job. Water cannot stably overflow upwards if the preceding bowl has not accepted it, held it and passed it on in a controlled fashion.

**Saga** adds the **direction of motion** to this cone. Data do not climb from Bronze to Gold along a straight vertical line; they move around the cone in a spiral. Each turn passes through statefulness, gates, validation, compensation and replay. In other words, the transition to the next horizontal cross-section is not a mechanical hop, but a completed cycle: the artifact must acquire state, pass readiness, preserve lineage, have a recovery path and only then rise higher.

It is precisely this model that makes the classical three-level scheme insufficient. If the cone contains only three large cross-sections — Bronze, Silver and Gold — the gaps of responsibility between them become far too wide. Between raw evidence and trusted flow there must be a place for material that must not seep upwards: this is where **Plumbum** appears. Between cleansed data and business meaning there must be an explicit contractual boundary: this is where **UDM** appears. Before broad consumption there must be a separate space for AI/ML-ready representations and advanced serving: this is where **Platinum** appears. And above consumer products there must be a level that arbitrates conflicts of meaning and fixes canonical interpretation: this is where **Semantic / Canonical Authority / Super Truth** appears.

This gives us the canonical order of levels: **Bronze → Plumbum → Silver → UDM → Platinum → Gold → Semantic / Canonical Authority / Super Truth**. This is not an arbitrary extension of the three Medallion rings, but the result of taking flat Medallion, raising it into a maturation cone and animating it with Saga motion. Additional horizontal cross-sections of responsibility become visible. Each bowl appears exactly where, without it, responsibility would remain hidden, mixed into a neighbouring level or pushed out into shadow logic.

### Data Fountain level gradient

- Each horizontal cross-section of the cone is a separate fountain bowl, with its own height of trust, boundary of responsibility and right not to pass material further upstream. Below is the canonical order of cross-sections, from the base of the cone to its apex.
    - **Bronze — fixation of raw truth.** The broadest bowl at the base of the cone. Here data are kept as close as possible to the fact of arrival: payload, metadata, lineage, manifest, correlation context and the ability to audit and replay. Bronze answers the honest question: what exactly arrived, when, from where and in what original form?
    - **Plumbum — fail-closed quarantine.** The first protective cross-section above Bronze. Material that fails validation — invalid, incomplete, policy-blocked, semantically dangerous or broken at any higher level — does not seep upwards, but stops here with a reason, owner, severity, repair path and lineage. Returning to the normal flow is not done by a direct downstream patch, but through Bronze-compatible replay or an official correction event.
    - **Silver — analytical readiness.** Here raw data are standardised, typed, deduplicated, subjected to schema enforcement and quality gates, and turned into contract-ready entities. Silver is no longer merely evidence, but it is not yet business semantics either: it is a cleansed and reusable foundation upon which meaning can safely be built.
    - **UDM — semantic forge.** The contractual boundary at which cleansed material receives its canonical business form: stable identity, granularity, business terms, relationships, allowed states, temporal validity and rules of interpretation. UDM stitches up the semantic leap between “cleaned” and “business-meaningful” which classical Medallion leaves rather too implicit.
    - **Platinum — advanced serving gate.** The cross-section where approved semantics become AI/ML-ready representations: features, embeddings, AI Signals, GraphRAG, semantic search and governed ML/AI serving. Platinum sits before Gold because these representations require their own readiness gate — lineage, policy, training-serving consistency and blast-radius control — before Gold packages them into consumer-ready products.
    - **Gold — consumer-ready serving.** Here approved semantics are packaged into data products: KPIs, dimensions, facts, dashboards, APIs, semantic views, master data products and governed business marts with an owner, contract, SLA/SLO, lifecycle and access policy. Gold does not invent truth; it publishes it in a form fit for consumption.
    - **Semantic / Canonical Authority / Super Truth — upper arbitration of meaning.** The narrowest bowl at the apex of the cone. This is where canonical terms, keys, relationships, reference/master semantics, interpretation rules, precedence and historical consistency are fixed. If Gold provides the consumable product, Super Truth ensures that different products do not create different versions of business truth.

## Why not a pure Data Warehouse?

### The core idea of the pattern

**Data Warehouse** is the classical pattern for building a centralised, structured and controlled analytical repository. Its baseline idea is straightforward: collect data from different operational systems, cleanse it, bring it into a shared model, load it into a single schema-first environment and then use that environment for reporting, BI, KPIs and management analytics. In its stronger form, the Data Warehouse attempted to be a **single source of truth**: not a set of disconnected tables, but a controlled model in which business terms, metrics, dimensions and history carry the same meaning across the organisation.

The key principle of the DWH is **schema-on-write**: before data enter the warehouse, their structure, types, relationships, business rules and history-management approach must be defined up front. Data are not merely stored “as they are”; they pass through ETL / ELT processes and are transformed into a coherent analytical model.

### Historical problems the Data Warehouse set out to solve

**The historical emergence of the Data Warehouse.** The roots of the Data Warehouse reach back into the 1960s and 1970s, when corporate analytics began to form the ideas of **dimensions** and **facts**, early dimensional data marts and an integrated repository for decision support. In 1983 Teradata released the first database system designed specifically for decision support, while the term **business data warehouse** was formally established in 1988 after the IBM Ireland paper by **Barry Devlin** and **Paul Murphy**, *“An architecture for a business and information system”*. In 1992 **Bill Inmon**, in *Building the Data Warehouse*, fixed the classical definition of a data warehouse as a **subject-oriented, non-volatile, integrated, time-variant** collection of data for supporting management decisions. In 1996 **Ralph Kimball**, in *The Data Warehouse Toolkit*, proposed a practical bottom-up approach through dimensional modelling, star schemas and data marts. Thus two influential methodologies took shape: the **Inmon approach** as a centralised top-down warehouse, and the **Kimball approach** as bottom-up construction through conformed data marts. For Data Fountain this historical tension matters not as an old argument, but as an early expression of the same dilemma: centralised canonical truth versus flexible domain-driven model growth.

- **Disparate operational systems.** CRM, ERP, billing, finance, HR and other systems lived separately, each with its own identifiers, formats and rules.
- **No consolidated view of the business.** Reports were assembled manually, figures did not reconcile, and answering a simple question could require work from several teams.
- **Analytical workloads harmed production systems.** Complex queries, joins and aggregations could not safely be run against operational databases.
- **Inconsistent business terminology.** “Active customer”, “revenue”, “conversion”, “deal” or “successful payment” could mean different things in different departments.
- **Lack of historical state.** Operational systems often stored only the current state, whereas analytics needed to answer the question “what was true at that point in time?”.
- **Poor reporting data quality.** The DWH introduced quality control, standardisation, deduplication, SCD dimensions and publication discipline for analytical data.

### Strengths of the Data Warehouse

- **A single controlled model.** The DWH creates a shared analytical foundation for the whole organisation.
- **High quality, predictability and enterprise stability.** Schema-on-write, ETL checks, standardisation and governance reduce reporting chaos and make the DWH a strong pattern for financial, regulatory and management analytics.
- **Strong support for BI and management analytics.** Relational models, dimensional modelling, star / snowflake schemas and OLAP approaches are well suited to classical dashboards and KPIs.
- **Historical correctness.** The DWH works well with slowly changing dimensions, state history and retrospective reporting.
- **Clear governance.** There is centralised responsibility for the model, access, quality, lineage and publication rules.

### Data Warehouse problems that led to other approaches

The issue is not that the Data Warehouse is “bad”. The issue is that it was created for a world in which data were mostly structured, the number of sources was smaller, change was slower and the primary consumer was BI reporting. Once clickstream, logs, JSON, events, IoT, ML/AI, real-time pipelines and petabyte-scale storage appeared, the pure DWH began to creak.

- **Schema-on-write rigidity.** A new field, a new source or new business logic often required changes to the model, ETL, tests and release cycle. This slowed the response to business needs.
- **Centralised bottleneck.** All changes passed through the DWH team, which became the narrow point between data producers and data consumers.
- **Poor fit for raw and unstructured data.** Logs, events, documents, images, text, semi-structured payloads and experimental datasets did not sit naturally in the classical relational model.
- **Loss of potentially useful signals.** Because data were transformed into the “correct” model on arrival, some raw information could be discarded before anyone understood its value for ML, AI or new analytics.
- **High scaling cost.** Classical DWH platforms were expensive, especially as data volumes grew rapidly into terabytes and petabytes.
- **Batch-first latency.** Many DWH processes were historically built around nightly or periodic loads. This is not a flaw of the model alone, but a problem of reaction time: even a high-quality and correct schema could be too late for low-latency, streaming and operational analytics scenarios.
- **Limited flexibility for Data Science.** Data scientists often need access to raw, detailed and not-yet-polished data for feature engineering, exploration and model training.

## Why not a pure Data Lake?

The Data Lake appeared as a natural response to the rigidity of the classical Data Warehouse: organisations needed cheap, scalable storage capable of accepting different data formats without full up-front modelling. It provided raw preservation, faster ingestion, support for semi-structured and unstructured data, and a better foundation for ML/AI and exploratory analytics.

### The core idea of the pattern

**Data Lake** is a pattern in which an organisation first stores the maximum amount of data in raw or near-raw form, and applies structure, interpretation and a specific model later, to suit a particular consumer’s task. Where the Data Warehouse says “model first, then load”, the Data Lake says: “do not lose the data first; the model will appear once it becomes clear what the data are needed for.”

The key principle of the Data Lake is **schema-on-read**: data may be stored in files, object storage, parquet/orc/csv/json/log/event formats, and the schema is applied at the moment of reading or processing. This makes it possible to onboard new sources more quickly, more cheaply and without full up-front modelling.

### Historical problems the Data Lake set out to solve

**The historical origins of the Data Lake.** The idea of the Data Lake was born as a reaction to the limitations of the classical Data Warehouse and Data Mart in the era of Big Data. In October 2010, James Dixon, then CTO and co-founder of Pentaho, described the Data Lake metaphor in his blog post [“Pentaho, Hadoop, and Data Lakes”](https://jamesdixon.wordpress.com/2010/10/14/pentaho-hadoop-and-data-lakes/). He contrasted the data mart — “a store of bottled water”, cleansed, packaged and structured for consumption — with the data lake as “a large body of water in a more natural state”, into which data flow from the source and various users may explore them, dive in or take samples.

The original meaning of this metaphor was not a chaotic dump of every conceivable file, but the preservation of raw material before it becomes clear which questions the business, analytics or data science will wish to ask. This became especially important against the backdrop of the Hadoop ecosystem, cheaper distributed / object storage and scenarios in which logs, clickstream, JSON, telemetry, text and other semi-structured or unstructured data did not sit naturally within the Data Warehouse’s schema-on-write model.

Dixon later returned to the subject in [“Data Lakes Revisited”](https://jamesdixon.wordpress.com/2014/09/25/data-lakes-revisited/), and overviews such as [Dataversity — “A Brief History of Data Lakes”](https://www.dataversity.net/articles/brief-history-data-lakes/) recorded the Data Lake as an evolutionary response to data marts and warehouses and an attempt to preserve diverse data without premature loss of potential value. Historically, the Data Lake was not “anti-DWH” but a correction to its rigidity: more raw flexibility, less premature modelling, yet with a new need for metadata, ownership, catalogue, quality gates and governance.

The **Data Lake** thus set out to solve the following problems:

- **The high cost of the classical DWH.** The Data Lake made it possible to store large volumes of data in cheap object storage without purchasing expensive monolithic DWH appliances.
- **The inability to accommodate the full diversity of data.** Logs, clickstream, JSON, events, text, documents, images, telemetry and IoT signals could now be stored without forcibly fitting them into a relational model.
- **The slowness of schema-on-write.** New sources could be onboarded more quickly: first lay down the raw data, then gradually work out their structure.
- **Data Science’s need for raw signals.** ML/AI teams needed detailed data for exploration, feature engineering and training, not just pre-aggregated BI showcases.
- **Growth in volumes to petabyte scale.** Object storage and distributed compute made it possible to scale storage and compute more flexibly than in classical DWH systems.

### Strengths of the Data Lake

- **Raw truth is preserved as fully as possible.** The Data Lake does not force premature discarding of fields, payloads or signals whose value may only become apparent later.
- **Flexibility and speed of ingestion.** New sources can be onboarded without a lengthy modelling cycle.
- **Support for diverse formats.** Structured, semi-structured and unstructured data can coexist within a single storage approach.
- **A better foundation for ML/AI.** Data scientists can work with detailed events, logs, behavioural signals and experimental datasets.
- **Cheap storage scaling.** Object storage makes it possible to keep large historical volumes without the cost of a classical enterprise DWH.
- **More flexible separation of storage and compute.** In modern cloud / object-storage implementations, data can reside in a single store while different engines process them for different tasks.

### Data Lake problems that led to the emergence of Data Lakehouse and other approaches

The Data Lake solved the problem of flexible data preservation, but it often failed to solve the problem of trust in those data. If the Data Warehouse was sometimes too rigid, the Data Lake in a weak implementation became too permissive.

- **Data Swamp instead of Data Lake.** Without cataloguing, ownership, quality gates, metadata, contracts, lifecycle rules and governance, the lake quickly turns into a swamp of files in which it is unclear what is trusted, what is deprecated and what must never be used.
- **Absence of unified business semantics.** The Data Lake stores data well, but it does not, of itself, answer the question of what “customer”, “revenue”, “conversion” or “activity” actually means.
- **Low quality without publication discipline.** If anything can be placed in the lake but there are no quality gates, promotion rules or a clear mechanism for transition to a trusted state, downstream consumers receive chaos.
- **Difficulty for BI consumers.** Raw files and semi-structured payloads are not a convenient interface for dashboards, KPIs and regular management reporting.
- **ACID, concurrency and update problems.** A classical file-based lake coped poorly with transactionality, concurrent writes, merge/update/delete, time travel and stable table versions.
- **Duplication and proliferation of copies.** Teams frequently created their own derived datasets without unified lineage, spawning several “truths” on top of a single raw store.

It was precisely these problems that created the demand for the **Data Lakehouse**: an attempt to combine the cheap and flexible storage of the Data Lake with the transactionality, tables, governance, performance and BI-readiness of the Data Warehouse. The Lakehouse did not abolish the Data Lake; it attempted to discipline it so that raw flexibility would not degenerate into semantic chaos.

## Why not a pure Data Lakehouse?

**Data Lakehouse** emerged as a response to the weaknesses of the Data Lake: it attempts to combine a cheap, scalable object-storage environment with the discipline of the Data Warehouse — tables, transactionality, ACID behaviour, schema evolution, time travel, governance, performance optimisation and BI-readiness.

### The core idea of the pattern

The baseline idea of the Lakehouse is not to maintain, on the one hand, a “swamp of raw files” and, on the other, an expensive DWH, but to build a single analytical platform on top of lake storage, where data can move from raw to trusted and consumer-ready states without constant copying between incompatible worlds.

The Lakehouse adds a table format / metadata layer to the Data Lake: Delta Lake, Apache Iceberg, Apache Hudi or comparable mechanisms. As a result, files in object storage begin to behave not merely as a collection of parquet/json/csv objects, but as governed tables with versions, transactions, schemas, partitioning, compaction and controlled change.

### Historical problems the Data Lakehouse set out to solve

**The history of the term in its modern form.** Data Lakehouse was born out of disappointment with the first generation of Data Lakes: cheap storage and Hadoop / Spark made it possible to store almost everything, but without transactionality, a catalogue, governance, quality gates and intelligible semantics, many lakes rather quickly became data swamps. Lakehouse was the answer to that limit: retain lake storage as a cheap and open foundation, whilst adding warehouse-like discipline to it.

The term *lakehouse*, in its modern architectural sense, became established around 2019–2021 with the appearance of table formats and transactional storage layers such as Delta Lake, Apache Iceberg and Apache Hudi. They added table-level behaviour to lake storage — a metadata layer, snapshots, governed schemas and transactional semantics — without requiring the data to be moved into a classical warehouse.

Databricks substantially popularised the term in its article [“What is a Data Lakehouse?”](https://www.databricks.com/blog/what-is-data-lakehouse), while the idea received academic form in the CIDR 2021 paper by Armbrust, Ghodsi, Xin and Zaharia, [“Lakehouse: A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics”](https://www.cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf). In this context, Lakehouse should not be read as a marketing sum of “lake + warehouse”, but as a response to architectural fatigue with the two-layer model: a raw lake for data science on one side, a warehouse for BI on the other, and between them copies, latency, stale data, divergent SQL / semantics, divergent security policies and high operational cost.

Thus, **Data Lakehouse** set out to solve the following problems:

- **Data Swamp.** Lakehouse attempted to discipline the lake through tables, metadata, catalogues, quality rules and governed data-maturation zones.
- **The split between Lake and Warehouse.** Organisations often had a raw lake for data science and a separate DWH for BI, creating duplication, latency, different versions of truth and complex ETL copies.
- **Absence of ACID and stable versions.** Table formats added transactionality, snapshots, time travel, merge / update / delete and more reliable operation with concurrent pipelines.
- **The Data Lake was awkward for BI.** Lakehouse made lake data look more like SQL tables that BI engines, dashboards and analysts could consume.
- **The need for a common foundation for BI and ML/AI.** Lakehouse promised that the same trusted datasets could be used for dashboards, feature engineering and model training.

### Strengths of the Data Lakehouse

- **Combines Lake flexibility with Warehouse discipline.** Data can be stored cheaply and at scale, whilst still being represented as tables with schemas, transactions and historical versions.
- **Less copying between systems.** The Lakehouse reduces the need to keep moving data from the lake into a warehouse merely to make it suitable for SQL and BI consumption.
- **Better support for streaming and batch.** Modern table formats make it possible to build pipelines that work with both batch processing and near-real-time updates.
- **Time travel and reproducibility.** Historical table states can be reproduced, pipelines can be debugged more reliably, and analytics can be placed on a rather firmer footing.
- **A common technical foundation for different consumers.** BI, data science, ML/AI, data engineering and advanced analytics can work on top of shared curated datasets rather than a collection of isolated replicas.

### Data Lakehouse problems that led to other approaches

The Lakehouse greatly improved the technical layer of the Data Lake, but it did not automatically resolve the organisational, product and semantic problems of data. It answers the question “how do we store and process tables reliably in the lake?”, but it does not by itself guarantee that the organisation has the right ownership, contracts, semantics and consumer-ready data products.

- **Semantics do not appear automatically.** A table format may guarantee transactionality, but it does not explain what business entities, metrics and interpretation rules actually mean.
- **Ownership and organisational weight.** If all changes still pass through one data-platform team, the Lakehouse can reproduce the centralised bottleneck of the classical DWH: technically powerful, but organisationally heavy.
- **Data products do not emerge of their own accord.** The presence of curated tables does not mean they have SLAs, contracts, documentation, lifecycle, ownership or a clear value proposition for consumers.
- **Governance may be technical without being federated.** Access policies and a catalogue are only part of governance; responsibility rules, domain ownership, policy-as-code and product-level control are still required.
- **Performance and cost management are not free.** Partitioning, clustering, compaction, file sizing, caching and workload isolation require continuing engineering discipline.

It was precisely these limitations that created demand for **Data Mesh**: an approach that shifts the emphasis from a single centralised platform to domain ownership, data products, a self-serve platform and federated computational governance. Data Mesh does not abolish the Lakehouse; rather, it says that a technical platform is not enough if data have no owners, contracts or product accountability.

## Why not a pure Data Mesh / Event Mesh?

**Data Mesh** and **Event Mesh** appear as answers to the limitations of centralised platforms: even a technically strong Lakehouse does not, by itself, guarantee the right ownership, fast domain evolution, living event-driven integration or consumer-first data products. It is important, however, to distinguish two related but different variants: **Data Mesh** as an organisational and product pattern for analytical data, and **Event Mesh** as an event-integration pattern for the movement of change between systems.

### Variant 1: Data Mesh

**Data Mesh** moves responsibility for data closer to the domains. Its central idea is that the teams which best understand a particular business area should not merely generate data for a central data team, but should own those data as **data products** — with contracts, SLAs, documentation, quality rules, lifecycle and intelligible consumer interfaces.

Data Mesh consists of several baseline principles: **domain ownership**, **data as a product**, **self-serve data platform** and **federated computational governance**. In other words, domains are responsible for the meaning and quality of their data products, the platform gives them the means to publish those products, and governance is not centralised as manual gatekeeping, but automated through policy-as-code, metadata, contracts and shared standards.

### Historical problems that Data Mesh set out to solve

**The historical emergence of Data Mesh.** Data Mesh appeared towards the end of the 2010s as a response to large organisations’ fatigue with centralised data platforms. Data Warehouse, then Data Lake and Lakehouse, removed part of the technical constraint, but often left the organisational bottleneck intact: one central data team was expected to integrate, cleanse, explain and support data for every domain. The concept was formulated by Zhamak Dehghani at Thoughtworks. In 2019 she published [“How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh”](https://martinfowler.com/articles/data-monolith-to-mesh.html), and in 2020 the approach was refined in [“Data Mesh Principles and Logical Architecture”](https://www.thoughtworks.com/insights/blog/data-mesh/data-mesh-principles-and-logical-architecture), where the principles acquired a clearer logical form.

The original historical context matters: Data Mesh was not born as yet another storage pattern or vendor stack. Its root cause was socio-technical. In [“Data Mesh: Delivering Data-Driven Value at Scale”](https://www.thoughtworks.com/insights/books/data-mesh), Dehghani describes Data Mesh as a paradigm for delivering data-driven value at scale, rather than as a replacement for Lakehouse technologies. The essential shift is that domains become first-class owners of analytical data, while the central platform is expected to provide self-service capability and automated guardrails.

**Contemporary use.** By the mid-2020s, Data Mesh is used mainly in larger organisations with many domains, data sources and consumers, where a centralised data team no longer scales organisationally. Practice has shown that Data Mesh is rarely best introduced as a grand, all-at-once transformation. Thoughtworks, in [“Data Mesh in practice: Getting off to the right start”](https://www.thoughtworks.com/insights/articles/data-mesh-in-practice-getting-off-to-the-right-start), describes a more pragmatic route: start with a clear data strategy, select specific domains and use cases, build thin slices of data products, and then gradually grow the operating model, platform capabilities and governance. Practice-oriented reviews likewise note that early adopters have tended to be scale-ups and enterprises — including e-commerce, financial services and large digital organisations — suffering from bottlenecks in central data systems and central data teams.

Modern Data Mesh is therefore best read not as an alternative to the Lakehouse, but as an organisational and product layer above, or alongside, it. Lakehouse may provide tables, transactionality, catalogues, performance and a batch / streaming foundation; Data Mesh adds the questions that the technical layer rather conspicuously leaves open: who owns the data product, what contract it has, which SLA / SLO applies, how it is documented, how quality is measured, what its lifecycle is, which consumer interface it exposes and who is accountable when meaning changes.

Thus, **Data Mesh** set out to solve the following problems:

- **The central data team as bottleneck.** In DWH / Lakehouse models, all changes often passed through a single team which could not possibly understand every domain in equal depth.
- **Distance between data engineers and business context.** A central team could build technically correct tables, yet still miss the finer semantics of sales, marketing, billing, attribution or operations.
- **Data without product accountability.** A table might exist without an owner, SLA, description, lifecycle, compatibility rules or intelligible consumer contract.
- **Slow organisational scaling.** The more domains and sources appeared, the harder it became to maintain all pipelines, models, metrics and rules centrally.

### Strengths of Data Mesh

- **Ownership sits where the knowledge lives.** Domain teams understand their entities, events, metrics and edge cases better than a distant central platform team is ever likely to do.
- **Data products rather than “just tables”.** Consumers receive a governed product with a contract, quality expectations and support, not a raw technical artefact left to be interpreted locally.
- **Better organisational scaling.** New domains can develop their own products without permanently waiting in the queue of a central data team.
- **Governance does not disappear; it becomes federated.** The rules remain shared, but they are applied automatically and closer to the place where data are produced and understood.

### Variant 2: Event Mesh

**Event Mesh** is a different, more runtime-oriented expression of mesh thinking. If Data Mesh answers the question “who owns analytical data products and how should they be consumed?”, Event Mesh answers a rather different question: “how do events and changes move between domains, services and platforms in near-real time?”

Its core idea is that systems should not integrate only through batch exports, ETL or point-to-point APIs. They publish and consume **events** through a governed event fabric: topics, streams, brokers, routers, event contracts, schemas, replay, subscriptions and policy controls. In such a model, a state change becomes an event which other domains may use without tightly coupling producer and consumer.

### Historical problems that Event Mesh set out to solve

**The historical emergence of Event Mesh.** Event Mesh grew out of a longer evolution of enterprise integration and event-driven architecture. During the 1990s and 2000s, organisations sought to reduce point-to-point integration chaos through message-oriented middleware, publish / subscribe models, Enterprise Service Bus and integration brokers. With the arrival of microservices, cloud, SaaS, IoT, mobile channels and global digital platforms, however, the centralised ESB increasingly became a bottleneck. A more distributed, cloud- and hybrid-ready model was needed for domain events and near-real-time system response.

In its modern sense, the term *Event Mesh* has become established as an architectural approach for building a dynamic network of event brokers which routes events between applications, devices, clouds, regions and domains, regardless of where those participants are deployed. Solace has popularised this definition through the idea of an event-broker network, whilst CNCF CloudEvents captured an important precondition: scalable event-driven integration needs a standardised form of event metadata rather than vendor-specific understandings.

Technically, Event Mesh became viable through the maturity of distributed streaming and messaging platforms. Apache Kafka popularised the durable distributed commit log and stream processing, whilst NATS, Pulsar, RabbitMQ, cloud pub/sub services and commercial event brokers developed different forms of routing, federation, geo-distribution and protocol bridging. Crucially, Event Mesh is neither a single product nor simply “a Kafka cluster”; it is an architectural form in which events have contracts, routing, policy, observability, replay or retention strategy, and producers and consumers remain loosely coupled.

**Contemporary use.** By the mid-2020s, Event Mesh is used wherever business events must move quickly between many systems: payments and fraud detection, order fulfilment, inventory, IoT telemetry, operational alerts, real-time personalisation, CDC pipelines and event-fed data products. In the cloud-native world it is often combined with event-driven microservices, serverless, stream processing, schema registries, CloudEvents, OpenTelemetry, policy-as-code and data contracts.

For Data Fountain, Event Mesh matters not as a self-sufficient “truth”, but as transport and as the operational nervous system of change. Data Fountain takes from Event Mesh low-latency event movement, loose coupling, CDC / Event Sourcing, replay and reactive pipelines, but passes those events through Bronze, Plumbum, Silver, UDM, Platinum, Gold and Semantic gates so that a runtime event becomes not merely fast, but governed business truth.

Thus, **Event Mesh** set out to solve the following problems:

- **Point-to-point integration chaos.** Many direct API links between systems created a fragile web of dependencies.
- **Batch latency.** Business events reached downstream systems too late — sometimes hours or days after the fact.
- **Weak system reactivity.** Without events, it is difficult to build real-time recommendations, alerts, fraud detection, operational automation and AI signals.
- **Loss of change history.** If a system stores only the current state, it is hard to reconstruct exactly what happened and when.

### **Strengths of Event Mesh**

- **Low-latency movement of change.** Events can be delivered in near real time, allowing downstream systems to react whilst the business context is still current.
- **Loose coupling between producers and consumers.** One domain publishes events, and many consumers may read them without a direct integration to the source system.
- **Replay and auditability.** Event history makes it possible to replay changes, reconstruct state and debug pipelines with rather more confidence than with current-state snapshots alone.
- **A natural foundation for CDC, Event Sourcing and Kappa Architecture.** Event Mesh fits well with streaming architectures and low-latency data products, provided that contracts, ordering and replay discipline are properly governed.

### Problems of Data Mesh and Event Mesh that make them insufficient on their own

- **Data Mesh can become an organisational declaration without a platform.** If there is no self-serve tooling, metadata, contracts, quality gates and automated governance, domains receive responsibility without the means to discharge it properly.
- **Semantic fragmentation without canonical authority.** If every domain defines “customer”, “revenue” or “activity” for itself, federated ownership does not remove the need for canonical semantics, master / reference rules and a single arbitration of meaning.
- **Event Mesh is not analytical truth.** Events are excellent at recording change, but they do not, by themselves, create curated, historical, semantic or consumer-ready models.
- **Event contracts are difficult to evolve.** Versioning, schema compatibility, ordering, idempotency, deduplication and replay require strict engineering discipline; otherwise the event fabric merely distributes inconsistency faster.
- **Operational complexity grows at scale.** Even with a self-serve platform, many domains, products, topics, streams, policies and contracts require a strong control plane; without it, mesh becomes distributed disorder rather than distributed ownership.

Data Fountain therefore does not copy Data Mesh or Event Mesh directly. It takes from **Data Mesh** domain ownership, data products and federated computational governance, and from **Event Mesh** the event-driven movement of change, replay, CDC / Event Sourcing and low-latency pipelines. On top of that, however, it adds its own tier discipline from Bronze to Super Truth: contract gates, quarantine, semantic forge, AI-serving, Gold data products and canonical authority.

## Historical fairness and the contemporary dilemma

It would be rather unfair to look down on these patterns from the comfortable vantage point of 2026. Each of them — **Data Warehouse**, **Data Lake**, **Data Lakehouse**, **Data Mesh** and **Event Mesh** — was, in its own time, a genuine advance in engineering thought. The Warehouse brought order to the chaos of disparate databases and gave the business, for the first time, a single version of truth. The Lake democratised access to data and opened the door to Data Science. The Lakehouse attempted to combine inexpensive storage with transactional discipline. Mesh moved ownership closer to the domains, whilst Event Mesh made the movement of change live, reactive and much closer to real time. Each of these steps was perfectly logical for its historical moment and did, in fact, move the industry forward.

But it was not only architectural thinking that moved on. The very nature of working with data changed as well: Machine Learning ceased to be a laboratory curiosity, Deep Learning opened up new classes of problem, and AI and LLMs have, within just a few years, altered what we expect from a data platform. Today it is no longer enough for a platform merely to feed BI reports. It must support feature engineering, real-time inference, embeddings, semantic search and AI-driven automation — whilst still remaining governed, auditable, explainable and cost-effective.

And it is precisely here that one encounters a dilemma rather well captured by an old joke: *“should I go with the clever ones or the pretty ones?”* A modern data team is often not choosing between a good option and a bad one, but between different sorts of compromise. If one wants strict semantics, governance and stable reporting, one reaches for the Warehouse — and accepts rigidity and slower change. If one wants to store everything and avoid losing potentially useful ML/AI signals, one builds a Lake — and accepts the risk of a Data Swamp and semantic disorder. If one wants domain autonomy or reactivity, one turns to Mesh or Event Mesh — and receives fragmented meaning or fast events without any guarantee of business-aligned truth. As a result, many platforms live in a state of “least bad” compromise: “yes, we have a Data Swamp, but at least we lose nothing”; “yes, the DWH is hard to change, but at least the reports are stable”. This is not an engineering failure. It is the natural consequence of the fact that each historical pattern was optimised for a particular set of problems, whereas a contemporary data platform has to answer all of them at once.

The question, then, is no longer which of the classical patterns has “won”. They were all right in their time, and they have all left useful engineering inheritance. The real question is different: how does one build an architecture that does not force the platform to choose between the “clever” and the “pretty”, between governance and flexibility, between raw truth and semantic truth, between BI and AI, between centralised quality and domain autonomy? It is precisely from this dilemma that the need for **Data Fountain** arises — not as yet another fashionable label, but as an attempt to assemble historically strong ideas into a single governed model without accepting their old compromises as unavoidable evils.

## Data Fountain — not a variation, but a different logic

Each of the classical approaches optimises a single axis. **Warehouse** bets on schema-first canonical truth — one model, one team, one cadence. **Lake** bets on store-first flexibility — meaning is born ad hoc, to suit the task at hand. **Lakehouse** adds table discipline to the lake, yet semantics still do not come first. **Mesh** shifts ownership to domains, but canonical authority is absent. **Medallion** provides a quality gradient, yet three layers leave the semantic leap between “cleaned data” and “business truth” implicit. Each of these logics is correct within its own focus and incomplete beyond it.

**The logic of Data Fountain is different: composition-first.** No single pattern is a self-sufficient solution, yet none is superfluous either. Data Fountain takes the stable core of each — its isotope — and places it in the bowl where it is strongest. Bronze is naturally closer to the Lake. Silver, UDM and part of Gold carry the discipline of the Warehouse and the Lakehouse. Platinum is bound up with the Feature Store, AI/ML and future-facing consumption. Gold is responsible for consumer-ready products. Semantic is responsible for canonical meaning and Super Truth. Event-oriented flows are closer to Event Mesh, whilst ownership and data products are closer to Data Mesh. Yet the platform is not compelled to be a “pure” instance of any one of them: each level takes the nature it requires without destroying the coherence of the overall composition.

The levels of Data Fountain are not folders and not a naming convention. Each level is an autonomous architectural bowl with the right to halt the flow: its own responsibility, its own readiness criterion, its own form of control metadata and its own right not to pass data further. Isotopes are not layers alongside levels, but stable architectural disciplines that pass through the bowls and acquire a concrete form at each address: Saga formalises motion, WAP controls publication, Kappa guards against the splitting of truth between batch and streaming, DV2.0 lite disciplines identity and history, URDP governs reference semantics, Feature Store materialises AI-ready representations, Semantic Layer publishes the consumption boundary, Knowledge Graph makes meaning traversable, ACL protects canonical semantics from source-specific corruption, Data Catalog makes canonical assets discoverable, Fitness Functions continuously verify semantic invariants, and Super Truth arbitrates canonical interpretation.

The transition between levels is not a mechanical hop. It is a completed cycle with readiness, lineage, compensation and audit: an artefact must acquire state, pass a gate, preserve correlation context, have a recovery path and only then rise higher. This is precisely why Data Fountain does not ask “which pattern is best in the general case?” but rather “which isotope of this pattern is needed precisely here?”

**Canonical semantic authority** is a further distinction from mere variation. Meaning is not born in a dashboard, a prompt or an ETL job; it is fixed in a contract — UDM → URDP → Super Truth — and only then projected into SQL, BI, AI, Feature Store or a Gold product. If different consumers receive different meanings for the same business term, that is not “different points of view” — it is shadow semantics, which Data Fountain removes through contract-first order.

**AI-readiness as an architectural requirement, not an option.** In a world where neural networks, LLMs, RAG, GraphRAG, agents and MCP integrations are becoming full consumers of data alongside human beings, the quality of semantics ceases to be a matter of convenience and becomes a matter of mathematical correctness of the result. A human analyst is able to compensate for imprecision: spot an odd number in a dashboard, check the SQL, clarify the context and find a local assumption. An AI consumer works differently: it constructs an answer with whatever precision the input context permits. If canonical terms, mappings, relationships and rules are blurred, duplicated or contradictory, the model will not “err slightly” — it may construct a confident, formally consistent yet semantically wrong answer that is difficult to detect precisely because it looks plausible. Data Fountain therefore designs semantic cleanliness not as a bonus for BI, but as a precondition for AI-ready truth: every AI consumer must receive unambiguous, contract-grounded, traceable context and respond on the basis of approved meaning rather than a statistical guess atop inconsistent sources.

Cognitive load here is an architectural metric, not an aesthetic judgement. If a new element does not fit the image of the fountain — has no intelligible place, no direction of motion and no purpose — that is a signal of operational risk: unclear responsibility, duplicated truth, a weak contract, a bypass pipeline or a layer without a clear role. The architectural composition must be sufficiently coherent for engineers and analysts to grasp quickly: where raw truth is born, where data are checked, where semantic truth is formed, where they become a product and where they are used for advanced serving.

This is therefore not a variation. A variation is when a couple of extra layers are added to Medallion. A different logic is when the very principle changes: instead of choosing between “the clever ones and the good-looking ones” — governance versus flexibility, BI versus AI, centralised versus federated — Data Fountain gives each problem its own bowl and its own place, preserving the coherence of the composition. The residual risk becomes not architectural but input-driven: if the data are so poor that the system cannot process them, it can always say that **the data are not fountain-grade**.

# Ore beneficiation of data

## Data as ore, not a finished product

The fountain metaphor explains the composition of Data Fountain: levels, cascades, overflows and controlled motion. But the internal logic of how data pass through the system is best explained by a different metaphor — **ore beneficiation**.

Ore contains useful material, but it also contains gangue, impurities, slag and uncertainty. No one attempts to produce a piece of jewellery directly from raw ore. It passes through a specific conveyor, and each stage of that conveyor has a precise analogue in working with data.

Everything begins with **extraction**: ore is mined from the earth, data are extracted from source systems, CDC streams, APIs, files and events in their original form. Next comes **crushing**: large lumps of rock are broken into smaller fragments, raw payloads are parsed into individual records, fields, events and structural units. Then comes **screening**: the crushed material is sorted by size and type, records pass through schema validation, type checks and policy screening, and whatever fails to pass the sieve is screened out into quarantine — not discarded, but isolated for analysis and repair.

After screening, beneficiation proper begins. **Gravity separation** separates the heavier useful fraction from the lighter barren rock — in much the same way, deduplication and noise removal separate duplicates, technical noise and test records from useful business signals. **Flotation** raises target minerals to the surface by means of chemical treatment — data are standardised, typed, enriched with reference context and acquire canonical form.

Next comes **refining**: the concentrate is purified to high purity, and cleansed data receive canonical identity, business terms, grain, relationships and contract gates. And finally — **minting**: the pure metal is turned into a coin, an ingot or a piece of jewellery, and contracted truth is packaged into data products, KPIs, APIs, dashboards, features and AI-ready representations.

Raw data work in exactly the same way as ore. They may contain valuable business signals, but alongside those signals come duplicates, errors, invalid records, source-specific noise, incomplete identifiers, conflicting interpretations and semantic uncertainty. The task of Data Fountain is not to force this ore straight into a Gold dashboard or an AI model, but to pass it through a controlled beneficiation conveyor in which each stage does its own work and does not encroach upon its neighbour.

### The beneficiation conveyor as metaphor, not a repetition of level definitions

The formal roles of Bronze, Plumbum, Silver, UDM, Platinum, Gold and Semantic / Super Truth have already been defined in the section “Baseline levels of Data Fountain”. Here the same sequence is used solely as a beneficiation metaphor: raw material does not become a product directly, but passes through controlled cleansing, slag isolation, contract formation, advanced serving, product packaging and canonical arbitration.

## Architectural elegance of the flow

This chain removes the central problem of classical repositories — the attempt to force raw data straight into complex models. Data Warehouse frequently demanded modelling too early. Data Lake frequently permitted too long a period of indecision. Data Fountain separates readiness into levels and gives each level its own responsibility.

**Bronze** does not attempt to be clever. **Plumbum** does not halt the entire flow; it isolates specific problems and accepts the “scrap” of failed products for re-smelting. **Silver** does not invent business meaning. **UDM** forges the contract. **Platinum** prepares approved semantics for Feature Store, AI/ML and future-facing serving. **Gold** turns the cleansed and beneficiated material into consumable products. **Semantic / Super Truth** holds the reference specimens of meaning and arbitrates canonical truth against which those products may be reproduced consistently.

### Three laws of beneficiation

**The law of concentration:** at each successive level, the concentration of trusted truth increases and noise decreases. An attempt to leap from Bronze to Gold without Silver / UDM is an attempt to produce a piece of jewellery from unrefined ore.

**The law of conservation:** slag does not vanish without trace. Plumbum preserves problematic records with a reason, owner and repair path. Candidate versions in WAP are likewise not lost: they are either published or remain available for analysis.

**The law of isotopic correspondence:** each pattern isotope operates at its own level and does not encroach upon its neighbours. Saga formalises the rules of motion, WAP controls publication, UDM defines the semantic contract, URDP enriches reference context, Super Truth arbitrates meaning as canonical authority, and Gold packages the product for the consumer.

### Final formula

**Data Fountain is a purpose-built beneficiation conveyor for data, from raw ore through to finished products, in which each level is a fully-fledged level in its own right: it has its own responsibility, its own boundaries, its own contract gates and the right not to pass data further if they are not yet sufficiently mature.**

**Each level of Data Fountain is a level in its own right, not merely an intermediate folder within one large pipeline.** Bronze, Plumbum, Silver, UDM, Platinum, Gold and Semantic / Super Truth have their own purpose, their own readiness criterion, their own form of control metadata and their own right to halt or redirect the flow. A level does not become “real” merely because there is a subsequent level after it; it is a completed architectural bowl that accepts a particular state of data, applies its own discipline and passes on only what meets its contract gates.

Each level implements its own set of **pattern isotopes**. Bronze naturally carries raw truth, manifest, lineage, Event Sourcing / CDC and replay-safe discipline. Plumbum implements fail-closed quarantine, a recovery contract, forensic evidence and controlled rerun. Silver applies quality gates, standardisation, deduplication, analytical readiness and Kappa-safe processing. UDM implements the semantic contract, canonical terms, stable identity and predicate-like rule discipline. Platinum takes approved semantics and transforms them into Feature Store, embeddings, AI Signals, GraphRAG / Hybrid RAG and advanced serving. Gold packages approved meaning into consumer-ready data products, KPIs, dimensions, facts, APIs and dashboards. Semantic / Super Truth arbitrates canonical meaning, reference rules, master semantics and conflicts of truth.

The final logic is therefore as follows: **levels are the autonomous bowls of the fountain, and isotopes are the stable architectural disciplines that pass through those bowls and acquire a concrete form wherever they are needed.** This is precisely what allows Data Fountain to be not a haphazard mixture of Medallion, Lakehouse, Mesh, WAP, Saga, DV2.0 lite, URDP and AI-serving, but a single governed composition: each level has its own completeness, each isotope has its own identity, and together they hold the platform as a coherent architecture of truth maturation.

If the incoming data are sufficiently good, the fountain must accept them, pass them through controlled levels of beneficiation and deliver them to consumers in a fit form. If, however, they are so poor, contradictory or incomplete that even a governed architecture cannot make anything honest of them, the platform must have the right to say so plainly: **the data are not fountain-grade**.

# **Canonical Semantic First — meaning determines structure**

## The short formula of the principle

**Canonical Semantic First** means: in Data Fountain the first unit of architectural thinking is not a SQL query, not a table, not a dashboard field and not a prompt, but a **canonical semantic term** with a contract, interpretation rules, lineage, versioning, ownership and a clear address within the fountain’s architecture. The order is always the same: **term → contract → rules → representation**. First the business term is fixed in the language of the domain; then its contract with grain, keys, owner, lineage and quality expectations; then the rules of mapping, inference, precedence and replay; and only after that do SQL views, tables, BI models, Feature Store definitions, embeddings, graph projections, API payloads or AI-context appear.

SQL, BI views, Feature Store, embeddings, vector indexes, GraphRAG, API and agentic workflows must be derivative projections of canonical semantics, not independent places where business meaning is born afresh. Canonical terms acquire a concrete form at each level of the fountain: UDM forms the contractual boundary for entities, naming, granularity, keys, relationships and rules; URDP governs reference mappings and interpretation rules; Super Truth arbitrates approved meaning in conflicting or business-binding contexts; Gold packages approved semantics into consumer-ready products; Platinum materialises AI-ready representations. None of these levels is the sole address of the principle — Canonical Semantic First operates end-to-end through all isotopes.

## Three historical lines of working with meaning and their synthesis in Data Fountain

**The frame of this section.** Canonical Semantic First rests not on a single technology, but on the synthesis of three historical lines of working with meaning: **SQL / relational** as declarative computation over relations, **predicate logic / Prolog / Datalog** as the discipline of facts, predicates, rules and inference, and **LISP-like symbolic structures** as the idea of composable, machine-transformable semantic artefacts. This section shows why each line on its own is insufficient, yet together they explain the order **term → contract → rules → representation** for every isotope of Data Fountain.

### Historical context: relational theory, SQL and the unfinished compromise

It is worth beginning with historical fairness. Edgar Codd’s relational model was not a “language of tables”, but a mathematical idea about data as *relations*, *tuples*, *attributes*, *predicates* and *constraints* — that is, a logical model close to first-order predicate logic. In the classical paper [E. F. Codd, “A Relational Model of Data for Large Shared Data Banks”](https://dl.acm.org/doi/10.1145/362384.362685), the key point was not the tabular form itself, but the separation of logical meaning from the physical organisation of data: the user should work with a declarative model of facts and relationships, not with the way the machine happens to store records on disc.

SQL emerged as an engineering success — a language for accessing relational systems. IBM describes this path through System R, Don Chamberlin, Ray Boyce and SEQUEL / SQL in its historical reference [IBM — The relational database](https://www.ibm.com/history/relational-database). But it is precisely here that the unfinished compromise was born: the industry needed a language close enough to the relational idea to be declarative and portable, yet pragmatic enough to be accepted by vendors, optimisers, existing file/index structures, application programmers and the enterprise market. SQL became not a pure implementation of relational theory, but a working agreement between mathematical elegance, productivity, commercial compatibility, syntactic convenience and the need to migrate real systems quickly.

It is telling that SQL:2023 adds JSON and Property Graph Queries / SQL/PGQ, effectively acknowledging the need to work not only with classical tables but also with graphs, semi-structured data and new forms of semantic traversal. See the overview [Gerald Venzl — Announcing the general availability of the SQL:2023 Standard](https://www.geraldonit.com/announcing-the-general-availability-of-the-sql2023-standard/) and the catalogue [ISO/IEC 9075:2023](https://www.iso.org/standard/76583.html). For Data Fountain this is an important signal: even the SQL standard itself is moving towards richer data representations, but it does so as an access and computation layer, not as a canonical layer of business meaning.

The critique of the gap between SQL and relational theory is well known from the work of C. J. Date and Hugh Darwen, notably [The Third Manifesto](https://www.thethirdmanifesto.com/). Their position matters for Data Fountain not as a call to “abandon SQL”, but as a reminder: SQL is a strong instrument of computation, access, compatibility and migration, yet it ought not to be the sole carrier of business semantics. If business meaning lives only in SQL text, it becomes dependent on dialect, optimiser behaviour, implicit casts, null-handling, the BI tool and the local conventions of a particular team.

### Why the diversity of SQL dialects is a symptom, not an accident

The modern industry has many SQL-like languages and dialects: classical ANSI/ISO SQL, T-SQL, PL/SQL, PostgreSQL SQL, BigQuery SQL, Spark SQL, Flink SQL, ksqlDB / KSQL, CQL and others. Some are optimised for OLTP, others for analytics, streaming, distributed compute, columnar storage, event streams or a specific vendor runtime.

This is not simply “poor standardisation”. It is a consequence of the fact that SQL simultaneously performs several roles: access language, transformation language, analytics language, operational integration language and, frequently, an informal semantic layer. When a single language is used both for physical access and for business meaning, each runtime begins to pull it towards its own compromises.

In Data Fountain this means: **SQL must remain the execution / query interface, but must not be the canonical source of meaning**. If “active customer”, “revenue”, “conversion”, “synthetic contract”, “canonical customer identity” or “valid attribution window” exist only as a SQL fragment in a dashboard or job, that is not semantics — it is shadow logic.

### Before AI this was tolerable. After ML / DL / LLM — it no longer is

Before the widespread use of ML, Deep Learning, LLM, RAG and agentic workflows, the gap between SQL practice and stricter semantic / predicate logic was not always critical. A BI report could be checked by hand, a SQL query could be pinned in a dashboard, and differences between dialects often remained a local engineering problem.

But the modern AI stack works differently. RAG, GraphRAG, semantic search, vector databases, Feature Store, agents and MCP-style integrations require not merely tables, but **stable meanings, terms, relationships, rules, evidence and permissions**. If those meanings are reconstructed afresh each time from different SQL queries, prompts, BI expressions and ETL fragments, the platform begins to pay a double price: computational and semantic.

The computational price is the repeated re-computation of the same concepts in different pipelines. The semantic price is the risk that different AI / BI / ML consumers receive different meanings for one and the same business term. For classical reporting this produced “different numbers in different dashboards”. For AI it can already produce **semantic chaos**: incorrect context retrieval, wrong embeddings, inconsistent features, erroneous agent decisions and poorly explainable answers.

### Prolog-like, Datalog-like and predicate-oriented thinking as a bridge to AI

Symbolic AI, expert systems and logic programming were historically built around facts, terms, predicates, rules and inference. Prolog has its roots in first-order logic and was one of the classical instruments of AI, knowledge representation and expert systems. See for reference [SWI-Prolog](https://www.swi-prolog.org/) and the general description of Prolog. This line matters not out of nostalgia for old AI, but because it reminds us of a simple thing: knowledge becomes manageable when it has explicit facts, terms, predicates and rules, rather than dissolving into random projections, prompts or SQL fragments.

A close example for the data world is Datalog: it combines a relational view of data with logical rules and recursion. This is important for the UDM level, because it shows an alternative optic: business meaning can be represented not as a set of ad hoc SQL queries, but as a governed set of terms, predicates, rules and constraints. See the academic overview [Maier, Tekle, Kifer, Warren — Datalog: Concepts, History, and Outlook](https://dl.acm.org/doi/10.1145/3459664.3459667).

This does not mean that Data Fountain must rewrite its platform runtime in Prolog or Datalog. The idea is different: **naming canonical terms at the UDM level ought to be closer to canonical term / predicate / contract discipline than to a random SQL projection**. SQL may execute the rules, but the rules themselves should live in a versioned semantic control plane.

### The LISP world: a parallel branch of working with form, sets and computable semantics

It is worth noting separately one further historical universe — **LISP**. If SQL became the mass language of access to relational data, and Prolog / Datalog showed the predicate-oriented path to knowledge representation, then LISP took a different route: programme and data share a common form, structures can be built, transformed and executed as expressions, and lists, trees, symbols, sets, rules and functions become material for the system’s own reasoning. In this sense LISP is not simply an “old AI language”, but a separate civilisation of working with symbolic structures, where meaning can not only be stored but also programmatically restructured.

For Data Fountain this matters as a third optic alongside SQL and predicate logic. SQL handles declarative query over relations well. Prolog / Datalog handles facts → predicates → rules → inference well. LISP-like thinking adds a further property: semantic artefacts can be composable data structures that are easy to version, generate, transform, verify and pass between runtimes. This is close to our idea of canonical semantic terms: a business term should not be a textual annotation to a column, but a structured object from which one can generate a SQL view, a graph projection, a feature definition, a validation rule, an API contract or an AI-context.

The success of this branch is not purely academic. AutoCAD has lived for decades with AutoLISP / Visual LISP as a mechanism for extension and automation; it is one of the strongest examples of a long-lived platform where an internal language gave users a way to formalise their own domain logic. Clojure showed a modern form of LISP thinking on the JVM: immutable data structures, functional composition, REPL-driven development and practical integration with enterprise runtimes. Alongside it stand Erlang and Elixir as a kindred functional line: not LISP in syntax, but very close in the discipline of working with immutable state, message passing, fault tolerance and computable processes as governed structures. See [Clojure — Rationale](https://clojure.org/about/rationale), [Erlang documentation — Concurrent Programming](https://www.erlang.org/doc/system/conc_prog.html) and [Elixir — Introduction](https://elixir-lang.org/getting-started/introduction.html).

The architectural conclusion is straightforward: Data Fountain need not become a LISP platform, just as it need not become a Prolog platform. But the key architectural points that form meaning — the UDM level, the URDP discipline and Super Truth — must think of semantics as a structured, computable, composable artefact. Then a canonical term is not “a column name with a description”, but a small formal unit of meaning: it can be compared, verified, assembled into a larger structure, reproduced in another runtime and explained to an AI without loss of sense.

### Synthesis of the three historical lines: why a canonical term requires SQL, predicates and symbolic form

If one brings these historical lines together, the result is not an academic excursion but a practical architectural requirement. SQL gave a mass declarative language for queries, compatibility and execution. Prolog / Datalog gave the discipline of facts, predicates, rules and inference. The LISP world gave the idea that knowledge artefacts can be structured expressions which the system not only reads but also restructures. For Data Fountain this means: canonical semantics must be simultaneously intelligible to the business, sufficiently formal for rules and sufficiently structured for the generation of different physical and runtime representations.

It is precisely here that Data Fountain cuts through the old compromise. We do not transfer business meaning into yet another SQL dialect, do not hide it in dashboard expressions and do not entrust an LLM to “guess from context”. We build a small, clear, governed set of canonical semantic terms, relationships and rules, and make all other forms — SQL, graph, vector, features, API, BI, prompt context — derivative. This is not a complication but a simplification: a single canonical form generates many technical representations, instead of many technical representations accidentally imitating a single business truth.

GraphRAG and vector databases in this logic are not a fourth historical line, but a modern stress test of the synthesis. VectorDB and embeddings are useful for similarity search, clustering, retrieval and recommendations, but they are not canonical semantics. If an embedding is built on top of non-canonical terms, inconsistent definitions or shadow SQL logic, it merely accelerates search across chaos. GraphRAG shows why AI-context requires all three lines simultaneously: SQL-like execution for access and computation, predicate-like rules for explainable relationships and symbolic / graph structures for traversable semantic memory.

Embeddings should therefore index already ordered meaning, rather than substitute for the definition of meaning. It is easier, cheaper and safer to build AI, calculations, KPIs, features and GraphRAG on top of a clear set of canonical semantic terms than on top of imprecisely defined concepts in which the logic wanders “from” and “to”.

### Architectural conclusion: from three lines to the principle of Canonical Semantic First

The three historical lines thus converge not in a separate UDM conclusion, but in the general principle of **Canonical Semantic First**. SQL gives Data Fountain its execution / query form; predicate-oriented thinking gives the discipline for facts, terms, rules, constraints and inference; LISP-like symbolic structures give the form of composable semantic artefacts that can be generated, verified, versioned and transferred between runtimes. Together they explain why the primary unit of architecture must be not a table, a query, a dashboard expression, a feature or a prompt, but a canonical semantic term with a contract and rules.

The practical order therefore remains the same for all isotopes: **term → contract → rules → representation**. First the business term is fixed in the language of the domain; then its contract with grain, keys, owner, lineage, versioning and quality expectations; then the rules of mapping, inference, precedence, temporal validity, conflict resolution and replay; and only after that do SQL views, tables, graph projections, Feature Store definitions, embeddings, vector indexes, BI models, API payloads, GraphRAG-context or agentic workflow context appear.

At different levels of the fountain this order acquires a different concrete form. The UDM level forms the semantic contract boundary for canonical entities, their naming, granularity, keys, relationships and allowed states. URDP as a semantic control plane discipline governs reference mappings, vocabularies, effective-dated rules and interpretation rules. Super Truth arbitrates approved meaning in conflicting or business-binding contexts. Gold packages approved semantics into consumer-ready products, and Platinum materialises AI-ready representations — features, embeddings, GraphRAG / Hybrid RAG context, semantic search and governed ML/AI-serving.

This means: no single architectural point is the sole address of the principle. The UDM level must not turn into an intermediate store of curated tables without its own contract discipline; its proper role is a **semantic forge**, where cleansed material from Silver receives its canonical contractual form. But equally, Feature Store must not become a parallel truth, Gold must not independently invent KPIs, Semantic Layer must not be the sole place where metric logic resides, and an LLM prompt must not become a hidden source of a business rule.

# **Canonical Regulations of Data Fountain**

**Data Fountain** is a discipline for transforming data into governed business truth: from raw evidence to semantic contract, from semantic contract to data product, from data product to AI/BI/ML-serving — without shadow rules, hidden SQL versions of truth or uncontrolled shortcuts, yet with a **canonical term backed by a contract**, where tables, SQL, embeddings, dashboards, features, graph projections and prompts are merely forms of expression for already approved meaning.

Thus we are now in a position to formulate a clear set of regulations defining rules, approaches and definitions, drawing upon all that has been set out above as well as the pattern isotopes that we employ.

### Level 0. Axioms of composition: what the regulations fix before the isotopes

This level establishes the logic of the entire regulatory framework: the principles below are neither a summary of the isotopes nor a substitute for the super-section “Pattern isotopes”. They form the canonical rules by which isotopes subsequently extend, detail and implement Data Fountain within specific architectural disciplines.

1. **Composition-first.** Data Fountain does not select one “principal” pattern and attempt to turn it into a universal answer. It gives each problem its own bowl, its own plane or its own isotope. No pattern is a self-sufficient solution, but neither is any superfluous, provided it has the right architectural address.
2. **Bowl autonomy.** Every level of Data Fountain is a fully-fledged architectural bowl with its own responsibility, readiness criteria, control metadata, governance envelope and the right to halt the flow. A level does not become “real” merely because another level follows it.
3. **The three laws of beneficiation as the regulatory foundation.** The regulations implement the Law of Concentration, the Law of Conservation and the Law of Isotopic Correspondence: trusted truth must increase at each level; slag, failed candidates and negative evidence do not vanish without trace; every isotope operates where it strengthens the composition rather than supplanting its neighbours.
4. **Canonical Semantic First.** The primary unit of architecture is the canonical semantic term, not the table, SQL expression, dashboard, feature, embedding or prompt. The order is invariable: **term → contract → rules → representation**. All runtime forms are derivatives of approved meaning.
5. **AI-readiness as a cross-cutting requirement.** AI, LLM, RAG, GraphRAG, MCP and agents do not compensate for imprecise semantics in the way a human analyst sometimes can. Semantic clarity, lineage, policy boundaries, point-in-time correctness and explainability are therefore requirements of the entire architecture, not merely of advanced serving and the Feature Plane.
6. **Cognitive load as an architectural metric.** If a new component, contract, product, feature, graph projection or serving boundary has no intelligible place, direction of motion and purpose within the fountain, that is not an aesthetic problem but a signal of operational risk: shadow semantics, duplicated truth, undefined ownership or a bypass pipeline.
7. **The right to say “the data are not fountain-grade”.** If the incoming data are so poor, contradictory, incomplete or policy-unsafe that even a governed architecture cannot honestly make business truth of them, the platform has the right to halt the flow and record the residual risk directly: the data are not fountain-grade.
8. **Observability as an architectural requirement.** The platform must be observable at every level and within every plane: freshness, lag, throughput, quality drift, schema drift, cost signals, consumer usage, incident signals, lineage completeness and policy coverage must be measurable rather than assumed. Without observability, readiness gates, WAP decisions, governance assurance and Super Truth arbitration operate blind. Observability is not a separate plane; it is a cross-cutting property that must be supported by the Data Plane through runtime metrics, the Control Plane through state drift and feedback loops, the Governance Plane through coverage and audit evidence, the Truth Recovery Plane through remediation state, recovery success rate and replay completeness, the Super Truth Plane through decision freshness, arbitration coverage and conflict resolution rate, and the Feature Plane through freshness, serving health and training-serving parity signals.
9. **Cost-awareness as an architectural discipline.** Every layer, materialisation, copy, feature, serving endpoint or additional hop must have an architectural reason. One ought not to add a level unless it changes the state of trust; one ought not to create a materialisation if a governed zero-copy contract is sufficient; one ought not to duplicate a pipeline if Kappa-safe replay covers the use case. Storage, compute and operational overhead are part of the architectural decision, not a side-effect to be noticed later.

### Level 1. The semantic imperative: the laws of business truth

1. **The principle of single arbitration / Super Truth.** Data Fountain admits many local truths: Bronze as evidence of arrival, Silver as a cleansed technical state, UDM as an entity contract, URDP as versioned mapping semantics, Gold as a consumer-ready product. But the final right of business arbitration belongs solely to **Super Truth** as canonical authority. It is Super Truth that determines which interpretation is approved for business actions.
2. **The prohibition of shadow logic / No Vendor, BI or Prompt Lock-in of Meaning.** No BI dashboard, SQL query, reverse ETL, application script, vendor tool or LLM prompt may be the sole place of residence of a business rule. KPIs, mappings, ownership, attribution logic, customer definitions and segmentation rules must reside in governed semantic contracts, URDP, metric contracts, data product contracts or Super Truth decision records. The **Semantic Layer** may serve approved metrics in a tool-neutral manner, but it is not the birthplace of canonical meaning.
3. **The principle of governed AI / Contract-Grounded Reasoning.** AI, LLM, MCP and agentic workflows do not create truth and do not invent business rules. They may explain, route, search, summarise and assist in decision-making, but only on the basis of approved truth, documented contracts, lineage, evidence context and permitted policy boundaries.
4. **The principle of semantic addressability.** Every metric, entity, mapping, feature or data product must have an address in the architecture: where the raw fact is born, where it is cleansed, where it receives a contract, where it is enriched with reference semantics, where it is historicised, where it is published and where it is consumed. If a semantic artefact has no address, it is a risk.
5. **The principle that consumer truth does not equal source truth.** A raw source may be evidence, but it is not necessarily business truth. Business truth arises only after passage through a controlled path: Bronze evidence → quarantine / quality gates → Silver readiness → UDM contract → URDP / Super Truth semantics → governed publication.
6. **The principle of SQL as an execution interface.** SQL remains the obligatory execution / query interface for compatibility, BI, ad hoc analytics and optimised execution. Business semantics, however, must not be born in an SQL dialect, a BI expression or a vendor tool. The diversity of SQL dialects is a symptom of the fact that one language carries rather too many roles at once. The canonical form of meaning must be term → contract → rule → lineage → physical / query representation.

### Level 2. Trust discipline: the laws of publication

This level regulates the boundary between what the system has technically built and what it permits consumers to use.

1. **The principle of controlled release / WAP Boundary.** Write does not equal Publish. A new version of a table, slice, mapping, feature or data product initially exists as a candidate state. It becomes published truth only after audit, validation, compatibility checks, impact assessment and the execution of the publication policy.
2. **Protection against phantom reads / No Candidate Leakage.** Business consumers, dashboards, ML jobs, AI agents and downstream services must not read candidate state directly. They work solely through governed published contracts, stable views, approved snapshots or explicitly permitted sandbox / test interfaces.
3. **Risk-based WAP: Fast Path and Slow Path.** WAP in Data Fountain is not a full-inspection customs post for every byte. If a change is schema-compatible, the mapping version is known, quality metrics are within threshold, lineage is complete and the blast radius is low, it may proceed via the Fast Path. If new semantics appear, an unknown mapping version, a sharp delta, a governance conflict or a high impact, the Slow Path is engaged with manual review or escalation.
4. **The principle of atomic publication.** A consumer must see a coherent approved version of a data product or contract, not an intermediate collection of partially updated tables. The published state must be stable, reproducible and compatible with the declared contract version.
5. **The principle of blast-radius awareness.** Any change to a rule, mapping, schema, feature or KPI must assess whom it affects: dashboards, downstream products, AI models, historical reports, access policies, financial reporting or operational automation.

### Level 3. Hygiene and immutability: the laws of quarantine

This level protects the conveyor from poisoning, prevents uncertain data from seeping into trusted layers and guarantees the possibility of replay.

1. **The principle of fail-closed isolation / Plumbum Quarantine.** Dangerous, invalid, incomplete, policy-blocked, semantically suspicious data or the “scrap of a failed product” — unsuccessful candidate versions, broken transformations, data products or model fragments — do not pass through readiness gates between levels automatically: Bronze → Silver, Silver → UDM, UDM → Platinum, Platinum → Gold or Gold → Semantic / Super Truth. They are isolated in Plumbum with a full reason, error context, source payload or failed artefact context, correlation identifiers, owner, severity, repair path and a decision on their further fate.
2. **Absolute Bronze rerun / Zero Direct Patching.** Direct manual editing of Silver, UDM, Gold, Feature Store or Semantic artefacts is prohibited as a means of “fixing truth”. Following an administrator’s decision or a permitted auto-remediation, the change must return to controlled replay from the Bronze / raw evidence level or via an officially permitted correction event.
3. **The principle of evidentiary preservation.** Bad data are not deleted silently. Quarantine preserves not merely the fact of the error but the evidence: the original payload, metadata, validation result, rule version, mapping version, time of arrival, source, pipeline step and decision history.
4. **The principle of non-poisoning of subsequent layers.** Silver, UDM, Gold, Feature Store and AI-serving must not compensate for poor quality through local hacks. If a record cannot be honestly interpreted, it must be halted, isolated or marked in accordance with the contract policy rather than silently “patched in” downstream.
5. **Human-in-the-loop for semantic conflicts.** Automation may correct only technical or pre-described errors. New mappings, non-trivial semantic conflicts, changes to business meaning, canonical arbitration and quarantine decisions with high impact are taken by a human owner or governance authority.

### Level 4. Semantic stability: the laws of form

This level ensures that entities have a clear form before data products, KPIs, ML features or AI-context are built from them.

1. **The principle of contract supremacy / UDM & URDP.** The physical identity, granularity and structure of entities are fixed by UDM contracts. Business semantics, reference mappings, canonical vocabularies, effective-dated rules and rules of interpretation are governed by URDP or an equivalent versioned semantic control plane.
2. **Historical rigour / Temporal Integrity.** A change to a mapping or business rule must not silently rewrite closed historical periods. Every rule must have an effective date, version, lineage and the ability to answer the question “what truth was in force at a given moment?”
3. **The principle of stable identity / DV2.0 lite.** Canonical keys, relationships, hub/link-like structures and replay-safe identity must be stable regardless of changes to source IDs, business labels or downstream presentation. Data Vault 2.0 lite is used not as a full modelling religion but as a discipline of identity, relationship and history where it strengthens UDM and Gold.
4. **The principle of SCD correctness for consumer history.** Gold dimensions and consumer-facing facts must reflect the historically correct state of business semantics. SCD does not arbitrate truth of its own accord; it publishes the historical version approved by UDM / URDP / Super Truth.
5. **The principle of schema evolution without semantic surprise.** A technical schema change must not silently alter business meaning. Backward / forward compatibility, deprecated fields, default logic, nullability, enum evolution and mapping changes must pass through contract review in proportion to their impact.
6. **The principle of term maturity / Maturity Gate.** If a term cannot be described as term → contract → rules → representation, it has not yet matured to UDM. If two SQL queries produce a similar result yet rest upon different semantic assumptions, they represent either two different terms or a conflict requiring Super Truth / governance arbitration.
7. **The principle of composable semantic artefacts.** A canonical term is not a textual annotation on a column but a structured object: definition, owner, version, effective period, relationship to neighbouring terms and policy boundary. From it are derived SQL views, graph projections, feature definitions, validation rules, API contracts and AI-context. A single canonical form in place of the repeated re-computation of meaning in every pipeline.

### Level 5. Distributed orchestration: the laws of coordination

This level defines the physics of execution for all the preceding principles: how data move, how they are tracked, how they are replayed and how they are not lost in a distributed system.

1. **The principle of passport control / Saga Statefulness.** The movement of data is governed not by a single monolithic conductor but by a set of obligatory identifiers and states: `correlation_id`, `run_id`, `batch_id`, `source_event_id`, `schema_version`, `mapping_version`, `contract_version`, `publish_version`. Every transition must be traceable.
2. **The principle of Kappa-safe replay.** Batch, CDC and streaming must not have three different business implementations. If a state needs to be reproduced, replay must pass through the same semantic implementation or a formally equivalent path, so that a rerun does not create a new truth.
3. **The principle of Event Sourcing / CDC fidelity to the source.** For external systems where one does not control the domain events, a CDC-first approach with audit and replay is the natural choice. For internal critical workflows, domain events as the primary source of change are permissible. Both approaches must preserve ordering, idempotency, deduplication and causality.
4. **The principle of idempotent processing.** A rerun of a pipeline step must not create duplicates, double publications or different results without a change in input evidence, rule version or contract version.
5. **The principle of compensation rather than hidden correction.** If a step was erroneous, the system must create an explicit compensation or correction path with lineage rather than silently rewriting downstream state.

### Level 6. Product consumption: the laws of Gold, Data Products and Universal Resources

This level defines how approved semantics are turned into consumable products rather than merely “yet another table”.

1. **A data product does not equal a table.** A Gold artefact becomes a data product only when it has consumer value, an owner, a contract, SLA / SLO, documentation, quality expectations, a lifecycle, an access policy and intelligible rules of evolution.
2. **Gold does not create truth of its own accord.** Gold packages and publishes approved semantics for particular consumer scenarios. If Gold begins to invent KPIs, mappings or business rules on its own initiative, it breaches Super Truth and creates shadow logic.
3. **Universal Resource / Master Data Product as consumer-first master semantics.** Master / reference semantics must be presented not as a technical reference for engineers but as a governed consumer-ready resource: stable, versioned, documented, historical and suitable for reuse across products.
4. **MDM as governed identity and master serving.** Master entities — customer, account, product, merchant, channel, provider or other reusable business subjects — cannot simply be a “golden record” without rules. The MDM isotope requires explicit identity resolution, source crosswalk, matching logic, survivorship, merge / split policy, hierarchy / relationship rules, stewardship, confidence, temporal validity, governance profile, authority reference to Super Truth and history-aware serving through governed views, APIs, packages and AI context. A golden record without these rules is shadow master truth.
5. **Product registry as the control point of Gold.** The Gold shelf must have a registry for product\_id, type, owner, lifecycle, contract, authority reference, governance profile, lineage, serving forms and composition rules. Without a registry, data products degenerate into a random assortment of tables, APIs, dashboards and packages with no unified product accountability.
6. **Packaging discipline before product proliferation.** A new dashboard, alert, tenant configuration, API, report or embedded view is usually a slice / package / serving change rather than a new base product. A new base product is needed only when a new reusable semantic meaning, a new consumer contract or a new authority-backed product boundary appears.
7. **Zero-copy before copy-first.** If a consumer can work with a governed shared asset without physical duplication, one ought to choose zero-copy sharing, stable views or governed contracts. Copies are permissible only when they have an explicit reason, an owner, a lifecycle and lineage.
8. **Federated computational governance.** Access, masking, row / column restrictions, policy classification and sensitivity controls must be computed on the basis of metadata, contracts and policies rather than configured manually for each individual product.

### Level 7. Advanced serving: the laws of Platinum, Feature Store and AI / ML

This level describes the use of approved truth in ML, AI Signals, feature-oriented consumption, semantic search, embeddings and real-time inference.

1. **Platinum as a trust gate.** Platinum does not merely materialise AI / ML-ready representations; it is a readiness boundary: approved semantics from UDM / URDP / Super Truth pass through a trust / readiness check before broad reuse in Feature Store, embeddings, GraphRAG, semantic search, MCP / LLM and Gold products. Without this gate, AI-facing surfaces may consume candidate or stale state as production context.
2. **Feature Store is not a parallel truth.** Feature Store re-uses approved entities, canonical keys, UDM / URDP semantics and lineage. It must not independently invent customer identity, attribution mapping or KPI logic.
3. **Training-serving consistency.** A feature used for training must be semantically compatible with the feature used for inference. Batch and real-time paths must converge upon the same contract-grounded logic.
4. **Point-in-time correctness.** Training datasets, feature sets, embeddings-derived signals and AI-context must not see future values relative to the moment of prediction or decision. Every feature or signal influencing a model or agentic workflow must have as-of semantics, event time / processing time discipline, leakage checks and lineage to the approved contract version.
5. **A feature is not a column.** A production feature must have a stable name, business meaning, owner, entity key, grain, version, transformation logic, freshness expectation, offline / online availability, lineage, quality checks, governance profile and deprecation policy. If a feature exists solely as an ad hoc SQL expression or a notebook column, it is not yet a production feature.
6. **Embeddings index meaning, not replace it.** VectorDB and embeddings are useful for retrieval and similarity, but they are not the arbiter of truth. They must index canonical meaning rather than accelerate search across chaos.
7. **GraphRAG requires a graph of meaning.** For multi-hop reasoning, explainability and entity relationships, AI-context must rest upon approved entities, relationships and rules rather than merely chunks and vector similarity.
8. **Knowledge Graph / Ontology as graph-discoverable meaning.** Approved meaning must be graph-traversable and impact-analysable: canonical entities, concepts, relationships, mappings, features, products, metrics, policies and Super Truth decisions must form a semantic lineage graph. A change to a mapping, concept, feature definition, product contract or decision must show which KPIs, products, dashboards, prompts, policies, explanations and AI Signals are affected.
9. **Explainability before magic.** If an AI decision influences a business action, it must be possible to explain which data, rule versions, features and semantic contracts were used.
10. **Headless BI / Semantic Layer as a governed metric serving boundary.** Metrics must not be born in a dashboard calculation, a BI workbook, a prompt template or ad hoc SQL. Every production metric requires a definition contract comprising formula, grain, owner, version, lineage and authority reference. The Semantic Layer publishes approved metrics in a tool-neutral manner, but it is not the birthplace of canonical meaning and it does not supplant UDM / URDP / Super Truth. For official KPIs, customer-facing numbers and executive decisions, the Semantic Layer must have an authority escalation contract: when metric serving suffices as approved consumption truth and when a Canonical Authority / Super Truth decision is required.
11. **Refusal is a valid platform behaviour.** If for AI, BI or an agentic workflow there is no approved source, canonical definition, readiness, policy permission or sufficient evidence context, the correct behaviour is refusal or escalation rather than improvisation. “I cannot answer safely” is a sign of a mature platform, not a failure of UX.
12. **The dual cost of absent canonical semantics.** Without canonical terms the platform pays a computational cost (the same concepts re-computed in different pipelines, jobs and models) and a semantic cost (different AI / BI / ML consumers receive different meanings for the same business term). Canonical Semantic First removes both costs simultaneously.

### Level 8. Isotopic correspondence of patterns: the laws of composition

This level fixes how the child patterns of Data Fountain are systematised within a single architecture and do not supplant one another.

1. **The law of address.** Every isotope has a single architectural address: a level, plane or boundary where it is strongest. In its proper place an isotope strengthens the composition; out of place it creates operational risk, duplicated truth or unclear ownership.
2. **The law of non-substitution.** No isotope supplants its neighbour: UDM does not become Gold, Gold does not create canonical meaning, Feature Store does not become Super Truth, the Semantic Layer does not define entity identity, the Knowledge Graph does not create a parallel truth, Governance does not arbitrate business meaning, and WAP does not replace Quarantine.
3. **The law of cross-cutting application.** Certain isotopes operate not within a single bowl but across several bowls and planes: Saga, WAP, Kappa, CDC / Event Sourcing, Governance, Zero-Copy, the Semantic Layer, the Knowledge Graph and Feature Store. Their form may change according to address, but the invariant must remain stable: statefulness, publish discipline, replay equivalence, policy enforcement, non-duplication of truth, approved metric serving, graph-discoverability or AI-ready representation.
4. **The law of stable core / adapted surface.** Data Fountain works not with dogmatic academic patterns but with their isotopes: the stable core is preserved, unnecessary academicism is trimmed away, and the surface is adapted to the role within the fountain. Saga remains a discipline of the long process but need not become a workflow engine; DV2.0 lite remains a discipline of identity / history but not a full Raw Vault; WAP remains a publish discipline but not a total manual customs post.
5. **The law of compositional compatibility.** Isotopes must not merely coexist but be compatible within a single flow: Kappa accelerates a single change-aware path, WAP controls the right to publish, Plumbum fail-closed isolates unsafe material, Saga provides statefulness and compensation, UDM / URDP provide semantic coordinates, Gold packages approved meaning, Platinum materialises AI / ML-serving, and Super Truth arbitrates final interpretation.
6. **The law of control-state visibility.** The decision of every isotope must leave a machine-readable trace: contract version, mapping version, rule version, feature definition, metric definition, graph relationship, publish decision, quarantine reason, governance policy, lineage or Super Truth decision. If a discipline exists only as an oral agreement or local SQL / notebook / prompt, it is not a canonical part of Data Fountain.
7. **The law of cognitive integrity.** An isotope must reduce rather than increase cognitive load. If, after the addition of a pattern, the team understands less clearly where truth is born, who the owner is, which contract is in force and how an artefact moves, that is not “rich architecture” but a signal of faulty composition.
8. **The law of residual risk.** If no isotope can honestly transform material into trusted or approved truth — on account of poor source data, semantic conflict, policy block, missing ownership or the impossibility of replay — the correct behaviour is not a shortcut but a halt, quarantine, escalation or direct recording: the data are not fountain-grade.

### Final hierarchical formula

**Data Fountain is a Medallion-compatible architecture of controlled data maturation in which raw evidence passes through quarantine, readiness, semantic contracts, advanced AI / ML-ready serving, consumer-ready product packaging, governed publication and final semantic arbitration; every child pattern is employed as an isotope with a clear address, responsibility and boundaries of application.**

# A critic’s voice: this looks rather more like a sandwich than a fountain

Quite so. We have designed and built an elegant fountain. Yet to see its perfection, one must rather turn the water on. In this abstraction, the water is **data**, and the fountain’s jets are **dataflow**. **Data Fountain** in motion is the work and interaction of different architectural planes, each performing its role from its own perspective and within its own boundary of responsibility.

## Architectural planes of Data Fountain

The planes of Data Fountain are cross-cutting surfaces that run through Bronze, Plumbum, Silver, UDM, Platinum, Gold and Semantic / Super Truth. Levels answer the question *where the data are*; planes answer *which force moves, constrains, restores or canonises them at that point*.

The canonical order of planes is: **Data Plane → Control Plane → Governance Plane → Super Truth Plane → Truth Recovery / Readiness Plane → Feature Plane**. Everything else — quarantine routing, operational façades, A2A coordination, particular services or MCP / LLM runtimes — is a way of implementing these planes, not a new conceptual plane in its own right.

| Priority | Plane | Single responsibility | Typical manifestations |
| --- | --- | --- | --- |
| 1 | **Data Plane** | Movement of payloads and artefacts through states of maturation. | Source → Bronze → Silver → UDM → Platinum / Gold |
| 2 | **Control Plane** | Machine-readable state that permits, blocks, versions and explains movement. | Contracts, manifests, schema registry, DQ verdicts, watermarks, publish decisions, lineage |
| 3 | **Governance Plane** | An evidentiary system of ownership, policy, classification, access and audit. | Policy-as-code, data product ownership, masking, retention, audit evidence |
| 4 | **Super Truth Plane** | Final arbitration of canonical meaning and decision truth. | Semantic contracts, metric authority, precedence rules, decision records |
| 5 | **Truth Recovery / Readiness Plane** | Governed return of unsafe or failed artefacts to the normal path. | Plumbum case → remediation → Bronze-compatible replay → readiness |
| 6 | **Feature Plane** | AI / ML-ready representation of approved meaning. | Features, embeddings, AI Signals, GraphRAG, semantic search, MCP / LLM context |

The logic of the order is quite straightforward: first there must be data movement; then the control that makes this movement governed; then governance, which makes it permitted and evidentiary; then Super Truth, which determines the approved meaning; then recovery / readiness, which restores the system to truth after failures; and only after that the Feature Plane, which materialises approved meaning for AI / ML and agentic consumption.

### Data Plane

**Data Plane** is the plane of the actual movement of data: raw files, CDC events, streaming records, replay artefacts, curated entities, data products and serving outputs. In Data Fountain it is not a mere “pipe” carrying a payload from one place to another; it carries data through controlled states of maturation: raw evidence → readiness → semantic contract → advanced serving / consumer product → canonical interpretation.

Its principal invariant is this: **payload must not leap straight into business truth**. Medallion sets the direction of maturation; Saga adds correlation, state and compensation; CDC / Event Sourcing provides evidence of change; Kappa keeps fresh processing and replay on a single semantic path; Plumbum stops unsafe material fail-closed; WAP separates candidate state from published truth; UDM / URDP / DV2.0 lite / SCD add identity, mappings and historical correctness; and Super Truth arbitrates meaning.

Kappa and WAP do not conflict here; rather, they balance speed and safety. Kappa prevents batch, stream and replay from splitting into different truths, whilst WAP prevents a rapidly built candidate from automatically becoming downstream truth. The Fast Path admits low-risk compatible changes; the Slow Path is engaged for new meaning, unknown mappings, governance conflict or a high blast radius.

**Data Plane formula:** the Data Plane moves the fountain’s water quickly and reproducibly, but the right to become trusted, published or canonical truth arises only after passing the control, governance, readiness, WAP and Super Truth boundaries.

### Control Plane

**Control Plane** is the plane of machine-readable control state: contracts, policies, manifests, schema versions, DQ verdicts, watermarks, readiness, publication decisions, topology / routing, lineage, observability and orchestration intent. It does not carry the payload; it defines the rules under which the Data Plane is permitted to move it.

The Data Plane moves the water; the Control Plane governs the shape of the fountain. It ought to work declaratively: a domain describes the desired state of a data product, contract or policy; the platform checks compatibility, adds guardrails and gives the Data Plane a stable set of rules. The Control Plane should not become a synchronous “central god” for every record: that would create latency, tight coupling and a single point of failure.

| Capability | Control Plane | Data Plane |
| --- | --- | --- |
| Contracts | Versions of schemas, semantic terms and compatibility rules | Executes transformations under the active contract |
| Publication | WAP gates, Fast / Slow Path and publish decision | Creates the candidate and switches the published state |
| Lineage | Defines mandatory lineage and correlation state | Supplies runtime evidence, logs, metrics and traces |
| Recovery | Records quarantine cases, replay windows and readiness | Performs rerun, replay and materialisation |
| Resilience | Fail-closed behaviour for new changes and last-known-good rules | Continues operating on an approved snapshot |

The principal invariant is this: **downstream must not read the “latest artefact” merely because it physically exists**. An artefact becomes consumable only when control state exists: the manifest is complete, the schema is compatible, the DQ verdict is known, lineage is sufficient, policy is classified, readiness has been formed and the publish decision permits use.

**Control Plane formula:** the Control Plane makes the movement of data governable: the Data Plane executes, whilst the Control Plane permits, stops, versions, explains and makes that movement replay-safe.

### Governance Plane

**Governance Plane** is the plane of trust, accountability and permitted consumption. It does not move the payload, hold desired state or arbitrate meaning. Its task is to prove that an approved artefact has an owner, classification, sensitivity, lineage, policy profile, access boundary, audit trail and lifecycle.

Governance in Data Fountain is not a manual access committee but **continuous assurance**: policy coverage, classification gaps, stale access, over-broad grants, missing owners, lineage breaks, expired exceptions, quality drift and policy drift must be visible continuously. For AI / ML this means that an agent, model or LLM inherits the policy boundaries of the artefacts it uses: sensitivity, purpose limitation, row / column restrictions, allowed context, audit and the explanation boundary.

The boundaries are simple: the Control Plane expresses rules as machine-readable state; the Governance Plane proves that those rules have been applied to specific assets and consumers. Super Truth determines canonical meaning; Governance ensures that the decision has an owner, audit trail, access control and policy propagation. The Data Plane creates artefacts; Governance determines whether they may become broadly consumable.

| Level | Governance focus | Prevents |
| --- | --- | --- |
| Bronze | Source ownership, raw classification, retention | Raw data without an owner or sensitivity classification |
| Plumbum | Quarantine reason, severity, repair authority | Deletion or patching without evidence |
| Silver | Readiness evidence, DQ ownership, lineage | Trusted tables without quality responsibility |
| UDM | Canonical term ownership, mapping responsibility | Shadow definitions in SQL / BI |
| Platinum | Feature / embedding governance, model-use permissions | AI features as a parallel truth |
| Gold | Data product ownership, SLA / SLO, certification | Dashboards without product accountability |
| Super Truth | Decision-record governance, conflict audit | Canonical decisions without a version or effective period |

**Governance Plane formula:** the Governance Plane turns ownership, classification, policies, access, masking, lineage and audit into an evidentiary system for the safe consumption of approved truth — without duplicating meaning and without creating a governance bottleneck.

### Super Truth Plane

**Super Truth Plane** is the plane of canonical meaning and final decision truth. Data Fountain contains many local truths: Bronze evidence, Silver prepared truth, UDM contract truth, URDP reference truth, Gold product truth, Feature Store signal truth, BI consumption truth and the LLM explanation surface. Super Truth does not abolish them; rather, it determines which interpretation has precedence in a given business context.

It is a decision boundary, not yet another curated table above Gold. It is needed where different bounded contexts, mappings, metrics, features or explanations may be technically valid yet semantically incompatible. Gold may publish a KPI, the Feature Plane may produce a score, and an LLM may provide an explanation; nevertheless, a production decision ought not to choose the most convenient version of truth for itself.

The boundaries are straightforward: the Data Plane builds candidates; the Control Plane determines whether they are permitted; the Governance Plane proves the policy envelope; the Super Truth Plane determines what those candidates mean. If repaired data return from Plumbum through replay, Super Truth decides whether this changes the approved metric or merely adds correction context.

**Super Truth Plane formula:** Super Truth answers a single question: which interpretation is approved for the business decision at this very moment, with these versions, this lineage, this effective period, this evidence bundle and this confidence.

### Truth Recovery / Readiness Plane

**Truth Recovery / Readiness Plane** is the plane of governed return to truth after a failure, quarantine event, failed candidate or readiness break. It does not “fix the data by hand”; rather, it defines how an unsafe or not-yet-ready artefact may legally return to the normal path without a direct downstream patch.

Plumbum is not a separate architectural plane here, but an implementation pattern for the fail-closed quarantine boundary. The minimum recovery contract is: reason, severity, failed rule, owner, evidence bundle, repair options, administrator decision, replay eligibility and the permitted path back through Bronze-compatible replay or an official correction event.

1. **Detection.** A gate has not been passed: the manifest is incomplete, the schema is incompatible, DQ has failed, policy has blocked the artefact, a semantic conflict has appeared, a candidate has failed or a feature is broken.
2. **Case.** A recovery case is created with the reason, failed rule, evidence, owner, correlation identifiers and decision history.
3. **Decision.** The owner, steward or governance authority chooses rejection, source repair, a correction event, mapping update, contract update, replay or escalation.
4. **Replay.** Return occurs through raw evidence, a correction event or approved replay input — not through a direct Silver, Gold or Feature patch.
5. **Validation.** The artefact once again passes through schema, DQ, policy, lineage, readiness and publication gates.
6. **Handoff.** Downstream receives the artefact only after a readiness record exists.

Readiness is not the same as physical existence. `READY_FOR_REPLAY` does not mean `READY_FOR_SILVER`; `READY_FOR_REBUILD` does not mean `READY_FOR_SERVING`; and `REMEDIATED` does not mean `PUBLISHED`.

**Truth Recovery / Readiness Plane formula:** Plumbum case → approved remediation intent → Bronze-compatible replay → normal validation / readiness path → governed handoff.

### Feature Plane

**Feature Plane** is the plane of AI accessibility and AI operability: reusable ML features, embeddings, AI Signals, semantic search, GraphRAG / Hybrid RAG, LLM / MCP / agent-ready context, point-in-time correctness and training-serving parity. It sits after approved semantics because AI / ML representations must not become a parallel truth.

The Feature Plane takes approved meaning from UDM / URDP / Super Truth and materialises it for models, agents and advanced serving. A feature is not a column; an embedding is not semantics; a vector index is not truth; and LLM context is not a business rule. All of them must be derivatives of canonical terms, contracts, lineage, policy profile and effective versions.

The principal invariants are: training-serving consistency; point-in-time correctness; no feature logic without owner, version and lineage; embeddings index meaning rather than replacing it; GraphRAG requires a graph of approved relationships; an AI decision must be explainable; and refusal or escalation is valid behaviour where there is no approved source, readiness or policy permission.

**Feature Plane formula:** the Feature Plane makes approved truth fit for AI / ML, but it does not create a new truth: it materialises contract-grounded representations and returns signals to Super Truth as evidence, not as an independent arbitration of meaning.

### Boundary matrix between planes

To avoid repeating the same “boundaries with…” explanations under every plane, it is better to keep the boundaries in a single matrix.

| Boundary | Short rule |
| --- | --- |
| Data ↔ Control | The Data Plane executes movement; the Control Plane permits the next state. |
| Data ↔ Governance | The Data Plane creates artefacts; Governance determines whether they have an owner, policy and audit envelope. |
| Data ↔ Super Truth | The Data Plane builds candidates; Super Truth determines the approved interpretation. |
| Data ↔ Recovery | The Data Plane performs replay / rerun; Recovery determines eligibility and the readiness path. |
| Data ↔ Feature | The Data Plane materialises feature artefacts; the Feature Plane defines the AI / ML-serving contract. |
| Control ↔ Governance | Control expresses policy state; Governance proves enforcement on assets and consumers. |
| Control ↔ Super Truth | Control versions decision records; Super Truth determines the semantic decision. |
| Control ↔ Recovery | Control records recovery / readiness state; Recovery determines the permissible transitions. |
| Control ↔ Feature | Control holds feature definitions, versions and lineage; the Feature Plane uses them for serving. |
| Governance ↔ Super Truth | Governance provides accountability and audit for the decision; Super Truth determines meaning. |
| Governance ↔ Recovery | Governance controls policy-safe remediation; Recovery executes the path without a direct patch. |
| Governance ↔ Feature | Governance defines allowed use, sensitivity and audit; the Feature Plane provides the ML / AI representation. |
| Super Truth ↔ Recovery | Recovery restores evidence; Super Truth decides whether the approved interpretation changes. |
| Super Truth ↔ Feature | Feature signals are evidence; Super Truth decides whether they become business decision truth. |
| Recovery ↔ Feature | A feature rebuild passes through recovery / readiness gates; local patching in the Feature Store is prohibited. |

### Isotopes by plane — without repeating lists

| Isotope | Data | Control | Governance | Super Truth | Recovery | Feature |
| --- | --- | --- | --- | --- | --- | --- |
| **Saga** | Stateful movement | Correlation state | Traceability | Decision lineage | Compensation path | Feature lifecycle |
| **WAP** | Candidate → publish | Publish decision | Publication assurance | No candidate truth | Re-publication | Serving gate |
| **Kappa** | Single processing path | Replay equivalence | Replay governance | Semantic parity | Safe rebuild | Training-serving parity |
| **Plumbum** | Fail-closed stop | Quarantine case | Policy-block evidence | Negative evidence | Recovery boundary | Unsafe feature stop |
| **UDM / URDP** | Canonical-form input | Semantic versions | Term ownership | Approved meaning | Mapping remediation | Feature semantics |
| **DV2.0 lite / SCD** | Identity / history | Temporal state | Historical audit | As-of arbitration | History-safe replay | Point-in-time joins |
| **Data Product** | Consumer artefact | Product registry | Product accountability | Metric-authority link | Corrected product version | AI / ML product surface |
| **Knowledge Graph / Semantic Layer** | Semantic projections | Definitions and topology | Impact analysis | Decision graph | Conflict context | GraphRAG / metric serving |

### End-to-end scenarios

- **Bronze ingestion with a sensitive payload.** The Data Plane accepts raw evidence; the Control Plane records the manifest, schema and watermark; Governance determines classification, retention and access; Recovery blocks handoff if the manifest is incomplete; Super Truth does not interpret the raw payload as business truth; and the Feature Plane receives no context without approved semantics.
- **Gold KPI correction.** A discovered mapping error is not fixed by a direct patch in Gold. Recovery creates a case, Control opens the replay path, Data performs Bronze-compatible replay, Governance records the owner, audit trail and blast radius, WAP makes the new publish decision, and Super Truth determines the effective interpretation.
- **Feature Store rebuild.** If a feature has been built from an incorrect mapping or suffers training-serving skew, it becomes unsafe for serving. Recovery starts a rebuild from approved evidence and contract versions; Governance checks permitted use; Control records the feature definition and lineage; and Super Truth decides whether the signal affects a business decision.
- **LLM / MCP answer.** An agent does not simply take “all the data from the lake”. It receives approved context: canonical terms, policy-filtered artefacts, lineage, effective versions and permitted actions. If approved context is missing, the correct behaviour is refusal or escalation rather than improvisation.

**Final formula:** the architectural planes of Data Fountain are not six new layers and not six separate services, but six cross-cutting surfaces of responsibility. The Data Plane moves the payload; the Control Plane makes that movement executable and versioned; the Governance Plane makes it permitted and evidentiary; the Super Truth Plane determines approved meaning; the Truth Recovery / Readiness Plane restores the system to governed truth after failures; and the Feature Plane materialises approved meaning for AI / ML without creating a parallel truth.

# Pattern isotopes — complete proposed list

## Pattern isotopes of the global level

**The global level** means neither “without an address” nor “more important than the other isotopes”, but a different mode of operation: these isotopes begin working from the very first movement of data, pass through all planes and levels, and set the basic physics of the fountain. Saga immediately establishes statefulness and compensation, Medallion immediately establishes the maturation gradient, Quarantine / DLQ immediately establishes a fail-closed stop, and Feature Store immediately establishes the requirement for AI-ready representation and point-in-time discipline for all flows that may become AI / ML-facing.

### Saga Pattern — the operational discipline of governed data movement

**Saga Pattern** in Data Fountain is not a separate data layer, nor an attempt to turn the data platform into a classical distributed transaction system. Rather, it is the operational discipline of governed movement of truth between the fountain’s levels: local steps, explicit states, readiness gates, correlation context, audit, idempotency, replay and compensation.

This isotope is needed wherever the process is no longer a single atomic operation: ingestion, validation, promotion, quarantine, Silver build, UDM readiness, identity transitions, Platinum materialisation, Gold publication, canonical decision rollout, replay and backfill. Saga does not create a new truth; it prevents truth from moving implicitly, partially or in a way that is safe merely by convention.

- **Stateful flow, not “just a pipeline”.** Every run must have an intelligible state: started, validated, promoted, ready, failed, quarantined, compensated or replayed.
- **Local transaction, not global magic.** Each level commits only its own local responsibility: raw files, manifest, validation result, readiness record or published version.
- **Compensation rather than naïve rollback.** In a data platform, compensation more often means quarantine, invalidating readiness, freezing a watermark, replaying from raw evidence or issuing a compensating event — not physically “deleting everything”.
- **Idempotency.** Re-running the same step must not create duplicates, double publications or a different truth unless the input evidence or rule version has changed.
- **Correlation as a first-class concept.** Run, batch, event, readiness, quarantine, replay and publish must be bound together by a stable correlation context.

The practical criterion is plain enough: if support or governance cannot say which `run_id` it was, which `batch_id` was involved, at which step the flow stopped, what has already been committed, what may be replayed and what must be compensated, the process has not yet reached saga maturity.

### Medallion Pattern — the discipline of truth maturation

**Medallion Pattern** in Data Fountain is not the classical three-tier Bronze → Silver → Gold arrangement, nor a naming convention for folders or tables. It is the baseline discipline of truth maturation, setting the direction of travel from raw evidence towards approved, governed and consumer-ready meaning.

In Data Fountain, Medallion operates as an isotope: it preserves its core maturity gradient, but unfolds into the production-grade order **Bronze → Plumbum → Silver → UDM → Platinum → Gold → Semantic / Canonical Authority / Super Truth**. What matters here is not a second encyclopaedia of levels, but the difference from the classical pattern: Data Fountain makes the boundaries of responsibility explicit between raw evidence, quarantine, analytical readiness, semantic contract, AI / ML-ready representation, consumer-ready product and approved decision truth.

- **Additional boundaries of responsibility.** Data Fountain does not leave the move from “cleaned” to “business truth” as one rather heroic leap: Plumbum, UDM, Platinum, Gold and Super Truth separate quarantine, contract, AI-serving, product packaging and canonical arbitration.
- **Contract gates between levels.** Moving upwards is not a matter of copying a table into a new folder. Every hop must carry readiness, lineage, a policy envelope, a WAP / publication decision and the possibility of recovery through the normal path.
- **AI / ML-ready branching without shadow truth.** Platinum and the Feature Plane provide features, embeddings, AI Signals, GraphRAG and semantic search, but only as derivative representations of approved semantics, not as a parallel business truth.

The key difference from the classical form is this: in standard Medallion, the large leap from “cleaned data” to “business truth” is often left implicit. Data Fountain teases that leap apart into UDM, Platinum, Gold and Semantic / Super Truth, so that semantics are not born accidentally in dashboards, notebooks or feature pipelines.

### Quarantine / DLQ Pattern — the discipline of fail-closed isolation and governed recovery

**Quarantine / DLQ Pattern** in Data Fountain is the controlled means of not feeding poor data into the next layers, whilst not losing problematic material without trace. Any level of the fountain may produce unsafe material: an invalid batch, a failed candidate, a broken transformation, a policy conflict, a semantic mismatch or a failed feature artefact.

**Plumbum** is both the level and the canonical address of isolation. The quarantine discipline itself applies wherever data may fail a gate.

Plumbum is not a dustbin, nor does it create a parallel truth. It isolates dangerous batches, records, change events, failed candidate versions, broken transformations, unsafe data-product states or failed feature artefacts with full reason, lineage, recovery context and a route back through Bronze-compatible replay or an official correction event. In plane terms, Plumbum is the implementation pattern for the Truth Recovery / Readiness Plane: a quarantine case must become approved remediation intent, replay eligibility and a normal validation path before it is allowed into a governed hand-off.

- **Fail-closed boundary.** Silver, UDM, Platinum, Gold and Semantic do not read quarantine payload directly; they see only the normal accepted path.
- **Forensic context.** Plumbum preserves tenant, source, entity, batch and run context; schema and mapping versions; watermark; invalidation stage; error class; file, manifest and checksum; and the explanation bundle.
- **Recovery, not direct patching.** After repair, the rerun starts from Bronze or from an approved correction event, rather than from a manual fix in Silver or UDM.
- **Human-in-the-loop for semantics.** An LLM or local MVC may explain the root cause and suggest remediation options, but a non-trivial decision remains with the administrator or governance owner.
- **No poisoning of the AI / Feature Plane.** Unsafe inputs, shadow features, broken embeddings and failed materialisations have no right to enter Platinum or serving.

### Feature Store Pattern — the discipline of governed materialisation of AI-ready semantics

**Feature Store Pattern** in Data Fountain is a global isotope of AI-ready semantics: the discipline for reusing ML / AI signals, embeddings, AI Signals, GraphRAG / Hybrid RAG context and feature-oriented representations on top of approved UDM / URDP / Super Truth semantics. It is global not because it lives only in the Feature Plane or Platinum, but because its requirements — reusable definitions, lineage, point-in-time correctness, offline / online parity, policy-aware serving and no shadow feature logic — must apply across the whole system: from raw evidence and semantic contracts through to Gold products, Super Truth decisions and AI / ML-facing consumption.

Its principal value is a governed AI-accessibility surface which takes approved meaning from different points of the fountain — UDM contracts, URDP mappings, Super Truth decisions, Gold products and feature evidence — and makes it machine-discoverable, versioned, point-in-time correct, policy-governed and fit for offline training, online inference, MCP / LLM, semantic analytics and Gold data products without creating a parallel truth.

- **Feature Store does not replace UDM, Gold, the Semantic Layer or Super Truth.** Its strongest materialisation address is Platinum / Feature Plane, but its isotopic discipline runs through every plane and level where approved meaning becomes a feature, signal, embedding, AI context or model-serving artefact.
- **One definition rather than duplication.** Feature-engineering logic ought to be formalised once on top of UDM, not repeated separately in SQL for training, an inference service, AI Signals and ad hoc notebooks.
- **Offline / online consistency.** The Offline Store supports training, backtesting, replay and point-in-time joins; the Online Store provides low-latency serving for inference. Both paths must use the same feature contracts in a demonstrably compatible form.
- **Point-in-time correctness.** Historical features must see the state of entities, mappings and ownership as it stood at the relevant time, not the current version with the historical context quietly removed.
- **Governed serving.** LLM / MCP, model APIs, semantic analytics and Gold products read feature evidence through policy-aware serving contracts, rather than through arbitrary ad hoc joins into feature tables.

## Pattern isotopes of the operational mechanics level

**The operational mechanics level** defines the fountain’s discipline of motion: the engineering invariants that operate **end-to-end**, controlling how an artefact behaves on its path towards maturity, how the system proves that each transition between states is correct, repeatable and free of unintended side-effects.

### Kappa Pattern — the discipline of a single change-aware path

**In brief:** Kappa in Data Fountain is not a requirement to “stream everything”, but the discipline of a single change-aware path: primary processing, CDC, event streams, replay, backfill, rebuild and identity transitions must pass through one semantic implementation, or through a formally equivalent route.

The principal value of Kappa is not latency as such, but protection against the splitting of truth across batch, streaming, snapshot, identity-serving and AI-serving paths. Even a batch process may be Kappa-safe if it works with ordered changes and reproduces state through the same route by which it handles new changes.

- **One change-aware trajectory.** A change enters the system in the same way for the fast path and the rebuild path; replay does not become a second, rather more convenient truth.
- **One semantic implementation.** A pipeline must not have two different implementations of the same business meaning: one for “today” and another for historical recalculation.
- **Replay as a normal operating mode.** Replaying changes is part of the architecture, not an emergency exception or a manual SQL patch.
- **Ordering, idempotency and versioning.** Kappa depends on event / change identifiers, ordering, schema and mapping versions, governance profile and the ability to perform selective replay.

Kappa does not define what a change means, does not decide whether it has the right to become published truth and does not arbitrate final meaning. Its single responsibility is to ensure that a change passes through one semantic path irrespective of the processing mode.

**Practical rule:** a canonical decision change, governance-profile update, feature-definition rollout, MDM merge / split or Gold product publication ought to pass through the same processing path as the primary flow, rather than through a separate batch SQL job with different logic. In a Kappa-first model these are governed semantic transitions with version, lineage, evidence, replay scope and a publish path.

- **Must-have:** KPI, revenue, attribution, customer identity, ML features, executive reporting, governed sharing, canonical decisions and high-blast-radius semantic changes.
- **Permissible simplification:** low-impact internal datasets, temporary technical tables or exploratory outputs which do not become production truth.
- **Anti-pattern:** driving events quickly, whilst calculating the historical result through a separate batch SQL path with different logic.

The minimal envelope for a Kappa-safe change pipeline should contain the entity / aggregate key, change or event type, occurred and captured time, sequence / offset / LSN or aggregate version, event / change ID, correlation ID, schema version, mapping version, governance-profile version, sensitivity classification, canonical-authority reference where required and publish status.

### WAP Pattern — the discipline of risk-based publication

**In brief:** WAP (Write-Audit-Publish) / Data Branching in Data Fountain is not a separate fountain bowl, nor a manual customs post for every byte, but a risk-based publication discipline: a new state of a data slice, table version, semantic contract, MDM identity transition, Feature Plane artefact or Gold Data Product first exists as candidate state, passes audit and evidence checks, and only then becomes the published trusted version.

WAP answers not the question “can we write the new data?”, but “are we entitled to make these data the new trusted version?”. Write therefore does not equal Publish: a successful build does not yet mean that the artefact may be exposed to downstream consumers, dashboards, AI, APIs or Gold products.

1. **Write.** The pipeline builds an isolated candidate state: a branch, staging snapshot, candidate table version or feature / context materialisation.
2. **Audit.** The platform checks schema compatibility, data quality, reconciliation, lineage, semantic integrity, mapping impact, governance profile, canonical-authority alignment, feature parity, zero-copy consumer safety and rollback readiness.
3. **Publish.** Only an approved candidate becomes the official published state through a stable view, table alias, governed endpoint or approved version pointer.

Any candidate, whatever its level or address — Silver readiness, a UDM contract change, feature materialisation or a Gold product — becomes published truth only through a WAP gate.

**Fast Path / Slow Path.** If the change envelope and evidence bundle contain a known contract version, compatible schema, known mapping, stable governance profile, complete lineage, data quality within threshold and low blast radius, publication may be automatic or semi-automated. If there is new semantics, an unknown mapping, breaking schema, KPI delta, governance conflict, high-blast-radius zero-copy publication, Feature Plane drift or canonical ambiguity, the Slow Path is required: freeze the candidate, review, align with Super Truth, rework or perform selective replay.

- **Plumbum versus WAP:** Plumbum blocks unsafe material before it becomes a candidate; WAP freezes a candidate that has been built but is not yet entitled to become published truth.
- **Published pointer:** publication should move an approved version pointer rather than destructively overwrite a production table.
- **No Candidate Leakage:** business consumers must not read candidate state directly.
- **Anti-pattern:** after a build, the pipeline immediately performs a MERGE into the production table. That is “write equals publish”, not WAP.

The minimum publish metadata contract should record the entity name, asset priority / type, candidate version, previous / published version, audit status, publish decision, gate mode, run ID, schema / mapping version, evidence-bundle completeness, owner / approval evidence, canonical-authority reference, governance profile, policy tags, rollback plan and impact summary.

### Zero-Copy Pattern — the discipline of a single governed asset without duplication

**In brief:** Zero-Copy Data Sharing in Data Fountain is the discipline of not duplicating truth: teams, services, Gold Data Products, the Platinum / Feature Plane, the Semantic Layer, MCP / LLM consumers and external consumers work with the same governed asset without physical file duplication, private ETL copies “on the side”, or parallel versions of truth.

Zero-copy does not mean “give everyone access to the bucket”. It means governed access to an approved asset through a table representation, a contract, an owner, a readiness signal, lineage, an IAM / policy boundary and an audit trail. Strip away the contract and governance, and what remains is not zero-copy but uncontrolled raw exposure.

The practical implementation of zero-copy rests on three architectural components: **physical storage backbone** — object storage as the single physical home of the files; **table format / metadata layer** — an open table format providing snapshots, schema, versioning and ACID-like behaviour over those files; and **SQL access layer** — an engine that reads the table format directly without copying the data into its own store. On top of this sit **catalogue / governance visibility** — a single registry of assets with classification, lineage and policy; **IAM / identity boundary** — a security perimeter at service-identity level rather than shared credentials; and **orchestration / control plane** — validation, readiness gates and the publication path.

- **One physical asset.** Data are not copied into each team’s private buckets or datasets without an explicit architectural reason.
- **Table contract rather than random files.** The consumer reads a stable table representation with schema, snapshot, manifest, watermark and readiness metadata.
- **Governed projection.** Access is constrained by tenant / entity scope, masking, row / column restrictions, permitted consumers and audit expectations.
- **Canonical reference.** If the asset affects KPI, attribution, revenue, AI decision support or feature generation, the contract must reference UDM / Data Product / Super Truth semantics.

As an isotope, zero-copy supports the non-duplication of truth across the whole chain: Bronze evidence, UDM contracts, Platinum feature evidence, Gold Data Products and Super Truth decision references.

**When a copy is nevertheless permissible:** a separate materialisation may be justified for a legal boundary, immutable publication snapshot, performance isolation, latency-sensitive serving or a product-specific optimised model. Even then, the copy must have an owner, retention, lineage, source snapshot, policy profile and a reason why the zero-copy contract is not sufficient.

- **Advantages:** less storage duplication, less semantic drift, better freshness, cleaner governed sharing and stronger replay / reproducibility.
- **Trade-off:** fewer physical copies mean more responsibility in metadata, contract discipline, governance and performance / support ownership.
- **Anti-patterns:** direct bucket access, local remap copies, “temporary” copies without lifecycle, latest-only reading without snapshot context, and AI / LLM direct table access outside a governed projection.

**Practical rule:** if a team can read a governed table contract without copying files into its own storage, that is zero-copy. If “sharing” creates another physical copy without an owner, lifecycle and lineage, it is a materialisation — however neatly automated it may be.

### Circuit Breaker Pattern — the discipline of protection against cascading failures

**In brief:** Circuit Breaker in Data Fountain is the operational discipline of fail-fast protection: when a downstream level, source API, processing segment, serving endpoint or control dependency becomes unhealthy, the system temporarily stops new attempts to move through that segment rather than accumulating timeouts, retries, cost spikes and cascading failures.

This isotope is needed wherever the failure of one component may poison or block the whole flow: Bronze ingestion, Silver build, UDM contract validation, Platinum materialisation, Gold publication, Feature Store serving, an external SaaS API, schema registry, DQ engine, catalogue or policy service. Circuit Breaker does not create truth and does not repair data; it protects the normal path from an unhealthy dependency and allows the system to degrade in a controlled fashion.

- **Closed state.** The segment is healthy: requests, batches, events or publish attempts move through the normal readiness / WAP / governance path.
- **Open state.** Once the failure threshold is exceeded, the system fails fast, blocks new attempts towards the unhealthy downstream dependency, preserves control-state evidence and prevents a retry storm from damaging neighbouring levels.
- **Half-open state.** The system allows a limited number of test attempts or health probes in order to verify dependency recovery without bringing the full load back at once.
- **Fallback / last-known-good.** Where the use case permits it, consumers may temporarily read an approved published snapshot, cached semantic contract or last-known-good feature definition — but not candidate state and not a partial rebuild.

In Data Fountain, Circuit Breaker must be tied to the Control Plane: breaker state, reason, failure class, threshold, opened\_at, affected segment, owner, retry window, half-open policy and recovery condition must all be machine-readable. Otherwise it is not an architectural discipline, but merely a local timeout buried in code.

**Boundaries with neighbouring isotopes:** Plumbum isolates unsafe data or a failed artefact; Circuit Breaker isolates an unhealthy operational segment. Saga compensates or recovers a long process; Circuit Breaker may prevent a new step from starting whilst the downstream dependency is unhealthy. WAP decides whether a candidate is entitled to become published truth; Circuit Breaker decides whether the publication path is operationally healthy enough to be attempted at all.

- **Must-have:** external APIs, CDC connectors, schema registry, DQ / validation services, Silver build clusters, UDM / URDP validation, Feature Store online serving, Gold publication endpoints and high-blast-radius semantic changes.
- **Permissible simplification:** a low-impact offline backfill or exploratory pipeline where failure does not create a cascade and has bounded cost.
- **Anti-pattern:** infinite retries without timeout, jitter, breaker state or an owner; the pipeline “waits a little longer” whilst the downstream dependency is already dead and the upstream continues to flood the queue.

**Practical rule:** if the downstream dependency is unhealthy, Data Fountain should fail fast, freeze the candidate, preserve the recovery context and continue on an approved snapshot, rather than quietly creating partial state, a timeout chain or a shadow workaround.

### Backpressure / Flow Control Pattern — the discipline of flow-rate self-regulation

**In brief:** Backpressure in Data Fountain is the operational discipline of controlling the rate at which data move: a producer, ingestion job, stream, replay or materialisation must not push more payload than the downstream level, gate, queue, storage, compute layer or serving boundary can safely accept.

This isotope answers not “are the data valid?”, but “can the system process this flow now without loss, uncontrolled lag, memory explosion, a small-file storm, a cost spike or the degradation of other tenants?”. Backpressure operates before failure: it slows the flow under overload, whereas Circuit Breaker stops or fails fast when a dependency is unhealthy.

- **Pull-based consumption.** Downstream must be entitled to read only as much as it can process, rather than being passively flooded by the producer.
- **Bounded buffering.** Queues, staging zones, micro-batches and replay windows must have explicit limits; an unbounded queue is a hidden incident, not a scaling strategy.
- **Rate limiting / throttling.** Source connectors, CDC streams, tenant bursts, backfills and rebuilds must obey throughput, concurrency, cost and downstream-readiness limits.
- **Dynamic batching and load shaping.** Under load, the system may adjust batch size, parallelism, compaction cadence, replay chunking or scheduling in order to preserve a controlled flow.
- **Fairness between flows.** One tenant, source, backfill or feature rebuild must not consume the entire compute, I/O or serving budget and break the SLA of other consumers.

In Data Fountain, Backpressure must be visible in the Control Plane: lag, queue depth, watermark delay, processing rate, accepted rate, rejected / throttled rate, tenant quota, replay pressure, compaction pressure, cost signal and serving saturation must all form part of operational state. Without these signals, the system is not controlling the flow; it is merely reacting to the consequences.

**Boundaries with neighbouring isotopes:** Kappa guarantees one semantic path for fresh processing and replay; Backpressure ensures that this path does not drown under its own load. WAP controls whether a candidate has the right to become published truth; Backpressure controls the rate at which candidates ought to be built in the first place. Circuit Breaker stops the flow under failure or an unhealthy dependency; Backpressure slows the flow under overload before failure occurs.

- **Must-have:** CDC ingestion → Bronze, Bronze → Silver builds, the streaming Kappa path, replay / backfill, Feature Store materialisation, embedding rebuilds, Gold product refresh, multi-tenant ingestion and external API polling.
- **Permissible simplification:** a small bounded batch with predictable input, where downstream capacity is stable and failure does not create a backlog cascade.
- **Anti-pattern:** the producer writes without limit, the queue grows without bounds, autoscaling merely increases cost, and the downstream system begins to fail or publish stale / partial state.

**Practical rule:** if downstream cannot keep up, Data Fountain should slow the producer, limit replay, reduce concurrency, defer non-critical rebuilds or degrade freshness — but it must not lose data, break readiness or create uncontrolled copies or partial truth.

## Pattern isotopes of the data-contract level

**The data-contract level** defines the fountain’s contract discipline. It addresses the central question of semantic architecture: **what these data mean to the business, how those meanings are fixed, who is accountable for them, and how they change over time without losing historical truth**.

### UDM Pattern — contract discipline as the foundation of unification

**The UDM isotope** in Data Fountain is not “one more layer of tables” after Silver, but a semantic forge and contractual boundary where cleansed material receives its canonical business form. Its principal address is the data-contract level: stable identity, grain, canonical facts, dimensions, relationships, allowed states, policy boundary and usage rules which can then be safely consumed by Platinum, Gold, Semantic / Canonical Authority and AI scenarios.

In this isotope, the order is deliberately fixed: **term → contract → rules → representation**. The business term is first stated in the language of the domain; then comes its contract, with grain, keys, owner, lineage, effective period and quality expectations; then the mapping, inference, precedence and replay rules; and only after that do SQL views, tables, graph projections, Feature Store definitions, embeddings, BI models, API payloads or AI-context appear.

What remains specifically within the UDM isotope:

- **Canonical semantic boundary.** UDM answers the question “what do these data mean to the business in the same way for everyone?”, not “what arrived from the source?” and not “which KPI should be shown to the consumer?”.
- **Contract-first modelling.** Facts, dimensions, relationships and serving views ought to exist as consequences of the contract, rather than as convenient SQL artefacts.
- **Naming by artefact role.** Preferred naming for new structures is `udm_fact_*`, `udm_dim_*` and `udm_link_*`; `udm_bridge_*` is reserved for justified allocation, grain-shift or query-assist exceptions. The flatter `udm_*` namespace remains an umbrella for contract-level assets, registries, service representations or compatibility views.
- **Serving boundary.** Downstream consumers must not create parallel semantics in Gold, BI, APIs, the Feature Store or prompts; they ought to consume approved UDM / URDP / Super Truth meanings.

**Anti-pattern:** UDM as Silver++ or early Gold: a set of curated tables in which business meaning lives in SQL, dashboard expressions or prompt logic. The proper UDM isotope does not sell a consumer-ready product and does not arbitrate the final KPI; it creates stable contract truth upon which those higher levels can safely be built.

### URDP Pattern — the discipline of reference-semantics normalisation

**The URDP isotope** in Data Fountain is the semantic control plane for reference data, master mappings and interpretation rules. It is not a separate bowl between Silver and UDM, nor a local lookup table tucked away for convenience. Its role is to extend the UDM contract through governed dictionaries, mappings, effective-dated rules, ownership, validation, quarantine behaviour and lineage down to a specific mapping version.

URDP answers the question: **“by which rule does a raw or source-specific value become canonical business meaning, and who is accountable for that rule?”** This is where source and channel normalisation, CRM status to funnel mapping, manager identity aliases, revenue states, eligibility flags, classification taxonomies, allowed values and profile-aware reference behaviour ought to live.

What remains specifically within the URDP isotope:

- **Reference dictionaries.** Governed sets of allowed canonical values: source, channel, funnel stage, currency, role, profile, classification and KPI eligibility reason.
- **Master mappings.** Versioned rules for translating source-specific values into canonical UDM semantics, with owner, approval, effective period, fallback and quarantine behaviour.
- **Effective-dated semantics.** A rule must answer not only “what is correct now?”, but also “which meaning was correct at the date of the event, replay or historical KPI?”.
- **Runtime dependency for UDM / Gold / API / AI.** URDP assets ought to be executable platform assets, not documentation sitting politely beside the code.

**Practical rule:** if a rule answers “which canonical values are allowed?”, it belongs in `udm_ref_*`. If it answers “how does a source-specific value become canonical meaning?”, it belongs in `udm_map_*`. If the reference or mapping asset itself is historicised, use `udm_ref_sat_*` or `udm_map_sat_*`.

**Anti-pattern:** a small lookup in dashboard SQL, a BI calculated field, CSV file, prompt or frontend code which quietly becomes a second truth. URDP removes semantic drift precisely by making mappings governed, versioned, auditable and replay-safe.

**Relationship with ACL:** URDP defines mapping rules and reference semantics, whilst the Anti-Corruption Layer provides the architectural boundary that prevents source-specific values from bypassing those rules through ad hoc SQL, dashboard expressions, notebook logic, Feature Store transformations or prompt context.

### DV2.0 lite Pattern — the discipline of stable identity and history

**The DV2.0 lite isotope** in Data Fountain is not a full Raw Vault / Business Vault, nor a new methodological religion placed on top of Medallion. It is a deliberately limited modelling discipline for UDM / URDP contracts: stable canonical keys, explicit relationship objects, satellite-like history, lineage and replay-safe identity wherever these genuinely strengthen downstream consistency.

DV2.0 lite answers the question: **“which stable identity, relationship and historical evidence exists?”** It does not decide final business meaning, and it does not replace URDP mappings or Canonical Authority decisions. Its strength lies not in a separate vault layer, but in the identity and history discipline embedded within Data Fountain itself.

What remains specifically within the DV2.0 lite isotope:

- **Hub-like stable keys.** Canonical keys for reusable business entities: lead, deal, campaign, manager, source, client and project. Where a separate structure is genuinely required, use naming such as `udm_hub_*`.
- **Link-like relationships.** The preferred default for relationship truth is `udm_link_*`: attribution, ownership, hierarchy, lead-to-deal and manager assignment. `udm_bridge_*` is allowed only as a justified exception for allocation, grain shift or query-assist packaging.
- **Satellite-like history.** Business-critical changes are historicised: status meaning, revenue meaning, manager / team history, mapping history, eligibility and access-relevant semantics. Naming follows `udm_sat_*`, `udm_ref_sat_*` and `udm_map_sat_*`.
- **Replay-safe identity.** A backfill, CDC rebuild or replay must not create new keys for the same business entity without an explicit merge / split or authority rule.

**Practical boundary of the lite version:** take a DV2.0 element only when it opens consumer reuse, removes semantic drift or supports the governance / authority path. If the new object is needed only for a specific chart, dashboard or presentation use case, that is Gold logic, not DV2.0 lite.

### SCD Pattern — the discipline of historical correctness for Gold dimensions

**The SCD isotope** in Data Fountain is a consumer-facing temporal serving projection for Gold / Data Products. It is not raw history, it is not Super Truth, and it does not resolve mappings on its own. Its task is to prevent current-state joins from quietly rewriting the past when a consumer looks at a manager, source / channel, funnel stage, taxonomy, profile policy or other dimensions in a historical context.

SCD answers the question: **“what did this dimension mean for the business decision at that point in time?”** It consumes approved meaning from UDM / URDP / DV2.0 lite / Governance / Super Truth and publishes it in a stable historical consumption form for BI, APIs, AI, Platinum / Feature Store, marts and Data Products.

What remains specifically within the SCD isotope:

- **Type 2 for business-critical Gold dimensions.** Manager / team, source / channel, funnel / status class, product taxonomy, profile / policy and customer tier where a change affects KPI, ownership, attribution, eligibility, access or AI explanation.
- **Type 1 for non-critical descriptive attributes.** Cosmetic labels, spelling fixes and display names may be overwritten if they do not change business meaning.
- **As-of contract.** The consumer must know which date anchors the join between fact and dimension: event date, reporting date, publication date or another explicitly stated temporal anchor.
- **Temporal quality gates.** Overlaps, gaps, duplicate current rows, invalid mapping versions and policy conflicts must not pass into published Gold SCD.

**Anti-pattern:** “UDM already has history, so Gold will work it out.” A Gold consumer should not have to reconstruct temporal meaning from lower layers. SCD is the last mile of historical correctness, where a complex contract discipline becomes straightforward enough to consume.

### CDC / Event Sourcing Pattern — the discipline of the birth and propagation of change

**The CDC / Event Sourcing isotope** in Data Fountain is the change-aware discipline for how a change is born, enters the system, passes control and can be reproduced. CDC answers the question **“what changed in the state?”**, whilst Event Sourcing answers **“which business event caused this transition?”**. Neither approach is a separate layer; both strengthen Bronze, Silver, UDM, URDP, DV2.0 lite, SCD, WAP, Kappa, Gold and Super Truth.

CDC is strong where inserts / updates / deletes from an operational or SaaS source must be delivered quickly and cheaply without a full reread. Event Sourcing is strong where the event itself is evidence of a business transition: causality, auditability, replay, compensation, merge / split history, revenue transitions, owner reassignment or policy changes.

What remains specifically within the CDC / Event Sourcing isotope:

- **Change contract.** The contract describes not only the shape of the object, but also the shape of the change: `event_id`, `event_type`, `change_type`, `aggregate_id`, `aggregate_version`, `occurred_at`, `captured_at`, ordering metadata, idempotency key, schema / event version and lineage.
- **Ordering and idempotency.** Retry, replay and late-arriving changes must not create duplicates, incorrect ordering or a silent overwrite of historical truth.
- **Semantic change layer.** If the source provides only CDC before / after state, Silver standardises the change input, whilst UDM / URDP raise it to a semantic transition without pretending that it is already a fully fledged domain event.
- **Replay-ready evidence bundle.** A change ought to carry enough context for WAP, governance, temporal correctness and the Super Truth gate: mapping version, contract version, governance profile, correlation / causation IDs, effective period and authority reference where required.

**Anti-pattern:** calling every update an “event” without domain meaning, causality, aggregate version or replay rules. That is merely event-shaped CDC. The proper isotope does not supplant UDM, URDP or SCD; it gives them ordered, auditable and replay-safe change material.

## Pattern isotopes of the data-product level

**The data-product level** defines the fountain’s product discipline. It addresses the central question of consumption: **how approved semantics are turned into governed, versioned, consumer-first data products with ownership, contract, SLA / SLO, governance profile, delivery neutrality and intelligible business value**.

### Data Product Pattern — the discipline of the governed Gold product shelf

**Meaning of the isotope:** Data Product Pattern for Gold defines how approved semantics are turned into consumer-first data products with owner, contract, grain, SLA / SLO, lineage, governance profile, publish state and delivery forms. Its role is not to create a new truth, but to package UDM / URDP / Super Truth-approved meaning and Platinum evidence into a form that can be safely discovered, consumed, versioned and republished.

**What this isotope adds specifically:** Gold ceases to be a collection of “reporting tables” and becomes a governed product shelf. Each product requires consumer context, business value, ownership, contract, policy envelope, lineage, allowed serving forms and a recovery path. If an artefact has the right name but lacks these properties, it is not yet a production-grade Gold product.

- **Key boundary:** a Gold product packages approved semantics, but does not invent KPI, identity, attribution or mapping in a dashboard, notebook, prompt or local SQL.
- **Product registry:** `gold_product_registry` should be the control point for product\_id, type, owner, lifecycle, contract, authority reference, governance, lineage, serving and composition rules.
- **Naming:** the baseline rule is `gold_<type>_*`, where type describes the catalogue role: `ur`, `mdp`, `mdm`, `dp`, `slice`, `pkg`, `api`, `cmp`, `pack`, `svc`, `sig`, `sem`, `dec`.
- **Packaging without duplication:** a new dashboard, alert, API, tenant configuration or report is usually a slice / package / serving change rather than a new base product. A new base product is needed only when a new reusable semantic meaning appears.
- **WAP and recovery:** a published Gold product is not repaired by a silent table rewrite. Correction goes through a recovery case, replay or approved correction event, the normal validation path and WAP re-publication.

**Anti-patterns:** calling any table a data product; allowing Gold to create business meaning on its own; directly patching a published product; turning a custom package into a new truth; or giving downstream BI, API or AI consumers direct access to base Gold tables without governed serving.

**Relationship to neighbouring isotopes:** the Data Product Pattern is the wider frame for the whole Gold shelf. Universal Resource / Master Data Product is a specialised type within that frame, whilst MDM is a specialised master-serving capability surface. The general taxonomy, lifecycle, registry, packaging and consumption discipline live in the Data Product Pattern; concrete master/resource contracts and identity rules live in the corresponding specialised isotopes.

### Universal Resource / Master Data Product Pattern — the consumer-first pattern for Gold

**Meaning of the isotope:** the Universal Resource / Master Data Product Pattern describes the baseline consumer-facing category of Gold products for reusable master and reference semantics. It is not a reference table for its own sake, nor “just another dimension table”, but a stable semantic asset with a contract, grain, governance profile, history, delivery neutrality and intelligible business value in its own right.

**What this isotope adds specifically:** it answers which Universal Resources in Gold should become the first supporting products: customer identity, lead intelligence, funnel state, marketing attribution, revenue outcome, consent profile, account health, AI signal base and so forth. A single L0 Universal Resource may have many delivery forms — views, APIs, reports, AI context, Feature Store inputs and packages — but it must not give rise to many local truths.

- **Consumer-first boundary:** the product shape is determined not by the technical convenience of a table, but by the decision the consumer is able to make because of the product.
- **Minimum product contract:** consumer, decision context, grain, canonical key, meaning, owner, freshness, limitations, lineage, governance profile, delivery forms and authority reference.
- **Golden Customer Record as an example:** `gold_ur_golden_customer_record` is the reusable evidence core for marketing, sales, support, billing, LTV, attribution and AI personalisation; detailed matching, survivorship, crosswalk and merge / split rules belong to the MDM capability surface.
- **Zero-copy default:** downstream teams do not copy the base Universal Resource into local datasets for their own merge, masking or attribution rules. Consumption should go through governed projections, authorised views, APIs, MCP resources, slices or packages.
- **Governance profile:** sensitivity classification, allowed consumer roles, masking, row / column restrictions, policy tags and audit expectations are part of the product contract, not a manual setting after publication.

**Anti-patterns:** calling a lookup table or conformed dimension a master product without consumer value; starting from a technical table schema and then looking for a consumer afterwards; creating a separate master product for every dashboard; or allowing BI SQL, frontend code or prompt logic to define master / reference semantics.

**Relationship to neighbouring isotopes:** UDM defines what the entity means; URDP governs mappings and reference rules; MDM describes the governed identity, state and hierarchy capability; the Data Product Pattern defines how all of this is published and catalogued; Universal Resource / MDP packages approved master / reference semantics as reusable consumer-facing Gold products.

### MDM Pattern — the discipline of governed identity and master serving

**Meaning of the isotope:** MDM (Master Data Management) Pattern in Data Fountain is a Gold-facing isotope for productised master truth. It describes not a separate legacy MDM hub, but a governed intersection pattern for reusable business entities: customer, account, product, channel, supplier, location, consent, hierarchy, taxonomy and other company-wide entities.

**What this isotope adds specifically:** MDM details the `gold_mdm_*` capability surface: identity resolution, source crosswalk, survivorship, merge / split policy, hierarchy, stewardship, confidence, temporal validity, governance profile, recovery and controlled serving. If Universal Resource presents the consumer-facing resource, MDM explains and operates the master-state mechanics on which that resource rests.

- **Core contract:** canonical entity key, source IDs, identity crosswalk, matching logic, confidence thresholds, attribute-level survivorship, merge / split policy, hierarchy rules, temporal semantics, governance profile, authority reference and serving forms.
- **Local truth, not final truth.** MDM provides local product truth and the evidence core for master state, but conflict-prone meanings — KPI, attribution, consent interpretation and executive decisions — are escalated to Canonical Authority / Super Truth.
- **History-aware serving.** Current state without as-of semantics is dangerous for historical KPIs, audits and retrospective decisions. MDM ought to support effective-dated identity, mappings and history.
- **Recovery discipline.** Bad merges, split corrections, wrong crosswalks, rejected master snapshots and consent regressions are not patched manually in Gold; they move through the Plumbum / Truth Recovery / WAP path.
- **AI boundary.** MCP / LLM may propose or explain candidates, but it must not have direct publish rights for identity resolution, merge / split decisions, mappings or governance exceptions without contracts, WAP gates, audit and human approval for critical cases.

**Anti-patterns:** a golden record as a single flat table without evidence; Customer 360 without explicit identity rules; manual merge / split in production without audit and a replay path; direct access to base master tables; or BI, prompt or API logic defining survivorship outside the contract registry.

**Relationship to neighbouring isotopes:** the Data Product Pattern gives `gold_mdm_*` its place in the Gold catalogue; Universal Resource / MDP provides the consumer-facing product contract, for example `gold_ur_golden_customer_record`; MDM provides the governed capability surface, for example `gold_mdm_customer_identity_management`, which supports crosswalk, confidence, merge / split, stewardship and publish-safe master state.

### Metric Product Pattern — the discipline of governed KPI / metric products

**Meaning of the isotope:** the Metric Product Pattern in Data Fountain defines how a KPI, business metric, scorecard number or analytical measure becomes not a dashboard calculation, but a governed Gold data product with a formula contract, grain, allowed dimensions, owner, version, freshness, lineage, authority reference and intelligible consumer value.

**What this isotope adds specifically:** Headless BI / Semantic Layer may serve approved metrics in a tool-neutral manner, yet the metric product itself requires its own product contract: numerator / denominator, aggregation logic, time window, attribution window, null policy, currency / unit policy, breakdown rules, comparison baseline, historical behaviour and an escalation path to Super Truth for official KPIs.

- **Formula contract.** A metric must be described as a business definition plus an executable rule, not as a hidden SQL fragment, BI calculated field or prompt instruction.
- **Grain and allowed dimensions.** The metric product fixes the grain at which it exists, which breakdowns are permitted, which joins are legal and which slicing combinations create semantic risk.
- **Versioning without breaking history.** A change to the formula, attribution window or eligibility rule must create a new metric version with an effective period, rather than quietly rewriting historical comparisons.
- **Authority escalation.** For executive KPIs, revenue, margin, churn, conversion, customer-facing numbers and regulated reporting, the metric product must refer to a Super Truth decision or carry an explicit escalation contract.

**Boundaries with neighbouring isotopes:** UDM defines canonical terms and entities; URDP governs mappings; SCD provides as-of correctness; Headless BI / Semantic Layer serves the metric API; the Data Product Pattern maintains the registry and lifecycle; **Metric Product** defines how a particular KPI becomes a governed, versioned and reusable Gold product.

**Anti-pattern:** “Revenue”, “conversion”, “active customer” or “profit” living as different formulae in dashboards, notebooks, semantic-layer workbooks or LLM prompts. That is not a metric product, but a shadow KPI.

### Data Product Lifecycle Pattern — the discipline of versioning, deprecation and sunset

**Meaning of the isotope:** the Data Product Lifecycle Pattern defines the full lifecycle of a Gold product: draft → candidate → published → certified → deprecated → sunset → archived. Without this, the Gold registry eventually turns into a graveyard of old tables, dashboards, APIs and packages, where it is no longer clear what is still production truth and what has become a historical artefact.

**What this isotope adds specifically:** the Data Product Pattern says that a product must have an owner, contract and registry. The Lifecycle Pattern adds the discipline of evolution: semantic versioning, compatibility policy, migration path, consumer notification, deprecation window, successor product, usage-based retirement and audit trail for major changes.

- **Versioning contract.** Patch, minor and major changes have different blast-radius policies: cosmetic or non-breaking changes are not the same as changes to grain, formula, identity, allowed dimensions or access semantics.
- **Deprecation is not deletion.** A deprecated product remains discoverable with status, deadline, reason, owner, successor and migration guidance.
- **Sunset with consumer impact.** Before retirement, the platform must know the active consumers, dashboards, APIs, AI / ML jobs, downstream products, tenants and contractual obligations.
- **Rollback / recovery path.** If a new major version damages consumer trust or conflicts with a Super Truth decision, there must be a safe rollback or re-publication path through WAP and normal recovery.

**Boundaries with neighbouring isotopes:** WAP controls the publish decision; the Product Registry records the current state; the Composite Product Pattern exposes dependencies; the API / Event-Serving Pattern adds deprecation policy for endpoints and events; the Lifecycle Pattern keeps product evolution as a first-class discipline.

**Anti-pattern:** “Do not use the old table, although we are not deleting it yet” without status, successor, owner, usage analysis or sunset date. That is not deprecation, but architectural debt.

### Composite / Aggregate Product Pattern — the discipline of governed product composition

**Meaning of the isotope:** the Composite / Aggregate Product Pattern defines how Gold products may be built on top of one another without duplicating truth, creating circular dependencies or allowing product proliferation. Its central question is this: when is a new artefact a new base product, and when is it merely a slice, package, aggregate, dashboard, API representation or tenant-specific view of an existing product?

**What this isotope adds specifically:** Data Fountain permits consumer-aligned and aggregate products, but composition must be governed: upstream product contracts, the dependency graph, freshness propagation, compatibility expectations, lineage, blast radius and ownership split must all be explicit.

- **Source-aligned versus consumer-aligned boundary.** Not every consumer request creates a new base product. A new base product is justified only when a reusable semantic boundary or a distinct consumer contract appears.
- **Unidirectional composition.** An aggregate product may depend on lower-level or base products, but must not create cycles or backdoor logic that changes upstream meaning.
- **Dependency-aware freshness.** The SLA of an aggregate product cannot honestly be better than upstream freshness unless there is a separate materialisation or fallback policy.
- **Blast-radius control.** A change to an upstream metric, dimension, MDM identity or SCD policy must show which composite products, dashboards, APIs, AI contexts and decisions are affected.

**Boundaries with neighbouring isotopes:** Knowledge Graph / Ontology makes the dependency graph navigable; the Lifecycle Pattern governs version migration; Zero-Copy reduces copy-first aggregates; the Data Product Pattern maintains the registry; the Composite Pattern defines the rules for product-on-product composition.

**Anti-pattern:** every dashboard or tenant report becomes a new Gold base product with its own data copy and local logic. That is not product discipline, but uncontrolled multiplication of truth.

### API / Event-Serving Product Pattern — the discipline of governed API, service and stream serving

**Meaning of the isotope:** the API / Event-Serving Product Pattern defines how an approved Gold product becomes programmatically or real-time consumable through an API, service endpoint, event stream, webhook, reverse ETL or signal feed without losing contract discipline. Naming such as `gold_api_*`, `gold_svc_*` and `gold_sig_*` ought to mean not “yet another endpoint”, but a distinct serving contract.

**What this isotope adds specifically:** a tabular Gold product and an API / event product have rather different operational obligations. API / event serving requires versioning, compatibility, rate limits, authentication, policy propagation, ordering, replay / retention, idempotency, consumer groups, latency / freshness SLO, fallback behaviour and a deprecation policy.

- **API contract.** An endpoint must have a schema, version, authentication scope, rate limits, pagination, error model, freshness expectation, backward compatibility and sunset policy.
- **Event / stream contract.** A stream must describe event type, schema evolution, ordering guarantees, delivery semantics, retention, replay policy, consumer-group behaviour and idempotency key.
- **Serving does not create meaning.** An API or event feed must not locally invent mapping, KPI formula, identity resolution or policy logic; it serves the approved product contract.
- **Operational fallback.** If the real-time path is unhealthy, the consumer must know the expected behaviour: fail closed, last-known-good, stale-while-revalidate, degraded response or escalation.

**Boundaries with neighbouring isotopes:** CDC / Event Sourcing describes the birth of change evidence; Kappa guarantees replay-equivalent processing; Circuit Breaker and Backpressure protect the runtime; API / Event-Serving Product describes the consumer-facing serving contract for Gold products.

**Anti-pattern:** an endpoint or Kafka topic reads a curated table directly and is called a “data product”, but has no versioning, consumer contract, replay semantics, governance envelope or lifecycle. That is transport, not a product.

### Decision Product Pattern — the discipline of decision support as a Gold product

**Meaning of the isotope:** the Decision Product Pattern defines how a score, recommendation, alert, prediction, next-best action, risk signal or decision context becomes a governed Gold product. Unlike a Metric Product, which answers “what is happening?”, a Decision Product answers “what should be done about it, or which decision should this evidence support?”.

**What this isotope adds specifically:** a Decision Product has not only a data contract but also a decision contract: target decision, allowed use, human-in-the-loop boundary, confidence, explanation, policy constraints, fallback, monitoring, drift signals, model / rule version, evidence bundle and authority reference. It may rely on Platinum features or AI Signals, but it does not allow them to become business decisions in their own right without governance and a Super Truth boundary.

- **Decision boundary.** The product must state explicitly which decision it supports: approve / reject, prioritise, alert, recommend, escalate, segment or explain.
- **Confidence and explainability.** If the output influences a business action, the consumer must see confidence, reasons, key evidence, contract versions and limitations.
- **Human-in-the-loop.** For high-impact or policy-sensitive decisions, the product must define where automation is allowed and where a steward, manager or governance approval is required.
- **Fallback / refusal.** If evidence, freshness, policy permission or confidence is insufficient, the correct behaviour is refusal, escalation or a safe default, rather than confident improvisation.

**Boundaries with neighbouring isotopes:** Feature Store / Platinum provides features, embeddings and signals; Super Truth arbitrates approved interpretation; Metric Product provides measures; Decision Product packages them into a governed decision-support artefact with policy, confidence and explanation.

**Anti-pattern:** an ML score or LLM recommendation is used directly as a production decision without an owner, allowed-use contract, explainability, monitoring, fallback and Super Truth / governance boundary. That is not a Decision Product, but an unmanaged automation risk.

## Pattern isotopes of the decision-canonisation level

**The decision-canonisation isotopes** do not merely “produce the canon”; they form the full cycle of canonical meaning. They arbitrate approved terms, relationships, mappings, metric authority, decision records, ontology / graph semantics and precedence rules; protect the canon from source-specific, vendor-specific and legacy corruption; preserve it as versioned, evidence-backed and machine-readable decision state; make canonical assets discoverable through the catalogue / semantic-discovery surface; propagate that canon across all levels, planes and serving surfaces — governance policies, Gold products, Headless BI, Feature Store, GraphRAG, MCP / LLM context, APIs and dashboards; and continuously verify that downstream usage has not drifted away from approved meaning through semantic invariant tests and fitness functions.

### Canonical Authority Pattern — the discipline of final decision truth

**Canonical Authority / Super Truth Pattern** in Data Fountain is a decision-canonisation isotope: not a separate Semantic Layer, and not one more table above Gold, but the upper arbitration mechanism for meaning. It determines which interpretation is approved, versioned, explainable and fit for a business action.

This isotope is needed whenever local truths diverge. Bronze provides evidence truth; Silver, readiness truth; UDM, contract truth; URDP, reference truth; Gold, product truth; Platinum / Feature Store, feature or signal truth; and BI or MCP / LLM surfaces, consumption or explanation truth. All of these may be correct within their own boundaries, but the final decision for a KPI, AI Signal, executive report, customer-facing number or business-binding action must pass through Super Truth.

- **Final decision truth, not another data layer.** Super Truth does not duplicate Bronze, Silver, UDM, URDP, Platinum or Gold; it establishes precedence between local truths in a conflicting or critical context.
- **Decision boundary.** The pattern answers the question: which version of meaning is approved now, for this context, with these contract versions, lineage, confidence and governance envelope?
- **Precedence rules.** If a Gold metric, SCD as-of slice, URDP mapping, Feature Store signal or LLM explanation gives a different answer, Super Truth determines the approved interpretation, rejected alternatives and effective period.
- **Explainability.** Every decision must have an owner, definition / version, lineage, evidence bundle, confidence or quality context, allowed usage and a clear explanation of why this interpretation prevailed.
- **AI / MCP does not own truth.** An LLM, agent or MCP surface may explain, route, compare and escalate, but it must not independently invent a KPI, mapping, attribution rule or business status.

**Minimum structure of a canonical decision record:** every binding decision must record `decision_id`, decision type, approved interpretation, rejected alternatives, affected terms / metrics / mappings / products, owner / steward, approver, `effective_from` / `effective_to`, evidence bundle, contract versions, mapping versions, lineage reference, confidence or quality context, allowed usage, runtime implementation path, rollback / supersession rule and escalation path. Without such a record, a decision remains a verbal agreement rather than Super Truth.

The practical criterion is straightforward: if a production decision may affect a KPI, finance, customer-facing output, compliance-sensitive explanation or AI-driven action, the consumer must not choose the “convenient” version of truth. There must be a canonical decision record: what is approved, what was rejected, from which moment it applies, who owns it and which evidence and contracts support it.

### Federated Computational Governance Pattern — the discipline of automated policy enforcement

**Federated Computational Governance Pattern** in Data Fountain is a governance and policy-enforcement isotope which works alongside Canonical Authority, but does not replace it. Its role is not to decide what the correct business truth is; rather, it computes, applies and audits the rules of access, masking, row and column restrictions, classification, tenant / profile boundaries and policy-safe serving for truth that has already been approved.

In the short form: **Super Truth decides what the approved meaning is; Federated Computational Governance decides how that meaning is shown to different consumers safely, automatically and with evidence**. Domains retain ownership of their products and semantics, whilst policy primitives, classification vocabulary, access scopes, masking expectations and audit requirements must be machine-enforceable rather than informal agreements buried in chat threads.

- **Federated ownership with centralised policy logic.** Domains are accountable for the meaning and quality of their data products, but access, sensitivity, masking and tenant isolation ought to be applied through shared policy primitives.
- **Policy attaches to contracts, not random tables.** Governance should read UDM semantic contracts, URDP classification vocabulary, Gold product purpose, Feature Store reuse context and Super Truth decisions.
- **Computational, not documentary governance.** Classification, policy tags, row filters, authorised governed views, service identities, audit logs and access profiles must be artefacts the platform can apply, test and verify.
- **Governed projections rather than broad access.** Consumers should receive the permitted representation: full fields, masked fields, aggregates, tenant-filtered rows or restricted views, according to the active policy profile.
- **Audit-friendly enforcement.** It must be clear who saw what, through which contract, policy profile, published version, readiness state and serving boundary.

This isotope is particularly important for PII, CRM-sensitive, revenue-sensitive, tenant-bound and AI-serving scenarios. Its anti-pattern is manual IAM chaos, over-broad access, decorative metadata, policy by dashboard, or the LLM as policy engine. If sensitive data can be published or read without classification, a policy profile, a governed serving path and audit evidence, the platform does not yet have mature computational governance.

**Minimum policy-as-code envelope:** every production policy rule must carry `policy_id`, scope, owner, purpose, subject / service identity, resource class, sensitivity classification, allowed actions, row / column / tenant constraints, masking rule, purpose limitation, exception rule, effective period, audit requirement, enforcement point, test cases and authority reference. A policy without a machine-readable envelope becomes documentation; a policy with such an envelope can be applied, tested and verified automatically.

### Headless BI / Semantic Layer Pattern — the discipline of the governed semantic-serving boundary

**Headless BI / Semantic Layer** in Data Fountain is a governed semantic-serving boundary above approved semantics. Its task is to make metrics, dimensions, filters, breakdowns, KPI APIs, embedded analytics and BI-consumable definitions tool-neutral, reusable, explainable and API-first, without creating a parallel business truth.

This isotope does not invent entity semantics, reference mappings or final KPI arbitration. It consumes UDM contract truth, URDP reference truth, Gold / Data Product packaging and Super Truth decisions, and then publishes approved metric semantics in a form fit for BI tools, APIs, AI assistants, MCP consumers, embedded analytics and self-service exploration.

What remains specifically within the Headless BI / Semantic Layer isotope:

- **Metric definition contract.** Formula, numerator / denominator, grain, time logic, null policy, allowed filters, owner, version and freshness expectation must be fixed before any dashboard calculation appears.
- **Allowed dimensions contract.** The semantic layer defines which breakdowns are legal for a given metric, which dimensions are compatible with its grain, and which combinations create fan-out, misleading slices or semantic mismatch.
- **Metric lineage contract.** Every metric must trace back to UDM facts / dimensions, URDP mappings, Gold dependencies, build IDs, mapping versions and Super Truth / authority decisions.
- **Tool-neutral serving.** Power BI, Tableau, Looker, Superset, internal dashboards, metric APIs, embedded UI and AI consumers should consume one approved semantic surface rather than local formulae in every tool.
- **Authority escalation.** The semantic layer may serve approved consumption truth, but official or conflict-prone KPIs require an escalation path to Canonical Authority / Super Truth.

**Practical rule:** if a KPI exists only as a Power BI measure, Tableau calculation, LookML-only expression, dashboard SQL or prompt template, it is shadow metric semantics. The correct path is: business term → metric contract → rules → lineage → semantic object / metric API / BI adapter.

**Canonical naming for semantic serving:** production semantic-layer objects should be named by role: `sem_metric_*` for metrics and KPIs, `sem_dim_*` for allowed dimensions / breakdowns, `sem_view_*` for governed semantic views, and `sem_dec_*` for decision-aware metric surfaces or authority-linked decision outputs. Naming does not create meaning on its own, but it makes approved semantic objects discoverable and reusable, and distinguishes them from UDM contracts, Gold products and ad hoc BI calculations.

**Anti-pattern:** the semantic layer as shadow UDM or vendor-owned meaning. If a BI semantic tool begins to define entities, IDs, mappings or final KPI rules around UDM / URDP / Super Truth, it stops being a serving boundary and becomes a parallel source of truth.

### Knowledge Graph / Ontology Pattern — the discipline of governed graph semantics

**Knowledge Graph / Ontology Pattern** in Data Fountain is a relation-aware semantic isotope which makes canonical entities, relationships, concepts, taxonomies, rules, evidence paths, products, features and decisions graph-discoverable, machine-navigable, explainable and reusable without creating a separate graph truth.

This isotope does not replace UDM, URDP, MDM or Super Truth. UDM answers the question “which canonical objects and contracts exist?”; URDP answers “which reference meanings and mappings are in force?”; Super Truth answers “which interpretation is approved?”; and Knowledge Graph / Ontology answers “how is approved meaning connected, traversable and explainable for discovery, GraphRAG, impact analysis, AI context and semantic navigation?”.

What remains specifically within the Knowledge Graph / Ontology isotope:

- **Ontology registry.** A governed vocabulary of concept classes, relation types, constraints, owners, versions and status: entity, event, policy, metric, feature, product, mapping, authority decision, exception and taxonomy node.
- **Typed semantic relationships.** Edges such as `depends_on`, `maps_to`, `belongs_to`, `approved_by`, `explained_by`, `derived_from`, `governs`, `deprecated_by` and `effective_during` must rest upon approved contracts and lineage.
- **Semantic lineage graph.** The graph should show meaning paths — concept → rule → mapping → contract → feature → product → decision → evidence — rather than merely technical data movement.
- **GraphRAG / Hybrid RAG context.** AI retrieval ought to receive not just similar chunks, but a policy-aware subgraph, authority status, evidence path and approved relationship context.
- **Impact analysis.** A change to a mapping, canonical concept, feature definition, product contract or decision should show which KPIs, products, dashboards, prompts, policies, explanations and AI Signals are affected.

**Practical rule:** the graph is built on top of governed semantics, not directly from raw or consumer-specific assumptions. Critical nodes and edges must have lineage to UDM / URDP / Super Truth or an approved product contract, a policy-aware access scope and versioning / effective dating wherever relationship semantics affect meaning.

**Anti-pattern:** a handsome enterprise graph without ownership, contracts and authority alignment. If an ontology or graph edge quietly changes a KPI, access, AI interpretation or official definition, that change must pass through UDM / URDP / Super Truth governance rather than remaining “a graph edit”.

### Anti-Corruption Layer Pattern — the discipline of protecting the canon from semantic corruption

**Anti-Corruption Layer** in Data Fountain is a boundary discipline which prevents source-specific, vendor-specific or legacy semantics from quietly leaking into the canonical model, UDM, Gold products, Semantic Layer, Feature Store or AI context. Its role is not to create meaning, but to protect already defined canonical meaning at the boundary between an external / legacy bounded context and Data Fountain.

This isotope is needed wherever a source system has its own model of the world: CRM statuses, billing codes, SaaS-specific enums, partner API payloads, legacy IDs, channel labels, application states or vendor-defined objects. Without an ACL, these source models readily become shadow semantics: dashboard SQL reads a source column directly, a feature pipeline copies a vendor status, a prompt receives a raw label, and UDM is left to explain after the event why meanings have diverged.

- **Boundary before mapping.** ACL fixes the fact that source model ≠ canonical model. The move from source to canonical must pass through an explicit translation / adaptation boundary, rather than through an ad hoc SQL cast, dashboard expression or notebook logic.
- **Translation as a governed artefact.** Mapping, normalisation, fallback, quarantine behaviour, confidence and effective period must be first-class artefacts with lineage to URDP / UDM / Super Truth, not local code inside a pipeline.
- **Protection from vendor leakage.** A vendor-specific status, field name, enum or object shape must not become a business term merely because the source API happens to name a field that way.
- **Contract-preserving adaptation.** If a source changes semantics, ACL must stop or re-adapt the flow through contract review, quarantine, mapping update or Super Truth escalation, rather than quietly changing downstream meaning.

**Boundaries with neighbouring isotopes:** URDP defines mapping rules; ACL defines the architectural boundary that prevents those rules from being bypassed. UDM defines the canonical contract; ACL protects it from the source-specific model. Canonical Authority / Super Truth arbitrates meaning conflicts; ACL reduces the number of such conflicts by keeping non-canonical semantics out of the normal path.

**Anti-pattern:** a Gold dashboard, Feature Store job, BI calculation or LLM context directly uses CRM status codes, legacy source names, vendor object labels or partner API enums without governed adaptation through the ACL / URDP / UDM path. That is not fast delivery, but semantic corruption by shortcut.

### Data Catalog / Semantic Discovery Pattern — the discipline of governed discovery of the canon

**Data Catalog / Semantic Discovery** in Data Fountain is the discoverability isotope for canonical assets. If Canonical Authority produces approved meaning, Knowledge Graph makes that meaning traversable, and Headless BI serves approved metrics, the Catalog answers the consumer’s practical question: where does one find the correct term, metric, product, mapping, policy, owner, lineage, decision or approved version?

Without a catalogue, the canon may exist formally yet fail organisationally: UDM terms live in one place, URDP mappings in another, Gold products in tables, metric definitions in BI, policy rules in documentation, and Super Truth decisions in Confluence or Slack. The result is drearily predictable: the consumer cannot find the approved asset and quietly creates local truth.

- **Canonical asset registry.** The catalogue must register UDM terms, URDP mappings, Gold products, metric products, feature definitions, governance profiles, policies, decision records, ontology concepts and serving surfaces.
- **Searchable, not tribal.** Approved meaning ought to be searchable, documented, versioned, linked to an owner and available through self-service discovery, rather than through “ask the person who remembers”.
- **Lineage and authority visibility.** The consumer should be able to see where the asset came from, which contract and version are in force, who owns it, which governance profile applies, which dependencies exist and which Super Truth / authority reference supports it.
- **Discovery before duplication.** Before a new product, metric, feature or mapping is created, the platform should provide a way to find the existing approved artefact and decide whether it can be reused.

**Boundaries with neighbouring isotopes:** Knowledge Graph provides semantic navigation and relationship traversal; the Catalog provides the discovery surface: search, ownership, documentation, classification, lifecycle and usage context. The Data Product Registry fixes the Gold lifecycle; the Catalog connects that registry with UDM, URDP, policies, Feature Store, Semantic Layer and Super Truth decisions. Headless BI serves metrics; the Catalog helps users find, understand and verify the approved metric contract.

**Anti-pattern:** canonical terms, mappings, metrics, policies and decision records exist, but are scattered across Confluence, spreadsheets, Slack threads, notebook comments, BI workbooks and prompt templates without a machine-readable catalogue, ownership, lineage, search and status. That is not a canon, but an archive of agreements.

### Fitness Function / Semantic Invariant Testing Pattern — the discipline of continuous verification of the canon

**Fitness Function / Semantic Invariant Testing** in Data Fountain is the continuous semantic-verification isotope: an automated discipline for proving that canonical rules, metric formulae, mappings, product contracts, policies, features and decision records are not merely approved, but are still being honoured downstream after publication.

WAP checks candidate readiness before publication, DQ gates check quality, and Governance enforces policies; yet that is not sufficient for the long life of a canon. The canon can drift. A metric formula in BI may lag behind a Super Truth decision; a Gold SCD projection may use an old mapping version; the Feature Store may carry different transformation logic; prompt context may still contain a deprecated term; or a policy tag may remain stale. Fitness Functions turn such invariants into runnable tests, which is rather the point: the canon must be continuously provable, not merely agreed once.

- **Semantic invariants as tests.** An approved formula, mapping rule, identity contract, temporal rule, governance profile or authority decision ought to have automated checks that can be run regularly and whenever its dependencies change.
- **Post-publish correctness.** Verification does not end at publication. After release, the platform must check that downstream products, dashboards, APIs, features, embeddings, prompts and semantic views have not drifted away from approved meaning.
- **Drift detection.** Fitness functions should catch metric drift, mapping drift, schema / semantic mismatch, deprecated-term usage, stale policy tags, broken lineage, invalid effective periods and training-serving semantic skew.
- **Executable governance evidence.** Test results must become evidence for the Control, Governance and Super Truth planes: pass / fail, scope, affected assets, severity, owner, remediation path and blocking or warning behaviour.

**Boundaries with neighbouring isotopes:** WAP checks the candidate before publication; Fitness Functions check ongoing correctness after publication. DQ gates check data quality; Fitness Functions check semantic and architectural invariants. Federated Computational Governance enforces policy; Fitness Functions test that enforcement and downstream usage have not diverged from approved rules.

**Anti-pattern:** a canonical decision has been taken and a metric contract updated, yet dashboards, BI calculations, API responses, Feature Store jobs or LLM prompts continue to use the old formula, mapping or deprecated term — and the mismatch is discovered only after a business incident. That is not governed evolution; it is canon drift left unattended.

# By way of conclusion

An attentive reader, having reached the end, may quite reasonably sigh and say:

“All of this is fine, but it is rather complex. How is one actually meant to use it?”

My friend, let us remember that we are living in the age of AI. The concept deliberately begins with the label **AI-ready**.

Suppose you already have a working project which you would like to modify in accordance with **Data Fountain**. Suppose, too, that the project structure and its description already live, for example, in your corporate Atlassian Confluence.

In that case, the practical route is quite straightforward:

1. Copy this article into Atlassian Confluence.
2. Ask the local AI agent to generate a Bronze page for the project, one that follows the Data Fountain concept (point it to the copied page), extends the model of the existing project (point it to the central project page), and, if required, takes into account an additional set of components, selection principles, technology constraints or any other parameters that matter in your environment. Given a clear **AI-ready** concept, the agent should be able to produce a sound configuration option with detailed descriptions, required fields, contracts and implementation notes.
3. Proceed in the same way for all further layers. Then generate child pages for the individual components and ask for the appropriate component configurations, field names, contracts, readiness criteria and other records needed to make the architecture executable rather than merely decorative.

After that, even if you do not yet have a finished implementation or a fully operational project, you will at least have a proper architectural frame for the future data platform. Why not a finished platform straight away? Because AI agents, however useful and occasionally impressive they may be, remain somewhat capricious assistants. The need to think has not been abolished by the age of AI. The chief architect of an elegant and distinctive Data Fountain is still you.

Enjoy it.

# References

- [Databricks — Medallion Architecture](https://www.databricks.com/glossary/medallion-architecture)
- [Databricks Docs — What is the medallion lakehouse architecture?](https://docs.databricks.com/aws/en/lakehouse/medallion)
- [Azure Databricks — Medallion lakehouse architecture](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion)
- [Databricks — What is a Data Lakehouse?](https://www.databricks.com/blog/what-is-data-lakehouse)
- [CIDR 2021 — Lakehouse: A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics](https://www.cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf)
- [James Dixon — Pentaho, Hadoop, and Data Lakes](https://jamesdixon.wordpress.com/2010/10/14/pentaho-hadoop-and-data-lakes/)
- [James Dixon — Data Lakes Revisited](https://jamesdixon.wordpress.com/2014/09/25/data-lakes-revisited/)
- [Dataversity — A Brief History of Data Lakes](https://www.dataversity.net/articles/brief-history-data-lakes/)
- [Sam Newman — Building Microservices, 2nd Edition](https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/)
- [Zhamak Dehghani / Martin Fowler — How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh](https://martinfowler.com/articles/data-monolith-to-mesh.html)
- [Zhamak Dehghani — Data Mesh Principles and Logical Architecture](https://www.thoughtworks.com/insights/blog/data-mesh/data-mesh-principles-and-logical-architecture)
- [Zhamak Dehghani — Data Mesh: Delivering Data-Driven Value at Scale](https://www.thoughtworks.com/insights/books/data-mesh)
- [Thoughtworks — Data Mesh in practice: Getting off to the right start](https://www.thoughtworks.com/insights/articles/data-mesh-in-practice-getting-off-to-the-right-start)
- [Martin Fowler — Fitness Functions](https://martinfowler.com/articles/fitness-functions.html)
- [Martin Fowler — Data Mesh Principles: Federated Computational Governance](https://martinfowler.com/articles/data-mesh-principles.html#FederatedComputationalGovernance)
- [Martin Fowler — Governing data products using fitness functions](https://martinfowler.com/articles/data-products-fitness-functions.html)
- [Open Policy Agent — Policy-as-code](https://www.openpolicyagent.org/)
- [DAMA International — DAMA-DMBOK Body of Knowledge](https://www.dama.org/cpages/body-of-knowledge)
- [NIST — AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [ISO/IEC 42001 — Artificial intelligence management system](https://www.iso.org/standard/81230.html)
- [AWS Well-Architected — Control plane and data plane](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/control-plane-and-data-plane.html)
- [AWS — Control planes and data planes](https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/control-planes-and-data-planes.html)
- [HashiCorp Well-Architected — Control, management, and data planes](https://developer.hashicorp.com/well-architected-framework/operational-excellence/operational-excellence-control-management-data-planes)
- [IBM — Control plane vs. data plane](https://www.ibm.com/think/topics/control-plane-vs-data-plane)
- [IETF RFC 7426 — Software-Defined Networking: Layers and Architecture Terminology](https://www.rfc-editor.org/rfc/rfc7426.html)
- [Kubernetes Architecture](https://kubernetes.io/docs/concepts/architecture/)
- [TOGAF ADM — Architecture Development Method](https://pubs.opengroup.org/togaf-standard/adm/chap04.html)
- [Martin Fowler — Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
- [Martin Fowler — Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [Jay Kreps — Questioning the Lambda Architecture](https://www.confluent.io/blog/questioning-the-lambda-architecture/)
- [Jay Kreps — I Heart Logs: Event Data, Stream Processing, and Data Integration](https://www.amazon.com/Building-Scalable-Streaming-Systems-Architecture/dp/1491944249)
- [Gregor Hohpe, Bobby Woolf — Enterprise Integration Patterns](https://www.enterpriseintegrationpatterns.com/)
- [Enterprise Integration Patterns — Canonical Data Model](https://www.enterpriseintegrationpatterns.com/CanonicalDataModel.html)
- [Enterprise Integration Patterns — Dead Letter Channel](https://www.enterpriseintegrationpatterns.com/patterns/messaging/DeadLetterChannel.html)
- [Enterprise Integration Patterns — Correlation Identifier](https://www.enterpriseintegrationpatterns.com/patterns/messaging/CorrelationIdentifier.html)
- [Enterprise Integration Patterns — Idempotent Receiver](https://www.enterpriseintegrationpatterns.com/patterns/messaging/IdempotentReceiver.html)
- [lakeFS — Data Engineering Patterns: Write-Audit-Publish](https://lakefs.io/blog/data-engineering-patterns-write-audit-publish/)
- [Data Vault Alliance](https://datavaultalliance.com/)
- [Dan Linstedt — Introduction to Data Vault 2.0](https://www.danlinstedt.com/allposts/datavaultcat/intro-to-data-vault-2-0/)
- [Feast — Open Source Feature Store](https://feast.dev/)
- [Tecton — What is a Feature Store?](https://www.tecton.ai/blog/what-is-a-feature-store/)
- [Microsoft GraphRAG](https://microsoft.github.io/graphrag/)
- [Neo4j — What is GraphRAG?](https://neo4j.com/blog/genai/what-is-graphrag/)
- [Robert C. Martin — Clean Architecture](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)
- [Databricks — Semantic Layer Architecture: Components, Design Patterns, and AI Integration](https://www.databricks.com/blog/semantic-layer-architecture-components-design-patterns-and-ai-integration)
- [IBM — What Is a Semantic Layer?](https://www.ibm.com/think/topics/semantic-layer)
- [E. F. Codd — A Relational Model of Data for Large Shared Data Banks](https://dl.acm.org/doi/10.1145/362384.362685)
- [IBM — The relational database](https://www.ibm.com/history/relational-database)
- [ISO/IEC 9075:2023 — SQL standard](https://www.iso.org/standard/76583.html)
- [Gerald Venzl — Announcing the general availability of the SQL:2023 Standard](https://www.geraldonit.com/announcing-the-general-availability-of-the-sql2023-standard/)
- [C. J. Date and Hugh Darwen — The Third Manifesto](https://www.thethirdmanifesto.com/)
- [SWI-Prolog](https://www.swi-prolog.org/)
- [Maier, Tekle, Kifer, Warren — Datalog: Concepts, History, and Outlook](https://dl.acm.org/doi/10.1145/3459664.3459667)
- [Clojure — Rationale](https://clojure.org/about/rationale)
- [Erlang documentation — Concurrent Programming](https://www.erlang.org/doc/system/conc_prog.html)
- [Elixir — Introduction](https://elixir-lang.org/getting-started/introduction.html)
- [Anthropic — Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol)
- [Databricks — What is Model Context Protocol?](https://www.databricks.com/blog/what-is-model-context-protocol)
- [Anthropic Engineering — Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Anthropic Engineering — Code execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp)
- [dbt Labs — Building an AI-ready data platform for generative AI](https://www.getdbt.com/blog/ai-ready-platform-generative-ai)
- [dbt Labs — How the semantic layer helps prevent AI hallucinations](https://www.getdbt.com/blog/semantic-layer-ai-hallucinations)
- [Atlan — Active Data Governance / Context Layer for AI](https://atlan.com/active-data-governance/)
- [K2View — LLM Grounding](https://www.k2view.com/llm-grounding/)
- [Tetrate — LLM Hallucination Prevention](https://tetrate.io/learn/ai/llm-hallucination-prevention)
- [Chata.ai — How to Prevent LLM Hallucinations in Production AI Systems](https://chata.ai/resources/blog/how-to-prevent-llm-hallucinations-in-production-ai-systems)
- [Halloran et al. — MCP Safety Audit (arXiv)](https://arxiv.org/abs/2504.03767)
- [Checkmarx — 11 MCP Security Risks and How to Mitigate Them](https://checkmarx.com/blog/11-mcp-security-risks-and-how-to-mitigate-them/)
- [Docker — MCP Security Issues](https://www.docker.com/blog/mcp-security-issues/)
- [Saga pattern — microservices.io](https://microservices.io/patterns/data/saga.html)
- [Saga — Patterns of Distributed Systems](https://martinfowler.com/articles/patterns-of-distributed-systems/saga.html)
- [Process Manager — Enterprise Integration Patterns](https://www.enterpriseintegrationpatterns.com/patterns/messaging/ProcessManager.html)
- [AWS Prescriptive Guidance — Saga pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga.html)
- [Saga distributed transactions pattern — Microsoft Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/patterns/saga)
- [Saga pattern made easy — Temporal](https://temporal.io/blog/saga-pattern-made-easy)
- [Apache Iceberg branching and tagging](https://iceberg.apache.org/docs/latest/branching/)
- [dbt state-aware orchestration concepts](https://docs.getdbt.com/docs/deploy/state-aware-setup)
- [Data lakehouse architecture — Google Cloud](https://cloud.google.com/architecture/data-lakehouse)
- [BigQuery Iceberg tables docs](https://cloud.google.com/bigquery/docs/iceberg-tables)
- [Google Cloud Storage](https://cloud.google.com/storage)
- [Hexagonal Architecture — Alistair Cockburn](https://alistair.cockburn.us/hexagonal-architecture/)
- [Palantir Foundry — Ontology Best Practices](https://www.palantir.com/docs/foundry/ontology/ontology-best-practices)
- [Microsoft Research — GraphRAG project](https://www.microsoft.com/en-us/research/project/graphrag/)
- [IBM Think — What is GraphRAG?](https://www.ibm.com/think/topics/graphrag)
- [Enterprise Knowledge — Best Practices for Enterprise Knowledge Graph Design](https://enterprise-knowledge.com/best-practices-for-enterprise-knowledge-graph-design/)
- [Enterprise Knowledge — Graph Analytics in the Semantic Layer](https://enterprise-knowledge.com/graph-analytics-in-the-semantic-layer-architectural-framework-for-knowledge-intelligence/)
- [Atlan — RDF vs OWL](https://atlan.com/know/rdf-vs-owl/)
- [Introduction to Semantic Graphs and RDF](https://graph.build/resources/semantic-graphs)
- [Alation — How to Write AI-Ready Documentation](https://www.alation.com/blog/how-to-write-ai-ready-documentation/)
- [Martin Kleppmann — Prediction: AI will make formal verification go mainstream](https://martin.kleppmann.com/2025/12/08/ai-formal-verification.html)
- [SAP Community — Data Products: Structured Reusable Analytical Assets](https://community.sap.com/t5/data-professionals-blog-posts/business-data-architecture-data-products-structured-reusable-analytical/ba-p/14432303)
- [Alation — 5 Types of Data Products](https://www.alation.com/blog/data-product-types/)
- [TechTarget — What is a Metrics Store?](https://www.techtarget.com/searchbusinessanalytics/definition/metrics-store)
- [Modern Data 101 — The Data Product Strategy: Becoming Metrics-First](https://medium.com/@community_md101/the-data-product-strategy-becoming-metrics-first-23e8e0397a20)
- [Gartner — Top Trends in Data and Analytics for 2025](https://www.gartner.com/en/newsroom/press-releases/2025-03-05-gartner-identifies-top-trends-in-data-and-analytics-for-2025)
- [Modern Data 101 — Managing the Evolving Data Products Landscape: Versioning, Cataloging, Decommissioning](https://moderndata101.substack.com/p/managing-the-evolving-data-products-landscape-p2)
- [Alation — Data Product Lifecycle](https://www.alation.com/blog/data-product-lifecycle/)
- [Atlan — Data Deprecation Process](https://atlan.com/know/data-deprecation-process/)
- [Rewire — How Data Product Versioning Can Make or Break Your Federated Data Strategy](https://rewirenow.com/en/resources/blog/how-data-product-versioning-can-make-or-break-your-federated-data-strategy/)
- [DataOS — Types of Data Products](https://dataos.info/learn/understand_data_products/)
- [Modern Data Company — How to Organize Your Data Product Ecosystem](https://www.themoderndatacompany.com/blog/how-to-organize-your-data-product-ecosystem)
- [Martin Fowler — Designing Data Products](https://martinfowler.com/articles/designing-data-products.html)
- [Collate — Data Product Types](https://www.getcollate.io/learning-center/data-product)
- [Coalesce — What is a Data Product?](https://coalesce.io/data-insights/what-is-a-data-product/)
- [Striim — Data Mesh and Event Streams](https://www.striim.com/blog/data-mesh-event-stream-architecture/)
- [IBM — What is Event Streaming?](https://www.ibm.com/think/topics/event-streaming)
- [Atlan — What is a Data Product?](https://atlan.com/know/what-is-a-data-product/)
- [Colrows — Data Products Are Dead, Long Live Semantic Products](https://colrows.com/blogs/data-products-are-dead-long-live-semantic-products/)
- [DataCamp — What is a Data Product?](https://www.datacamp.com/blog/data-product)
- [Microsoft Azure Architecture Center — Anti-Corruption Layer pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer)
- [AWS Prescriptive Guidance — Anti-corruption layer pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/acl.html)
- [Alation — Canonical Data Models explained](https://www.alation.com/blog/canonical-data-models-explained-benefits-tools-getting-started/)
- [Neal Ford, Rebecca Parsons, Patrick Kua — *Building Evolutionary Architectures* (O'Reilly)](https://www.thoughtworks.com/insights/books/building-evolutionaryarchitectures-second-edition)
