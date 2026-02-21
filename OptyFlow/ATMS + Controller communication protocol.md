## Purpose

This document is a **short, opinionated starter** for asynchronous brainstorming ahead of our meeting. It is not a proposal or decision document. The goal is to align on constraints, enumerate viable options, and highlight trade‑offs so discussion is focused and productive.

## Context & Problem Statement

We need to define a **communication protocol, API, and data format** for communication between:
- **Traffic signal controllers (edge devices)**
- **ATMS (central server)**

The solution must work across a wide range of deployments and lifecycles (10–20 years is realistic for traffic hardware).

### Core Requirements

**1. Performance & Efficiency**
- Must work on low‑bandwidth / high‑latency links
- Compact messages preferred
- Efficient for high‑frequency telemetry

**2. Security**
- Industry‑standard encryption (TLS)
- Strong authentication and authorization
- Device identity must be first‑class

**3. API & Schema Governance**
- Canonical, versioned, shareable APIs
- Machine‑readable schemas
- Backwards compatibility strategy

**4. Deployment Flexibility**
- Closed loop (no public internet, on‑prem only)
- Open loop (cloud, hybrid, remote access)
- Should not require always‑on connectivity

## Explicit Assumptions

- **Transport layer: TCP**    
    - Reliability is required
    - Security tooling is mature (TLS, mTLS)
    - UDP‑based protocols can be revisited later if latency becomes a hard constraint

## Communication Protocol Options

Below is a **non‑exhaustive list** of candidate protocols with trade‑offs.

### 1. Raw TCP (Custom Protocol)

**Pros**
- Full duplex
- Minimal overhead
- Maximum control

**Cons**
- We must design everything: framing, retries, versioning, auth, tooling. Reinvents existing standards and tooling
- High long‑term maintenance cost
- Fully proprietary

### 2. WebSocket (over TLS)

**Pros**
- Full duplex
- Low overhead
- Widely supported (including browsers)
- Easy to run through firewalls

**Cons**
- Still requires custom API definition
- Auth is not standardized (usually header or subprotocol hacks)
- No built‑in schema or versioning story

### 3. MQTT

**Pros**
- Designed for constrained devices
- Low overhead
- Built‑in pub/sub semantics
- Standard auth patterns
- Offline buffering and QoS levels

**Cons**
- Requires a broker
- Request/response flows require custom "hacking"
- Harder to reason about for strict APIs

### 4. gRPC

**Pros**
- Strongly typed APIs
- First‑class schema (Protobuf)
- Streaming supported (bi‑directional)
- Excellent tooling and code generation
- Clear versioning and evolution model

**Cons**
- Higher overhead than raw TCP/WebSocket/MQTT
- More complex stack
- Some proxies/firewalls still struggle with HTTP/2

### 5. HTTP (REST)

**Pros**
- Universal support
- OpenAPI ecosystem
- Well understood by integrators

**Cons**
- Unidirectional
- Verbose
- Inefficient for frequent updates

### 7. AMQP (e.g. RabbitMQ)

**Pros**
- Strong delivery guarantees
- Mature ecosystem
- Rich routing

**Cons**
- Heavy for edge devices
- Broker required

## Data Format Options

### 1. JSON

**Pros**
- Universal
- Human‑readable

**Cons**
- Large payloads
- No native schema enforcement
- Inefficient for high‑frequency messages

### 2. Protobuf

**Pros**
- Compact binary encoding
- Strong schema + evolution model
- Excellent code generation
- Language‑agnostic

**Cons**
- Requires tooling

### 3. FlatBuffers / Cap’n Proto

**Pros**
- Zero‑copy parsing
- Extremely fast

**Cons**
- Smaller ecosystem
- More complex evolution story

### 4. CBOR / MessagePack

**Pros**
- Binary JSON‑like formats
- Compact
- Easier migration from JSON

**Cons**
- Schema is optional (can become messy)

### 5. XML

**Pros**
- Strong schema support (XSD)
- Mature tooling

**Cons**
- Extremely verbose
- Poor fit for constrained links

## Missing Dimensions (for Discussion)

These are **not addressed above** but should influence decisions:
- Device provisioning & bootstrap (first connection, cert issuance)
- Schema evolution & backward compatibility
- Offline buffering & replay
- Idempotency & deduplication
- Clock sync & ordering guarantees   
