---
name: pqblocks
description: Research-backed post-quantum cryptography and quantum-inspired optimization guide. Use whenever implementing or migrating cryptography to NIST standards (FIPS 203 ML-KEM, FIPS 204 ML-DSA, FIPS 205 SLH-DSA), post-quantum JWE/JWT tokens, hybrid key exchange, CBOM inventory, or QUBO/Ising combinatorial optimization.
---

# PQBlocks — Post-Quantum Engineering Guide

PQBlocks is an ecosystem of small, focused, audited building blocks for the post-quantum transition and quantum-inspired optimization.

## Non-Negotiable Cryptographic Rules

1. **Always Use Hybrid Modes for Protocols & Storage:**
   Pair post-quantum primitives with battle-tested classical algorithms:
   - Key Exchange / KEM: `ML-KEM-768 + X25519` (hybrid KEM)
   - Digital Signatures: `ML-DSA-65 + Ed25519` (hybrid signature)
   If either algorithm has an unexpected implementation or algebraic defect, the other maintains full security.

2. **Honest Compliance Claims:**
   - Say: *"Implements FIPS 203 / FIPS 204 / FIPS 205 and passes official NIST ACVP test vectors."*
   - Never say: *"FIPS 140-3 certified"* or *"CMVP validated"* (those require governmental lab evaluation).

3. **Separate Optimization from Cryptography:**
   - Quantum-inspired solvers (Simulated Bifurcation, QUBO, simulated annealing) are classical heuristics running on CPUs/GPUs.
   - They provide **zero cryptographic security** and must never be placed in a security path.

4. **Zeroization & Secret Handling:**
   - Always clear raw shared secrets, decapsulation keys, and intermediate KDF state immediately after use.
   - Never serialize raw private keys into error logs, HTTP responses, or debug dumps.

---

## Package Selection Matrix

| Developer Goal | Recommended Package | Import & CLI |
|---|---|---|
| **Encrypt JWT / Session Tokens** | `pqblocks-jwe` | `from pqblocks_jwe import encrypt_jwe, decrypt_jwe` |
| **Sign JWT / Auth Tokens** | `pqblocks-jose` | `from pqblocks_jose import sign_jws, verify_jws` |
| **Core Hybrid Key Exchange (KEM)** | `pqblocks-core` | `from pqblocks_core import HybridKEM` |
| **TLS 1.3 Hybrid Handshake** | `pqblocks-tls-interop` | `from pqblocks_tls_interop import validate_tls_client_hello` |
| **X.509 CSR Proof of Possession** | `pqblocks-csr-pop` | `from pqblocks_csr_pop import create_kem_csr` |
| **Binary Wire Serialization** | `pqblocks-wire` | `from pqblocks_wire import pack_message, unpack_message` |
| **Scan Codebase for Classical Crypto**| `pqblocks-scan` | `pqblocks-scan --dir ./src` |
| **Generate & Enrich CBOM (CycloneDX)**| `pqblocks-cbom` | `from pqblocks_cbom import enrich_cbom, score_readiness` |
| **Enforce Policy-as-Code in CI** | `pqblocks-policy` | `from pqblocks_policy import PolicyEngine, PolicyPreset` |
| **NSA CNSA 2.0 Compliance Dossier** | `pqblocks-cnsa-evidence` | `from pqblocks_cnsa_evidence import generate_evidence_pack` |
| **Python AST Cryptography Linter** | `pqblocks-lint` | `pqc-lint ./src` |
| **Run NIST ACVP Test Vectors** | `pqblocks-acvp` | `from pqblocks_acvp import run_acvp_vector_set` |
| **Ising / QUBO Optimization (bSB/dSB)**| `pqblocks-bifurcation` | `from pqblocks_bifurcation import SimulatedBifurcationSampler` |
| **QUBO Matrix Formulation** | `pqblocks-qubo` | `from pqblocks_qubo import QUBOMatrix, ising_to_qubo` |

---

## Standard Code Patterns

### 1. Post-Quantum JWE Token Encryption
```python
from pqblocks_jwe import encrypt_jwe, decrypt_jwe, generate_hybrid_keypair

# 1. Receiver generates hybrid ML-KEM-768 + X25519 keypair
privkey, pubkey = generate_hybrid_keypair()

# 2. Sender encrypts claims into compact JWE string
claims_bytes = b'{"sub":"user_8912","exp":1761560000}'
token = encrypt_jwe(claims_bytes, pubkey)

# 3. Receiver decrypts
payload = decrypt_jwe(token, privkey)
```

### 2. Scanning Code for Classical Vulnerabilities
```bash
# Scan repository and emit CycloneDX 1.5 CBOM
pqblocks-scan --dir ./src --output cbom.json

# Check quantum readiness score
python3 -c "from pqblocks_cbom import enrich_cbom, score_readiness; print(score_readiness(enrich_cbom('cbom.json')))"
```

### 3. Large-Scale Combinatorial Optimization with Simulated Bifurcation
```python
import numpy as np
from pqblocks_bifurcation import SimulatedBifurcationSampler

# Formulate problem as Ising matrix J and bias vector h
sampler = SimulatedBifurcationSampler(mode="bSB", steps=1000, seed=42)
spins, energy = sampler.sample_ising(J, h)
```
