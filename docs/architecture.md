Aion Node Architecture v0.1
1. Overview

An Aion node is a deterministic validation engine that:

Maintains the DAG ledger

Maintains the UTXO set

Participates in probabilistic consensus

Validates post-quantum transactions

Communicates with peers via gossip

Exposes a local API interface

The node is modular by design.

Each subsystem must be independently testable and deterministic.

2. High-Level Architecture
+--------------------+
|       API Layer    |
+--------------------+
|     Consensus      |
+--------------------+
|        DAG         |
+--------------------+
|       UTXO         |
+--------------------+
|      Mempool       |
+--------------------+
|        P2P         |
+--------------------+
|      Storage       |
+--------------------+
|       Crypto       |
+--------------------+

All modules operate over deterministic serialized structures defined in data-model.md.

3. Module Responsibilities
3.1 Crypto Module

Responsible for:

SHA3-512 hashing

Dilithium signature verification

Domain separation enforcement

Key validation rules

Properties:

Stateless

Pure functions

No network access

No storage access

3.2 Storage Module

Responsible for:

Persistent DAG storage

Persistent UTXO set

Transaction index

Peer metadata

Confidence scores

Requirements:

Crash consistency

Atomic updates

Deterministic reads

Likely backend (initial version):

RocksDB or sled (Rust-native)

3.3 DAG Module

Responsible for:

In-memory DAG representation

Parent validation

Causal ordering

Conflict tracking

Depth calculation

Properties:

No cryptographic verification

No signature checking

Pure structural logic

3.4 UTXO Module

Responsible for:

Tracking unspent outputs

Validating referenced outputs

Detecting double-spends

Updating state after confirmed transactions

Operates only after structural validation.

3.5 Mempool Module

Responsible for:

Holding pending transactions

Filtering invalid transactions

Conflict pre-checking

Prioritization (future)

Transactions remain in mempool until sufficient confidence.

3.6 Consensus Module

Responsible for:

Sampling peers

Querying transaction preferences

Updating confidence_score

Resolving conflicts probabilistically

Determining metastable preference

Properties:

Asynchronous

Probabilistic

Eventually consistent

No global ordering exists.

3.7 P2P Module

Responsible for:

Peer discovery

Gossip propagation

Request/response protocol

Transaction broadcast

DAG synchronization

Must resist:

Spam

Eclipse attacks

Malformed data

3.8 API Module

Responsible for:

Wallet interaction

Transaction submission

Querying transaction status

Querying confidence

Node metrics

Likely initial form:

JSON-RPC over HTTP

4. Transaction Validation Pipeline

When a transaction is received:

P2P receives serialized transaction

Crypto verifies basic structure & signature

DAG validates parent references

UTXO checks referenced outputs

Mempool inserts transaction

Consensus begins sampling process

Confidence score accumulates

UTXO set updated after threshold reached

5. Determinism Rules

All nodes must:

Use identical hashing rules

Use identical serialization rules

Reject malformed or ambiguous structures

Avoid non-deterministic floating-point operations

Consensus safety depends on deterministic validation.

6. Security Model

Assumptions:

Adversaries may have large-scale quantum capabilities

Network may contain malicious peers

Messages may be reordered

Sybil nodes may exist

Design priority:

Structural correctness > performance.
