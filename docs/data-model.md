# Aion Data Model Specification v0.1

## 1. Serialization

All structures are serialized using Borsh.

- Deterministic encoding
- Little-endian integers
- No floating point values

---

## 2. Base Types

- Hash: 64 bytes (SHA3-512 output)
- Amount: u64
- Timestamp: u64
- Version: u16

---

## 3. Transaction Structure

A transaction consists of:

- version
- parents
- timestamp
- inputs
- outputs

---

## 4. Transaction ID

The transaction ID is defined as:

tx_id = SHA3-512(BorshSerialize(Transaction))

Domain separation:

---

## 5. Inputs

Each input contains:

- referenced_output
- public_key
- signature

---

## 6. Outputs

Each output contains:

- amount
- locking_hash

---

## 7. Output Identification

The canonical output identifier is defined as:

output_id = SHA3-512(tx_id || output_index)

This identifier is used for:

- network transmission
- signature domain separation
- external referencing

Node implementations MAY internally represent UTXO references
using structured tuples such as:

(tx_id, output_index)


for performance and storage efficiency.

Such internal representations must map deterministically
to the canonical output_id.

---

## 8. Genesis Transaction Constraints

Genesis must satisfy:

- inputs = empty
- parents = empty
- outputs >= 1

Genesis outputs are considered valid UTXOs
without requiring input balance validation.

