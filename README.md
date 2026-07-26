# SCM Assistant Bot

A Retrieval-Augmented Generation (RAG) chatbot built in **Flowise** that answers questions about a supplier network using structured CSV data and a Supplier Governance Policy PDF. The chatbot uses semantic search over a Supabase vector database and generates accurate, context-aware responses using Groq LLMs.

## Live Chatbot

**Public URL:** *https://cloud.flowiseai.com/chatbot/996f38dc-99f6-405a-ae6c-76ed9580bfe2*

---

## Tech Stack

* **Framework:** Flowise
* **Vector Database:** Supabase (pgvector)
* **Embedding Model:** `sentence-transformers/all-MiniLM-L6-v2`
* **Embedding Dimensions:** 384
* **LLM:** `llama-3.1-8b-instant` (Groq)
* **Retrieval Strategy:** Similarity Search
* **Top-K Retrieval:** `k = 4`

---

## Data Sources

The chatbot indexes two different knowledge sources:

1. **Supplier CSV Dataset**
2. **Supplier Governance & Compliance Policy (PDF)**

Both sources are embedded and stored in Supabase for semantic retrieval.

---

## Chunking Configuration

### CSV (Preprocessed Text)

| Setting       |    Value | Reason                                                                                                                                                   |
| ------------- | -------: | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Chunk Size    | **1200** | Each supplier card is approximately 700–900 characters. A chunk size of 1200 keeps most supplier records intact without splitting important information. |
| Chunk Overlap |    **0** | Prevents duplicate supplier records across chunks and avoids redundant retrieval results.                                                                |

### PDF

| Setting       |    Value |
| ------------- | -------: |
| Chunk Size    | **1000** |
| Chunk Overlap |  **200** |

The PDF uses overlapping chunks to preserve context between policy sections while improving retrieval accuracy.

---

## Retrieval Pipeline

1. User submits a question.
2. The query is converted into embeddings using **sentence-transformers/all-MiniLM-L6-v2**.
3. Similarity search retrieves the **top 4** most relevant chunks from Supabase.
4. The retrieved context is passed to **Groq's llama-3.1-8b-instant**.
5. The LLM generates a grounded response based only on the retrieved knowledge.

---

# Sample Questions & Answers

## 1. Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer**

Based on the Supplier Governance & Compliance Policy (v3.2) and the provided supplier profile (SUP-107):

* There are **no Tier-3 suppliers** with an active disruption flag.
* SUP-107 is a **Tier-1 supplier**, so no Tier-3 response level applies.

The supplier has the following active disruption flags:

* Regulatory enforcement pending
* Port closure risk
* Currency volatility flag
* Port congestion risk
* Geopolitical tension flag
* Regulatory audit pending
* Tariff adjustment pending

If a Tier-2 or Tier-3 supplier had these disruptions, the policy defines:

* **Level 1 (Monitor):** Weekly supplier status updates
* **Level 2 (Manage):** Bi-weekly escalation calls, +30% safety stock, alternate supplier on standby
* **Level 3 (Activate):** Immediate CPO escalation, activate alternate supplier for at least 40% volume within 10 business days, +50% safety stock, and full RCA within 15 business days

Since SUP-107 is Tier-1, these response levels do not apply.

---

## 2. Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**Answer**

The chatbot identified the following suppliers as eligible:

* SUP-001 — Orrentek Precision Mfg
* SUP-023 — Tivoli Packaging Systems
* SUP-032 — Diversified Components
* SUP-048 — Berliner Pack GmbH

Overall, **25 Tier-1 suppliers** qualify for the Annual Volume Rebate Program.

---

## 3. Which region has the highest total PO value, and does it breach the concentration limit?

**Answer**

* **Region:** EMEA
* **Total PO Value:** **$2,677,370**

This does **not** breach the company concentration limit of **50%** of total organizational spending.

---

## 4. Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer**

Suppliers currently on the Supplier Watch List include:

* SUP-080 — Maghreb Castworks
* SUP-006 — Dongfeng Castings Co
* SUP-099 — Santiago Pack Corp

These suppliers have compliance scores below 60 and active policy flags.

The Supplier Watch List restricts these suppliers from being considered for future orders until they satisfy the required compliance standards.

---

## 5. Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer**

The chatbot identified:

* **Mechanical Components** as the category with the highest average defect rate.
* **SUP-047 — Rheinwerk Precision AG** has an average defect rate of **0.96%**.

Another supplier with a high defect rate:

* **SUP-029 — Kinki Precision Corp** recorded **1.00%** for Electronic Components.

The Tier-2 maximum permissible defect rate is **2.50%**, so these values remain **below the Tier-2 threshold**.



## Project Structure

```text
SCM Assistant/
│
├── dataset/
│   ├── suppliers.csv
│   └── Supplier_Governance_Policy.pdf
│
├── screenshots/
│
├── scm_assistant.json
├── README.md
└── .gitignore
```

---

## Author

**Jobin Jose**

AI Engineer | Python | Generative AI | RAG | LangChain | Flowise | Supabase | Groq
