# Aion Transaction Lifecycle v0.1

## 1. Overview

This document describes the lifecycle of a transaction
from reception to acceptance.

The process is deterministic and consensus-driven.

---

## 2. Genesis Handling

If the DAG is empty:

- the first transaction received is treated as genesis
- parent validation is skipped
- UTXO outputs are inserted directly

Genesis must:

- contain no inputs
- contain no parents
- contain at least one output

Genesis initializes the ledger state.

---

## 3. Transaction Flow

All non-genesis transactions follow the lifecycle below.

---

### Step 1 — Reception

Transaction enters the node via:

- P2P network
- Local API

---

### Step 2 — Structural Validation

Checks:

- serialization validity
- field integrity
- minimum parent count

Non-genesis rule:

- must reference at least two parents

---

### Step 3 — Hash Integrity

Recompute:

tx_id = SHA3-512(serialized_transaction)


Reject if mismatch.

---

### Step 4 — Cryptographic Validation

Verify:

- Dilithium signature
- public key consistency
- locking hash correctness

---

### Step 5 — DAG Validation

Verify:

- parents exist
- no cyclic dependency

Reject if any parent is missing.

---

### Step 6 — UTXO Validation

Verify:

- referenced outputs exist
- outputs are unspent

Reject if:

- double spend
- missing output

---

### Step 7 — Mempool Admission

Transaction enters pending pool.

Not yet canonical.

---

### Step 8 — Consensus Sampling

Node samples peers to determine preference.

Conflicts are evaluated probabilistically.

---

### Step 9 — Confidence Accumulation

Each transaction receives a:


Reject if mismatch.

---

### Step 4 — Cryptographic Validation

Verify:

- Dilithium signature
- public key consistency
- locking hash correctness

---

### Step 5 — DAG Validation

Verify:

- parents exist
- no cyclic dependency

Reject if any parent is missing.

---

### Step 6 — UTXO Validation

Verify:

- referenced outputs exist
- outputs are unspent

Reject if:

- double spend
- missing output

---

### Step 7 — Mempool Admission

Transaction enters pending pool.

Not yet canonical.

---

### Step 8 — Consensus Sampling

Node samples peers to determine preference.

Conflicts are evaluated probabilistically.

---

### Step 9 — Confidence Accumulation

Each transaction receives a:

confidence_score


based on repeated sampling.

---

### Step 10 — Acceptance

When:

confidence_score >= finality_threshold


transaction becomes accepted.

---

### Step 11 — State Transition

Accepted transactions:

- consume inputs
- create new UTXOs

Rejected conflicts:

- remain in DAG
- are ignored economically

---

## 4. Reorg-Free Property

Aion does not rely on chain reorganization.

All transactions remain in the DAG.

Consensus defines economic relevance, not structural existence.
