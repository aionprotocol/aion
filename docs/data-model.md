Aion Data Model Specification v0.1
1. Serialization Standard

All Aion structures are serialized using:

Borsh (Binary Object Representation Serializer for Hashing)

Deterministic encoding

Little-endian byte order

Serialization must be identical across all implementations.

2. Fundamental Types
Type	Rust Type	Size
Hash	[u8; 64]	64 bytes
Amount	u64	8 bytes
Timestamp	u64	8 bytes
Version	u16	2 bytes
PublicKey	Vec<u8>	Dilithium-dependent
Signature	Vec<u8>	Dilithium-dependent
3. Transaction Structure
use borsh::{BorshSerialize, BorshDeserialize};

#[derive(BorshSerialize, BorshDeserialize)]
pub struct Transaction {
    pub version: u16,
    pub parents: Vec<[u8; 64]>,
    pub timestamp: u64,
    pub inputs: Vec<TxInput>,
    pub outputs: Vec<TxOutput>,
}

Rules:

parents.len() >= 2

inputs.len() >= 1

outputs.len() >= 1

4. Transaction ID

Transaction ID is defined as:

tx_id = SHA3-512(BorshSerialize(Transaction))

64 bytes

Immutable

Content-addressed identity

5. Transaction Input
#[derive(BorshSerialize, BorshDeserialize)]
pub struct TxInput {
    pub referenced_output: [u8; 64],
    pub public_key: Vec<u8>,
    pub signature: Vec<u8>,
}

Rules:

referenced_output must exist in UTXO set

public_key must match output locking hash

signature must verify over the transaction body

Public keys are revealed only at spend time

6. Transaction Output
#[derive(BorshSerialize, BorshDeserialize)]
pub struct TxOutput {
    pub amount: u64,
    pub locking_hash: [u8; 64],
}

Where:

locking_hash = SHA3-512(public_key)

Rules:

Fresh public key per output

Wallets must avoid reuse

Future versions may enforce non-reuse at consensus layer

7. UTXO Identifier

Each output is uniquely identified as:

output_id = SHA3-512(tx_id || output_index)

Where:

output_index is u32

Concatenation is raw byte concatenation

8. Signature Domain Separation

Signatures are computed over:

domain_tag || serialized_transaction_without_signatures

Where:

domain_tag = "AION_TX_V1"

This prevents cross-protocol replay attacks.

9. Size Expectations (Dilithium Level 5)

Approximate:

Public key: ~2592 bytes

Signature: ~4595 bytes

Aion assumes large transaction size as a tradeoff for quantum security.

Optimization strategies may be introduced later.

10. Versioning Strategy

version field allows protocol upgrades.

Nodes must reject unknown major versions.

Minor upgrades must preserve binary compatibility.
