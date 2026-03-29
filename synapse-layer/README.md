# 🧠 Synapse Layer

> *"Giving Agents a Past. Giving Models a Soul."*

**Synapse Layer** is the universal memory layer for AI agents — persistent, private, model-agnostic, and open-source.

## Overview

Synapse Layer provides **zero-knowledge persistent memory** for AI agents. All data is encrypted client-side with AES-256-GCM before leaving the device. The server **never** sees plaintext data.

### Core Features

- **Zero-Knowledge Context™** — AES-256-GCM encryption with PBKDF2 key derivation (210k iterations)
- **Neural Handover™** — HMAC-SHA256 signed context transfer between AI models
- **Consensus Engine™** — Trust Quotient formula resolves contradictory memories
- **Semantic Recall** — pgvector HNSW index with gte-small embeddings (384-dim)
- **MCP-Native** — Works with any MCP-compatible agent (Claude, Cursor, etc.)
- **LGPD/GDPR Compliant** — Soft-delete with audit trails

## Installation

```bash
# npm global install
npm install -g synapse-layer

# Or use directly with npx
npx synapse-layer remember "User prefers dark mode" --user <uuid>
```

## Commands

### `synapse remember`
Store a memory with zero-knowledge AES-256-GCM encryption.
```bash
synapse remember "User prefers dark mode" --user <uuid> --importance 0.9
```

### `synapse recall`
Semantic recall via pgvector HNSW cosine similarity.
```bash
synapse recall "What are user preferences?" --user <uuid> --limit 5
```

### `synapse handover`
Neural Handover™ — transfer full context between AI models with HMAC-SHA256 signing.
```bash
synapse handover --to gpt-4o --user <uuid>
```

### `synapse import`
Import a handover package to restore context in a new model.
```bash
synapse import --blob <base64> --user <uuid>
```

### `synapse status`
Check memory vault health, Trust Score, and active memory count.
```bash
synapse status --user <uuid>
```

### `synapse forget`
Soft-delete a memory (LGPD/GDPR compliant — encrypted data retained for audit).
```bash
synapse forget "outdated preference" --user <uuid>
```

## MCP Configuration

Add to your MCP client config:

```json
{
  "mcpServers": {
    "synapse-layer": {
      "url": "https://rbeycxzizrrdmxpilepc.supabase.co/functions/v1/mcp-server"
    }
  }
}
```

## Trust Quotient Formula

```
TQ = importance(35%) + confidence(25%) + stability(20%) + credit(10%) + recency(10%)
```

Source type multipliers: Finance (1.5x) · Health (1.2x) · Legal (1.1x)

## Links

- 🌐 **Homepage**: [synapselayer.org](https://synapselayer.org)
- 📦 **npm**: [synapse-layer](https://www.npmjs.com/package/synapse-layer)
- 🔧 **GitHub**: [SynapseLayer/essencial](https://github.com/SynapseLayer/essencial)
- 🏭 **Smithery**: [synapselayer/essencial](https://smithery.ai/servers/synapselayer/essencial)

## License

Apache 2.0 — Built in São Paulo 🇧🇷
