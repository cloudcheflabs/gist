-----

## title: “Introducing Chango 3 — The Agentic AI Data Platform Where Agents and Data Live Together”
description: “Chango 3 unites Cloud Chef Labs’ own products — Ontul, NeorunBase, ShannonStore, ItdaStream, and Mium — with proven open source engines in a single self-hosted control plane.”
date: 2026-05-22
tags: [Chango, Agentic AI, Data Platform, Ontul, NeorunBase, Mium, RAG, Lakehouse, Sovereign AI, Open Source]
author: Cloud Chef Labs

# Introducing Chango 3 — The Data Platform for the Age of Agents

Today we are launching **Chango 3**. This isn’t just the next version of a data lakehouse. It’s a leap toward a **self-hosted Agentic AI Data Platform — where agents and data live together inside the same platform.**

Until now, most organizations have treated AI as something you “bolt on” after the data platform is built. The vector DB lived somewhere else, the agent runtime somewhere else again, and the permission models never quite lined up. Chango 3 inverts that, bringing object storage, streaming, a data engine, a multimodal database, and an AI agent together inside **one self-hosted control plane.**

> **Agentic. Sovereign. One Control Plane.**
> Agents, multimodal data, engines, and storage — in a single self-hosted platform.

-----

## Why an Agentic AI Data Platform?

As LLMs and agents move into real workloads, the demands on a data platform have changed fundamentally.

- Agents need to reach data **safely, within permission boundaries.**
- Vector similarity alone isn’t enough — retrieval increasingly needs **full-text and graph traversal together.**
- **Sovereign** requirements, where data must never leave the organization’s boundary, matter more than ever.
- And through all of this, your **proven data engines** — SQL, batch, streaming — must keep working.

Chango 3 is designed to meet these demands in a single platform by uniting the products Cloud Chef Labs builds itself with proven open source.

-----

## The Cloud Chef Labs Products Behind Chango 3

Chango 3 isn’t an abstract stack of “layers” — it’s a combination of real products, each of which also stands on its own. Here is each one, described precisely.

### Ontul — Unified Distributed Data Engine

A distributed data engine that unifies **batch processing, stream processing, and interactive SQL in a single engine.** One Ontul cluster handles every workload — no separate batch, streaming, or query clusters.

- **Arrow-native execution**: all data is processed in the Apache Arrow columnar format, with vectorized operations and zero-copy that remove serialization overhead.
- **Interactive SQL**: JDBC connectivity over Arrow Flight SQL (DBeaver, DataGrip) and multi-catalog federated queries.
- **Flink-style streaming**: continuous processing that handles events the moment they arrive — not micro-batches — with TUMBLING, SLIDING, and SESSION windows.
- **Apache Iceberg v2 native**: distributed INSERT/CTAS, MOR-based DELETE/UPDATE/MERGE, hidden partitioning, schema evolution, and time travel in one engine.
- **Exactly-once guarantees**: master-coordinated barrier checkpoints ensure exactly-once delivery to transactional sinks like Iceberg, JDBC, NeorunBase, and Kafka.
- **Security**: AES-256-GCM envelope encryption, built-in KMS, and IAM policies at catalog, table, column, and row level.

### NeorunBase — Multimodal Lakebase

A **PostgreSQL-compatible** distributed OLTP database that combines vector DB, full-text, graph DB, and spatial data into **one ACID SQL engine** — a multimodal Lakebase. Instead of running Pinecone, Elasticsearch, and Neo4j separately, it combines them inside a single SQL statement.

- **PostgreSQL wire-compatible**: psql, JDBC, pgAdmin, LangChain, and pgvector clients connect with no code changes.
- **Vector database**: a pgvector-compatible VECTOR type with a distributed HNSW ANN index. Because transactional data, metadata, and embeddings live in the same ACID transaction, it’s ready to use as a RAG backend.
- **Full-text (Lucene BM25)**: PostgreSQL FTS-compatible syntax with multilingual analyzers. A hybrid search combining vector ANN is expressed in a single `HYBRID_SEARCH(...)` line.
- **Graph DB + analytics**: BFS traversal, PageRank / Personalized PageRank, and multi-hop reachability checks as SQL TVFs — so a **Graph RAG** pattern that re-ranks hybrid-search results by graph score is expressed in one SELECT.
- **Iceberg CDC**: OLTP table changes are automatically CDC-synced to Iceberg/Parquet, so Ontul, Trino, and Spark analyze the same data in SQL.

### ShannonStore — S3-Compatible Object Storage

A **high-performance, S3-compatible object storage** system with high availability and elastic scalability. It fully supports the S3 API — CRUD, multipart upload, bucket versioning — so existing S3 tools and SDKs integrate as-is.

- **Erasure coding**: high durability at far lower cost than replication, with automatic recovery on disk failure.
- **Zero-downtime horizontal scaling**: add or remove nodes and it scales instantly, without interruption.
- **Open ecosystem integration**: works with Iceberg, Trino, Spark, and Flink, and integrates directly with Ontul, NeorunBase, and ItdaStream.

### ItdaStream — Kafka-Compatible Streaming

A next-generation streaming platform that is **fully Kafka-protocol compatible** while separating compute from storage. Existing Kafka clients work with no code changes.

- **S3 tiered storage**: recent data on memory/SSD (hot tier), older data on S3 (cloud tier) — removing local-disk limits and enabling **unlimited retention.**
- **Cost savings**: reduces reliance on expensive NVMe/SSD, cutting storage cost by up to 80% versus traditional Kafka.
- **Scale without re-replication**: add or remove broker nodes instantly, with no large-scale re-replication overhead.

### Mium — The AI Agent Platform for Ontul

**Mium is an AI agent platform that analyzes Ontul data in natural language.** When a user asks a question in plain language, the AI agent automatically generates Ontul SQL queries — enabling advanced analysis like aggregation, trends, filtering, and joins without SQL expertise.

- **Today it supports Ontul only.** Ontul is Mium’s single built-in tool, connected via Arrow Flight SQL using each user’s own credentials against Ontul catalogs. (The Tool SPI is designed to be extensible, so additional CCL stack tools may be added in the future.)
- **Multi-agent orchestration**: a Planner/Executor/Specialist pattern handles complex analysis requests step by step, with a pluggable LLM backend supporting multiple providers.
- **Sovereign AI**: all state — IAM, credentials, conversation history, encryption keys — is kept in built-in storage, and it runs entirely on your own infrastructure with no external database.
- **Per-user credential isolation**: each user connects with their own access, and credentials are kept under KMS envelope encryption.

-----

## Proven Open Source, Operated Alongside

Chango 3 doesn’t reinvent the wheel. On top of an Iceberg-based lakehouse, it operates proven open source engines with the same operational experience.

- **Trino** — distributed SQL. The `chango-trino-authz` plugin enforces Ontul IAM policies on every query — table allow/deny, column masking, row filters.
- **Apache Spark** — standalone mode. The `chango-spark-authz` plugin authorizes Spark SQL queries against Ontul IAM at the table level.
- **Apache Flink** — standalone session cluster. The `chango-flink-authz` plugin wires the same Ontul authorization model into Flink SQL.
- **Trino Gateway** — the HTTP gateway in front of one or more Trino clusters, for blue/green upgrades and routing.
- **Apache Polaris** — the Iceberg REST catalog used by Spark, Trino, and Flink to discover and write Iceberg tables, backed by a PostgreSQL metastore.
- **Apache Kafka / Schema Registry** — bundled options for teams that want the upstream Kafka broker (or replace with ItdaStream)
- **PostgreSQL** — managed as a first-class provisioner and reused for the Polaris metastore, Trino resource groups, and the NeorunBase coordinator catalog.

-----

## Use Cases

### AI-Native Analytics

A business user who doesn’t know SQL asks Mium a question in natural language; the agent generates Ontul SQL automatically, analyzes the data, and returns **trustworthy insight.** You can talk to your data without building a dashboard first.

### Graph RAG & Agent Retrieval

Inside NeorunBase, a hard filter (SQL WHERE) + semantic/keyword hybrid search + graph-based re-ranking all run in **one SELECT.** Rather than gluing together four systems (Postgres, Pinecone, Elasticsearch, Neo4j), production RAG and agent retrieval run in a single database.

### Sovereign Data Platform

Run the entire stack — data engine, multimodal DB, storage, and AI agent — **on-premises or in your own cloud account.** Because data never crosses the boundary, it fits environments with strict regulatory and security requirements.

### Unifying OLTP and Analytics

NeorunBase’s OLTP transactions are automatically CDC-synced to Iceberg, so Ontul, Trino, and Spark analyze the same data with no separate ETL or CDC pipeline.

### Real-Time Data Pipelines

Ingest events from ItdaStream (or Kafka), process them in Ontul, and load them into Iceberg tables — a real-time ETL pipeline built on one platform.

-----

## Getting Started

Chango 3 can be installed in online/public environments as well as offline/disconnected (air-gapped) environments.

- **Product overview**: <https://www.cloudchef-labs.com/en/data-platform/>
- **Documentation**: <https://cloudcheflabs.github.io/chango-docs>
- **Individual product docs**: [Ontul](https://cloudcheflabs.github.io/ontul-docs) · [NeorunBase](https://cloudcheflabs.github.io/neorunbase-docs) · [ShannonStore](https://cloudcheflabs.github.io/shannonstore-docs) · [ItdaStream](https://cloudcheflabs.github.io/itdastream-docs) · [Mium](https://cloudcheflabs.github.io/mium-docs)
- **Contact us**: <https://www.cloudchef-labs.com/en/contact/>

Agents, multimodal data, proven engines, and storage — in a single self-hosted platform. Start building the data platform for the age of agents with **Chango 3**.

-----

*© 2026 Cloud Chef Labs, Inc. All rights reserved.*