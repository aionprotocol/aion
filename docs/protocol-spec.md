# Aion Protocol Specification v0.2 (Post-Quantum Conservative Architecture)

## 1. Overview

Aion is a blockless distributed ledger protocol built under a conservative threat model.

The system assumes that adversaries may possess large-scale quantum computational capabilities.

Aion is therefore designed to remain secure even if classical public-key cryptography becomes vulnerable.

Core characteristics:

- Directed Acyclic Graph (DAG) ledger structure
- UTXO-based state model
- Probabilistic metastable consensus
- Confidence-based finality
- Emergent logical time
- Fully post-quantum cryptography

---

## 2. Design Principles

1. No reliance on classical signature schemes.
2. No backward compatibility with quantum-vulnerable cryptography.
3. Minimal exposure of public keys.
4. Fresh-key usage per output.
5. Structural security over convenience.

---

## 3. Ledger Structure

The ledger is represented as a Directed Acyclic Graph (DAG).

- Vertices = Transactions
- Edges = Parent references

Each transaction must reference at least two previous valid transactions.

No global linear ordering exists.

Ordering emerges from causal structure and validator convergence.

---

### 3.1 Bootstrap Exception (Genesis Rule)

The requirement that each transaction references at least two parents
applies to all transactions except the genesis transaction.

The genesis transaction is defined as:

- a transaction with:
  - zero inputs
  - zero parents
  - at least one output

Its purpose is solely to initialize the UTXO set.

Genesis is accepted unconditionally by all nodes and serves as the
root of the DAG.

All subsequent transactions must reference at least two parents.

---

### 3.2 Parent Ordering

Parent references must be serialized in deterministic order.

Nodes MUST sort parent transaction IDs lexicographically
before computing tx_id.

This prevents hash divergence across implementations.

---

## 4. Transaction Model (UTXO — Post-Quantum)

Aion uses a UTXO model adapted for a DAG-based ledger.

Each transaction consists of:

### 4.1 Header

- `tx_id` — SHA3-512 hash of the serialized transaction
- `parents` — array of at least two previous transaction IDs
- `timestamp` — local timestamp (non-authoritative)

---

### 4.2 Inputs

Each input contains:

- `referenced_output_id`
- `public_key` (CRYSTALS-Dilithium)
- `signature` (Dilithium signature over transaction body)

Rules:

- Each input must reference an unspent output.
- Public keys are revealed only at spend time.
- Signature must validate against the locking condition.
- Double-spend attempts are resolved via probabilistic consensus.

---

### 4.3 Outputs

Each output contains:

- `amount`
- `locking_hash` = SHA3-512(public_key)

Rules:

- Each output must correspond to a fresh public key.
- Wallet implementations must avoid key reuse.

---

## 5. UTXO Set

The UTXO set contains all currently unspent outputs.

An output is removed from the UTXO set once referenced by a valid transaction input.

Conflicts are resolved through consensus confidence accumulation.

---

## 6. Cryptographic Primitives

Aion assumes adversaries may execute Shor's algorithm at scale.

### 6.1 Hash Function

- SHA3-512
- 512-bit output
- ~256-bit effective security under Grover reduction

Used for:

- Transaction IDs
- Address derivation
- Integrity verification

---

### 6.2 Digital Signatures

- CRYSTALS-Dilithium (highest NIST security category)
- No ECDSA
- No EdDSA
- No hybrid mode

All signatures must be post-quantum secure.

---

## 7. Address Model

Addresses are defined as:
