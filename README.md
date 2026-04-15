# 🚀 Stratus Self-Service Streaming Platform (Azure)

![Architecture](architecture/architecture-diagram.png)

---

## 📌 Overview

**Stratus** is a **config-driven, self-service real-time streaming platform** designed to onboard CDC-based data pipelines at scale with **zero code changes**.

Built on **Azure Event Hubs (Kafka API), Databricks, and Delta Lake**, the platform processes high-volume event streams using a **multi-layer lakehouse architecture (Bronze / Silver / Gold)**.

### 🎯 Objectives

- Enable **rapid onboarding** of new data entities via configuration
- Ensure **idempotent, replay-safe processing**
- Support **scalable, low-latency streaming pipelines**
- Implement **production-grade data engineering patterns**

---

## 🏗️ System Architecture


+-------------------+
| CDC Producers |
| (Python Scripts) |
+---------+---------+
|
v
+---------------------------+
| Azure Event Hubs (Kafka) |
| Partitioned Event Stream |
+------------+--------------+
|
v
+---------------------------+
| Databricks Bronze Layer |
| Raw Append-Only Storage |
+------------+--------------+
|
v
+---------------------------+
| Databricks Silver Layer |
| Parse • Validate • Dedup |
+------------+--------------+
|
v
+---------------------------+
| Databricks Gold Layer |
| CDC Merge (I/U/D) |
+------------+--------------+
|
v
+---------------------------+
| Delta Lake (ADLS Gen2) |
| ACID • Scalable Storage |
+---------------------------+


---

## ⚙️ Technology Stack

| Layer        | Technology |
|-------------|-----------|
| Streaming   | Azure Event Hubs (Kafka API) |
| Compute     | Azure Databricks |
| Processing  | PySpark, Spark Structured Streaming |
| Storage     | ADLS Gen2 / Databricks Volumes |
| Table Format| Delta Lake |
| Language    | Python |

---

## 🔁 Data Pipeline Design

### 🟤 Bronze (Raw Ingestion)
- Ingests events directly from Event Hubs
- Append-only storage for immutability
- Minimal transformation
- Serves as **source of truth for replay**

### ⚪ Silver (Refined Layer)
- JSON parsing and schema enforcement
- Deduplication using event identifiers
- Data quality validation checks
- Handles malformed/invalid records

### 🟡 Gold (Serving Layer)
- Implements CDC merge logic:
  - `INSERT`
  - `UPDATE`
  - `DELETE`
- Uses Delta Lake `MERGE INTO` for upserts
- Produces **analytics-ready tables**

---

## 🧠 Core Design Principles

### 1. Config-Driven Onboarding
- Each entity defined via JSON configuration
- Schema, keys, and CDC logic externalized
- Enables **zero-code pipeline creation**

### 2. Idempotency
- Deduplication using event IDs + business keys
- Guarantees correctness under retries

### 3. Replayability
- Bronze layer stores raw immutable data
- Checkpointing allows recovery from failures
- Supports full pipeline reprocessing

### 4. Exactly-Once Semantics
- Spark Structured Streaming + checkpointing
- Delta Lake ACID guarantees prevent partial writes

---

## 📈 Scalability & Performance

- Event Hubs partitions → parallel ingestion
- Spark micro-batching → distributed compute
- Horizontal scaling via Databricks clusters

### Target Throughput
- Millions of events/day
- Low-latency ingestion (< seconds to minutes)
- Scalable across multiple entities

---

## 🛡️ Reliability & Fault Tolerance

- Checkpoint-based recovery
- Automatic retry on failure
- Bronze layer enables full replay
- ACID transactions via Delta Lake

---

## 🔄 Schema Evolution Strategy

- Schema enforced at Silver layer
- Controlled evolution via config updates
- Backward-compatible changes supported
- Invalid records isolated during validation

---

## 🧪 Implemented Entities

- `customer`
- `orders`

Each entity:
- Independently configured
- Supports full CDC lifecycle

---

## ⚡ Features

- ✅ Config-driven onboarding
- ✅ Real-time streaming ingestion
- ✅ Schema validation & parsing
- ✅ Deduplication & idempotency
- ✅ CDC merge (Insert/Update/Delete)
- ✅ Multi-entity orchestration
- ✅ Replay-safe architecture

---

## 🧩 Project Structure


├── notebooks/ # Databricks pipeline notebooks
│ ├── Bronze
│ ├── Silver
│ └── Gold
├── configs/ # Entity JSON configurations
├── producer/ # CDC event generators
├── architecture/ # Diagrams + execution proof
└── README.md


---

## 🧾 Proof of Execution

- ✔ Azure Event Hubs namespace configured
- ✔ Kafka endpoint integration validated
- ✔ Databricks pipelines executed end-to-end
- ✔ Bronze → Silver → Gold flow validated
- ✔ CDC merge tested (Insert/Update/Delete)

📷 Refer `/architecture` for screenshots

---

## 🧠 How to Onboard a New Entity

```bash
# Step 1: Add config
/configs/new_entity.json

# Step 2: Define schema + keys + CDC fields

# Step 3: Trigger pipeline
Run orchestration notebook

# Step 4: Pipeline executes end-to-end
``` id="onboard-code"

👉 No code changes required

---

## ⚠️ Known Gaps (Intentional for MVP)

- No DLQ (Dead Letter Queue)
- Limited observability (no monitoring stack)
- No schema registry integration
- Basic cost optimization

---

## 🔮 Future Enhancements

- Add DLQ for failed events
- Integrate monitoring (Azure Monitor / Prometheus)
- Schema Registry (Avro/JSON schema management)
- Auto-scaling optimization
- Data lineage tracking

---

## ✅ Outcome

Built a **production-aligned streaming data platform** demonstrating:

- Replay-safe pipelines  
- Idempotent processing  
- Multi-entity scalability  
- Lakehouse architecture best practices  

---

## 👤 Author

**Srikar Reddy Maddi**  
Senior Data Engineer | Streaming Systems | Lakehouse Architectures  
