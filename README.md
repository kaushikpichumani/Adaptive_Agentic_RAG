# Adaptive RAG using LangChain & LangGraph

This repository implements an **Adaptive Retrieval-Augmented Generation (Adaptive RAG)** system using **LangChain** and **LangGraph**.

Unlike static or even corrective RAG pipelines, **Adaptive RAG dynamically adapts its retrieval and generation strategy at runtime** based on:
- Document usefulness
- Answer quality
- Query supportability
- Availability of internal vs external knowledge

The workflow is modeled as a **stateful decision graph**, enabling conditional routing, retries, and fallback strategies.

---

## 🧠 What is Adaptive RAG?

Traditional RAG:
> Retrieve → Generate → Answer

Corrective RAG:
> Retrieve → Evaluate → Fix → Generate

**Adaptive RAG goes further** by:
- Evaluating document relevance
- Adapting the query if documents are weak
- Falling back to web search when needed
- Validating generated answers
- Deciding when to stop, retry, or reject unsupported queries

This makes the system **robust, self-adjusting, and production-ready**.

---

## 🏗️ Architecture Overview

<p align="center">
  <img src="assets/adaptive_rag.png" width="650"/>
</p>

---

## 🔄 Execution Flow (High Level)

```text
__start__
   │
   ├──► retrieve (vectorstore)
   │       │
   │       ▼
   │   grade_documents
   │       ├── useful ─────────────► generate ──► __end__
   │       └── not useful
   │               ▼
   │        transform_query
   │               │
   │               └──► retrieve (retry)
   │
   └──► web_search
               │
               ▼
            generate
               ├── useful ─────────► __end__
               └── not supported ──► stop
