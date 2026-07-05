---
name: ddia
description: >-
  Apply Martin Kleppmann's Designing Data-Intensive Applications principles
  when choosing databases, designing data models, replication, partitioning,
  transactions, streams, or backend scalability. Use for data architecture,
  consistency, caching, queues, batch/stream processing, or when the user
  mentions DDIA, data-intensive, replication, or CAP.
---

# DDIA — Agent Skill (Designing Data-Intensive Applications)

Rules from *Designing Data-Intensive Applications* (Martin Kleppmann, O'Reilly 2017). **Data is usually the hard part of backend web apps — choose tools and trade-offs explicitly.**

Pair with **web-api-design** at the boundary; **domain-driven-design** for domain models; **software-architecture** for style selection.

## When to apply

- Choosing database, cache, queue, search, or stream tech
- Data modeling (relational vs document vs graph)
- Replication, sharding, consistency, transactions
- Batch vs stream processing pipelines
- User says: DDIA, data-intensive, replication, partition, consistency, event log

---

## Core laws

1. **Reliability, scalability, maintainability** — every data decision serves these three.
2. **There is no free lunch** — latency, consistency, availability, cost — pick trade-offs.
3. **Know your access patterns** — schema and storage follow queries, not the reverse.
4. **Assume faults** — hardware, software, humans, networks fail.
5. **Simplest thing that works** — one database until proven insufficient.
6. **Evolve schemas and formats** — forward/backward compatibility is architecture.

---

## Agent workflow

```
1. LOAD        — read/write ratio, volume, latency, growth
2. MODEL       — relational / document / graph for the domain
3. STORE       — OLTP engine, indexes, analytics path if needed
4. DISTRIBUTE  — replicate? partition? leader/follower?
5. CONSISTENCY — transactions vs eventual; isolation level
6. DERIVE      — caches, search indexes, streams off source of truth
7. OPERATE     — backups, monitoring, human error guards
```

---

## Part I — Foundations

### Reliable, scalable, maintainable (Ch 1)

**Reliability:** faults happen — design to survive (redundancy, retries, graceful degradation).

**Scalability:** define load (QPS, users, data size) and latency targets; measure percentiles (p99), not averages.

**Maintainability:**
- **Operability** — easy for ops to run
- **Simplicity** — manage complexity (small number of composable ideas)
- **Evolvability** — easy to change as requirements shift

### Data models (Ch 2)

| Model | Good for |
|-------|----------|
| **Relational** | Joins, constraints, many-to-many, reporting |
| **Document** | Aggregate-shaped data, schema flexibility, one-to-few |
| **Graph** | Many-to-many relationships, connected data, traversals |

**Mismatch smell:** ORM fighting schema → wrong model or wrong boundaries.

**Query languages:** declarative (SQL) vs imperative; MapReduce for large batch.

### Storage engines (Ch 3)

- **OLTP** — row-oriented, B-trees or LSM-trees, indexes
- **OLAP** — column storage, warehouses, data cubes
- **Don't mix** transactional and heavy analytics on same DB without plan

### Encoding & evolution (Ch 4)

- JSON, Avro, Protobuf — schema evolution matters for services and streams
- **Backward/forward compatibility** when rolling deploys

---

## Part II — Distributed data

### Replication (Ch 5)

- **Leader/follower** — common default; sync vs async replication
- **Replication lag** problems: read-your-writes, monotonic reads, consistent prefix
- **Multi-leader** — multi-datacenter; write conflicts need resolution
- **Leaderless** — quorums (Dynamo-style); sloppy quorum, hinted handoff

### Partitioning (Ch 6)

- **By key range** vs **hash of key** — skew/hot spots
- **Secondary indexes** — global vs document-local
- **Rebalancing** when cluster grows

### Transactions (Ch 7)

**ACID** — understand what your DB actually guarantees.

**Isolation levels:** Read Committed → Snapshot Isolation → Serializable (2PL, SSI).

**Distributed transactions** — 2PC is fragile; prefer **sagas** / idempotent ops for cross-service (see domain-driven-design).

### Distributed systems reality (Ch 8–9)

**Fallacies:** network not reliable, latency not zero, clocks lie, partial failures normal.

**Consensus** — for leader election, atomic commit when truly needed.

**Don't:** assume synchronous cross-service transactions fix everything.

---

## Part III — Derived data

### Batch (Ch 10) vs Stream (Ch 11)

| | Batch | Stream |
|---|-------|--------|
| Latency | Hours/minutes | Seconds/ms |
| Tooling | MapReduce, Spark | Kafka, Flink |
| Use | Reports, rebuild indexes | Notifications, sync, CDC |

**Change data capture (CDC)** — log as source of truth; derive views.

### Future of data systems (Ch 12)

- **Unbundled databases** — specialized tools + derived data keeps sync
- **Eventual consistency** with **end-to-end correctness** (constraints, verification)
- Design apps around **dataflow**, not only request/response

---

## Decision heuristics

| Question | Heuristic |
|----------|-----------|
| One service, moderate load | PostgreSQL (or similar) until proven otherwise |
| Need flexible nested docs | Document DB or JSON column + careful indexing |
| Heavy graph queries | Graph DB or adjacency in SQL with limits |
| Read-heavy, tolerate stale | Cache (Redis) + TTL; know invalidation |
| Full-text search | Dedicated index (Elasticsearch, etc.) — derived from source |
| Cross-service updates | Saga + idempotency, not 2PC across microservices |
| Analytics | Separate OLAP path; don't crush OLTP |
| Real-time fan-out | Stream/log + consumers |

---

## Smells to flag

| Smell | Fix |
|-------|-----|
| One DB for everything including BI | Split OLTP/OLAP |
| No replication on prod data | Replicas + backup tested |
| Ignoring replication lag in UX | Read-your-writes strategy |
| Hot partition key | Reshard, salting |
| Serializable everywhere "just in case" | Match isolation to need |
| Cache without invalidation plan | TTL, event-driven invalidation |
| Dual writes to DB and search | CDC / outbox / single writer |
| Distributed monolith + shared DB | Partition data with services |

---

## Review output format

```markdown
## Data profile
[Load, size, read/write, latency needs]

## Model & storage
[Choice + why]

## Distribution
[Replication, partitioning, consistency]

## Failure modes
[What breaks; mitigations]

## Derived systems
[Cache, search, streams]

## Trade-offs accepted
[What we give up and why]
```

---

## Source

Martin Kleppmann, *Designing Data-Intensive Applications* (O'Reilly, 2017).
