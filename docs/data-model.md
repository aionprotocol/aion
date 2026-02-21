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
