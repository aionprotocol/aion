Aion Protocol Specification v0.1 (Draft)
1. Overview

Aion is a blockless distributed ledger protocol based on:

Directed Acyclic Graph (DAG) transaction structure

Probabilistic consensus

Confidence-based finality

Emergent logical time

Quantum-resilient cryptographic primitives

This document defines the foundational mechanics of the protocol.

2. Transaction Model

Each transaction in Aion contains:

Transaction ID (hash)

Parent references (2+ previous transactions)

Sender public key

Signature

Timestamp (local, non-authoritative)

Payload

Fee

Nonce

Transactions must reference at least two previous valid transactions to maintain DAG connectivity.

3. DAG Structure

The ledger is represented as a Directed Acyclic Graph:

Nodes = Transactions

Edges = References to previous transactions

No global linear ordering exists.

Instead:

Partial ordering emerges through references

Causal depth defines relative temporal structure

4. Confidence Score

Each transaction accumulates a confidence score over time.

Confidence increases when:

Validators sample and prefer it

Descendant transactions indirectly reinforce it

Network convergence increases

Finality is defined when:

confidence_score ≥ threshold

Threshold is parameterized and adjustable by protocol governance.

5. Validator Sampling

Consensus operates via repeated stochastic sampling:

A validator queries a random subset of peers.

It asks which transaction they prefer in case of conflict.

Repeated rounds produce metastable convergence.

This mechanism ensures probabilistic finality without mining or leader election.

6. Emergent Time

Time is not block-based.

Logical time is derived from:

DAG depth

Validator agreement density

Transaction propagation patterns

Temporal ordering is structural, not imposed.

7. Economic Model (High-Level)

Continuous validator rewards

Low perpetual inflation (1–2% annual target)

Dynamic anti-spam fees

Incentives proportional to network contribution

Detailed economic formulas will be defined in a separate document.

8. Cryptography

Planned primitives:

Post-quantum digital signatures

Modern hash functions resistant to Grover-style attacks

Secure peer authentication

Exact cryptographic suite to be finalized in crypto specification.

Status

This is an early draft specification intended to formalize core protocol behavior.
