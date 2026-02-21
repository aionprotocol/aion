# Aion Node Architecture v0.1

## 1. Overview

Aion nodes are designed as modular systems that enforce deterministic
state transitions over a DAG-based ledger.

Each module has a clearly defined responsibility to ensure:

- correctness
- reproducibility
- cryptographic integrity
- consensus convergence

The architecture is intentionally layered.

---

## 2. Node Modules

A node consists of the following logical components:

### 2.1 Crypto Layer

Responsible for:

- SHA3-512 hashing
- Dilithium signature verification
- Locking hash validation

This layer must be deterministic and side-effect free.

---

### 2.2 Storage Layer

Responsible for:

- Transaction persistence
- UTXO set persistence
- DAG state storage

Must ensure:

- crash consistency
- deterministic retrieval

---

### 2.3 DAG Layer

Responsible for:

- Transaction insertion
- Parent reference validation
- Conflict detection

Rules:

- All non-genesis transactions must reference at least two parents.
- Genesis is allowed to have zero parents.

---

### 2.4 UTXO Layer

Responsible for:

- Tracking unspent outputs
- Preventing double spend
- Maintaining economic state

State transitions occur only after:

- structural validation
- cryptographic validation
- DAG validation

---

### 2.5 Mempool Layer

Responsible for:

- Storing valid but unfinalized transactions
- Managing conflicts
- Feeding consensus sampling

---

### 2.6 Consensus Layer

Responsible for:

- Probabilistic sampling of peers
- Preference propagation
- Confidence accumulation

Consensus determines:

- which transactions become accepted
- which conflicts are rejected

---

### 2.7 API Layer

Responsible for:

- External interaction
- Transaction submission
- State queries

Must not bypass validation pipeline.

---

## 3. Validation Pipeline

Incoming transactions follow this order:

1. Structural validation
2. Cryptographic validation
3. DAG validation
4. UTXO validation
5. Mempool admission
6. Consensus evaluation
7. Final state transition

---

## 4. Genesis Handling

If the DAG is empty:

- the first transaction received is treated as genesis
- parent validation is skipped
- UTXO outputs are inserted directly

Genesis initializes the economic state.

---

## 5. Determinism Requirements

All nodes must:

- serialize identically
- hash identically
- sort parents identically
- avoid non-deterministic logic

Failure to do so may result in consensus divergence.

---

## 6. State Transition Rule

The UTXO set is updated only when:

- consensus confidence exceeds threshold

This separates:

- structural validity
- economic validity
- canonical acceptance
