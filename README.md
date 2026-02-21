# Aion

Aion is a formally specified distributed ledger protocol built around a blockless DAG architecture.

It is designed to provide:

- Asynchronous transaction ordering
- Probabilistic consensus
- Confidence-based finality
- Emergent time derived from causality
- Post-quantum security assumptions

Rather than relying on linear block production or leader-based coordination, Aion models ledger evolution as a causally-structured graph of transactions.

This enables order without rigid sequencing, and finality without deterministic locking.


---

## Overview

Aion is built on a Directed Acyclic Graph (DAG) transaction structure combined with a UTXO-based accounting model.

Transactions reference prior events directly, forming a causally consistent structure from which ordering and settlement confidence can emerge.

Consensus is not expressed through block production, but through probabilistic validation and confidence accumulation across the graph.

Time is not imposed externally, but derived from transaction relationships.


---

## Core Design Principles

### Blockless Ledger

Aion eliminates block production entirely.  
The ledger evolves as a transaction graph rather than a sequence of blocks.

### Probabilistic Consensus

Instead of deterministic finality through majority agreement, Aion is designed to converge through metastable sampling and confidence accumulation.

### Confidence-Based Finality

Settlement emerges asymptotically as confidence increases across transaction history.

### Emergent Time

Ordering arises from causal structure rather than timestamps or leader schedules.

### Post-Quantum Orientation

Signature and identity layers are designed to support post-quantum cryptographic primitives.


---

## Implementation Status

Aion is a formally specified distributed ledger protocol currently undergoing staged implementation.

The protocol design defines a DAG-based transaction ledger with a UTXO model, probabilistic consensus, confidence-based finality, and post-quantum security assumptions.

The current Rust node represents an early materialization of this specification and includes:

- DAG-based ledger structure (in-memory)
- Functional UTXO model
- Deterministic structural validation
- Multi-parent transaction support
- Canonical output identifiers
- Double-spend protection
- Genesis transaction exception handling
- Modular validation pipeline

The following protocol components are already specified at the design level but are **not yet implemented** in the current node:

- Probabilistic metastable consensus
- Confidence score accumulation
- Emergent time ordering
- Post-quantum signature scheme
- Peer-to-peer networking layer
- Distributed validator participation
- Incentive or reward mechanisms

As such, the existing implementation should be understood as a structural execution layer aligned with the protocol specification, rather than a complete realization of its consensus and security model.


---

## Documentation

The formal protocol specification is available in:

- `protocol-spec.md`
- `data-model.md`
- `architecture.md`
- `transaction-lifecycle.md`

These documents describe the intended system behavior independently of implementation stage.


---

## Current Node

The existing Rust node allows:

- Genesis insertion
- Valid transaction insertion
- Double-spend rejection

The node currently operates as an in-memory execution engine aligned with the protocol specification.


---

## Development Direction

The next stages of implementation include:

- Separation of validation layers
- Mempool integration
- Persistent storage
- Cryptographic signature layer
- Network layer
- Confidence-based consensus


---

## Status

Aion should be understood as a protocol with a defined architectural direction and an implementation progressing toward that specification.

---

## License

AGPL-3.0  
(to ensure openness remains reciprocal)

© 2026 Aion Protocol
