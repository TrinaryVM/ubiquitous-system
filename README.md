# Canonical Mystery JSON

**Protocol-grade canonical serialization: JSON → Tai Xuan Jing tetragram glyphstreams**

[![Spec Version](https://img.shields.io/badge/spec-1.0.0-blue)](./CANONICAL-MYSTERY-JSON-SPEC.md)
[![Status](https://img.shields.io/badge/status-protocol--grade-green)](./CANONICAL-MYSTERY-JSON-SPEC.md)

This repository holds the **Canonical Mystery JSON Specification** — a lossless, deterministic transport layer that encodes JSON into Unicode tetragram glyphstreams (Tai Xuan Jing symbols, U+1D306–U+1D356).

It is suitable for blockchain payloads, content-addressable storage, consensus hashing, and any system that needs a canonical, human-readable symbolic encoding of structured data.

---

## Spec

| Document | Description |
|----------|-------------|
| [`CANONICAL-MYSTERY-JSON-SPEC.md`](./CANONICAL-MYSTERY-JSON-SPEC.md) | Full normative + illustrative specification (v1.0.0) |

---

## What it does

```
JSON Semantics
  → Canonical JSON serialization
  → Deflate compression (lossless)
  → Base-81 radix encoding
  → Unicode glyph projection (U+1D306 … U+1D356)
  → Tetragram glyphstream
```

**Properties**

- **Lossless** — perfect round-trip encode/decode
- **Canonical** — deterministic, hashable, consensus-safe
- **Compressed** — deflate before radix encoding (often 50–90% smaller for JSON)
- **Symbolic** — human-readable Unicode glyph transport
- **Protocol-grade** — designed for distributed systems and cryptography

---

## Quick mental model

1. Serialize JSON with stable key ordering (canonical / alphabetical).
2. Compress the bytes with deflate.
3. Treat compressed bytes as a big-endian base-256 integer; convert to base-81 digits (0–80).
4. Map each digit to one Tai Xuan Jing glyph.
5. Decode by inverting the map, decompressing, and parsing JSON.

Object key order is **not** preserved; semantic value is. That is intentional for canonicalization.

---

## Framing note

Byte-length-safe envelopes (flags, `orig_len`, optional `data_len`, left-pad MUST rule) are specified as **TVM-FRAME-v1**. Implementations that encode arbitrary byte payloads (especially those with leading zeros) should follow that framing model so round-trips remain length-correct after big-integer conversion.

The Mystery JSON spec documents the JSON↔glyphstream pipeline; framing is the envelope around the digit stream.

---

## Standards alignment

| Layer | Reference |
|-------|-----------|
| JSON | RFC 7159 |
| Canonical JSON (conceptually) | RFC 8785 |
| Deflate | RFC 1951 |
| Glyph alphabet | Unicode Tai Xuan Jing Symbols (U+1D306–U+1D356) |

---

## Intended use cases

- Canonical payload encoding for ledgers / smart-contract inputs
- Deterministic hashing and Merkle constructions
- Symbolic transport where binary blobs are undesirable
- Interoperable encode/decode across languages (spec-first)

---

## Repository layout

```
.
├── README.md
└── CANONICAL-MYSTERY-JSON-SPEC.md
```

This repo is **spec-only**. Reference implementations may live elsewhere; the document remains the source of truth for wire semantics and round-trip guarantees.

---

## Contributing

Treat changes to the spec as protocol changes:

1. Prefer additive, versioned extensions over silent breaks.
2. Preserve lossless round-trip and determinism.
3. Document any framing / flag / alphabet changes explicitly.
4. Add or update round-trip examples when behavior changes.

---

## License

*Add your license here (e.g. MIT / Apache-2.0 / CC-BY-4.0 for the prose).*

---

## Status

| Field | Value |
|-------|--------|
| Spec version | 1.0.0 |
| Status | Protocol-grade canonical serialization layer |
| Spec dates | 2026-01-15 (published) · 2026-01-18 (last updated in source doc) |

For algorithms, APIs, error model, compression tables, and security notes, see the full spec.
