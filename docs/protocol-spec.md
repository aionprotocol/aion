Aion Protocol Specification v0.2

Post-Quantum Conservative Architecture

1. Threat Model

Aion assumes the existence of adversaries with:

Large-scale quantum computational capability

Real-time network surveillance

Ability to attempt key recovery immediately after public key exposure

Significant computational and financial resources

All protocol decisions prioritize long-term cryptographic resilience.

2. Ledger Model

The ledger is a Directed Acyclic Graph (DAG).

Definition

Each vertex represents a transaction.

Each edge represents a parent reference.

No cycles are allowed.

No global linear ordering exists.

Each transaction must reference at least two valid parent transactions.

3. Transaction Structure

A transaction consists of:

Transaction = {
    header,
    body
}
3.1 Header
header = {
    parents: [tx_id, tx_id, ...],
    timestamp: u64
}

Constraints:

At least two parent references.

Parents must exist and be valid.

Timestamp is informational only (non-consensus).

3.2 Body
body = {
    inputs: [Input],
    outputs: [Output]
}

The body is the signed portion.

4. Input Structure
Input = {
    referenced_output_id: Hash512,
    public_key: DilithiumPublicKey,
    signature: DilithiumSignature
}

Rules:

referenced_output_id must exist in current UTXO set.

signature signs the serialized body.

public_key must satisfy:

SHA3-512(public_key) == locking_hash of referenced output

5. Output Structure
Output = {
    amount: u128,
    locking_hash: Hash512
}

Where:

locking_hash = SHA3-512(public_key)

Rules:

Amount must be positive.

Total input amount ≥ total output amount.

Difference constitutes transaction fee.

6. Transaction ID
tx_id = SHA3-512(serialize(header || body))

512-bit identifier.

Collision-resistant under quantum-reduced security assumptions.

7. UTXO Set

The UTXO set maintains all unspent outputs.

An output is removed when:

A valid transaction referencing it accumulates sufficient confidence.

Double-spend resolution is handled probabilistically via consensus convergence.

8. Consensus Model

Aion uses metastable probabilistic consensus:

Validator samples k random peers.

Requests preferred transaction in case of conflict.

Updates local preference if supermajority observed.

Repeats sampling until convergence threshold met.

No mining.
No leader.
No block production.

9. Confidence Score

Each transaction maintains:

confidence_score ∈ [0, 1]

Confidence increases when:

Sampling rounds prefer the transaction.

Descendants reinforce its validity.

Network convergence stabilizes.

Finality condition:

confidence_score ≥ finality_threshold

Threshold defined via network parameters.

10. Cryptographic Primitives
Hash Function

SHA3-512

Used for:

Transaction IDs

Address derivation

Integrity checks

Digital Signatures

CRYSTALS-Dilithium (highest NIST category)

No classical signatures supported

No hybrid fallback

11. Address Model

Addresses are defined as:

address = SHA3-512(public_key)

Public keys are only revealed at spend time.

Each output must use a fresh public key.

Key reuse is strongly discouraged and may be rejected by future consensus rules.

12. Security Properties

Aion aims to provide:

Post-quantum signature security

256-bit effective hash security under Grover reduction

Minimal key exposure window

Resistance to leader-targeted attacks

Resistance to block reorganization attacks (no blocks)

13. Economic Model (High-Level)

Low perpetual inflation

Continuous validator rewards

Near-zero dynamic anti-spam fees

Contribution-weighted incentive model

Formal economic equations defined separately.

Status

This document defines the formal post-quantum conservative architecture of Aion.

Further specifications will define:

Binary serialization format

Networking protocol

Validator reward formulas

Governance parameters
