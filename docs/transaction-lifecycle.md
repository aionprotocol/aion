Aion Transaction Lifecycle v0.1
1. Overview

This document describes the complete lifecycle of a transaction inside an Aion node, from initial reception to probabilistic finality.

Aion operates under:

DAG-based structure

UTXO state model

Post-quantum signatures

Probabilistic metastable consensus

There is no global ordering.
Finality emerges from accumulated network confidence.

2. Transaction Entry Points

A transaction may enter a node through:

P2P Network (Gossip)

Local API Submission (Wallet)

In both cases, the serialized transaction is first processed identically.

3. Phase 1 — Structural Validation

Performed immediately upon reception.

3.1 Deserialization

Decode using Borsh.

Reject if malformed.

Reject if version unsupported.

3.2 Basic Structural Rules

Reject if:

parents.len() < 2

inputs.len() == 0

outputs.len() == 0

Transaction size exceeds maximum limit.

3.3 Hash Integrity

Compute:

tx_id = SHA3-512(serialized_transaction)

Ensure:

tx_id matches internal consistency.

No duplicate tx_id already confirmed.

4. Phase 2 — Cryptographic Validation

Handled by Crypto module.

For each input:

Verify Dilithium signature.

Verify domain separation tag.

Ensure locking_hash == SHA3-512(public_key).

Reject if any signature fails.

5. Phase 3 — DAG Validation

Handled by DAG module.

Ensure all parent transactions exist.

Ensure no cyclic reference.

Ensure no invalid ancestry.

Attach transaction to in-memory DAG.

If parents missing:

Request from peers.

Mark transaction as pending.

6. Phase 4 — UTXO Validation

Handled by UTXO module.

For each input:

Ensure referenced output exists.

Ensure referenced output is unspent.

Detect conflicting transactions.

If double-spend conflict detected:

Register conflict set.

Allow consensus to resolve.

Transaction enters mempool if valid.

7. Phase 5 — Mempool Admission

Transaction is stored as:

State: Pending
Confidence Score: 0

Mempool may:

Prioritize

Rate limit

Reject spam

8. Phase 6 — Consensus Activation

Consensus module begins probabilistic sampling.

Process:

Sample random peers.

Query preferred transaction in conflict set.

Update local preference.

Increment confidence_score if stable.

Repeat asynchronously.

No global lockstep.

9. Phase 7 — Confidence Accumulation

Each transaction maintains:

confidence_score: u32

When:

confidence_score >= FINALITY_THRESHOLD

Transaction becomes:

State: Accepted

Threshold defined in protocol parameters.

10. Phase 8 — State Transition

When transaction becomes Accepted:

UTXO set updated:

Referenced outputs removed

New outputs inserted

Transaction marked immutable

Mempool entry cleared

11. Conflict Resolution

In case of conflicting transactions:

Both remain in DAG.

Consensus sampling determines preference.

Losing transaction remains orphaned.

UTXO updates only applied to winner.

12. Reorg-Free Model

Aion does not perform chain reorgs.

Instead:

DAG retains all transactions.

Confidence determines canonical state.

No rollback of accepted transactions once threshold reached.

13. Finality Properties

A transaction is considered:

Pending — validated but low confidence.

Preferred — locally winning conflict.

Accepted — crossed finality threshold.

Rejected — lost metastable consensus.

Finality is probabilistic but irreversible beyond threshold.

14. Security Considerations

The lifecycle assumes:

Malicious peers may exist.

Transactions may arrive out of order.

Quantum-capable adversaries may exist.

Network partitions may occur.

Safety depends on:

Deterministic validation.

Sufficient sampling rounds.

Honest majority assumption.
