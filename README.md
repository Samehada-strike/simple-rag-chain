# Simple RAG Pipeline (LangChain + FAISS + Hugging Face)

This repository demonstrates a **clean, end-to-end Retrieval-Augmented Generation (RAG) pipeline**
built using **LangChain (LCEL)**, **FAISS**, **Sentence-Transformers**, and a **Hugging Face chat-based LLM**.

The goal of this project is to clearly illustrate **each core building block of RAG**
from raw data ingestion to an executable LCEL-based RAG chain.

---

## High-Level Architecture
Text Files
>>
LangChain Documents
>>
Text Chunking
>>
Embeddings (Sentence-Transformers)
>>
FAISS Vector Store
>>
Retriever
>>
Prompt Template
>>
LLM
>>
Answer

---

## Step 1: Data Ingestion

- Simple text files are created and stored under the `data/` directory.
- Each file represents a knowledge source (e.g. ML, DL, NLP).
- These files act as the **raw knowledge base** for the RAG system.

---

## Step 2: Loading Data as LangChain Documents

- Text files are loaded using LangChain loaders.
- Each file is converted into a `Document` object.
- Custom metadata is attached:
  - `topic`
  - `source`

This metadata flows through the entire RAG pipeline.

---

## Step 3: Text Chunking

- Documents are split using `RecursiveCharacterTextSplitter`.
- Chunking ensures:
  - Better semantic embeddings
  - Improved retrieval accuracy
- Metadata is preserved per chunk (including chunk IDs).

---

## Step 4: Embedding Model Initialization

- Sentence-Transformers model is used:
  - `all-MiniLM-L6-v2`
- Embeddings convert text chunks into dense vector representations.
- These vectors enable semantic similarity search.

---

## Step 5: Vector Store (FAISS)

- FAISS is used to store and index embeddings.
- Each chunk is stored along with its metadata.
- The FAISS index is persisted to disk for reuse.
- The index can be loaded later without recomputing embeddings.

---

## Step 6: Retriever Creation

- The FAISS vector store is converted into a **retriever**.
- The retriever performs similarity search over stored embeddings.
- Top-k relevant chunks are returned for each query.

---

## Step 7: Prompt Template

- A grounded prompt template is defined.
- The prompt:
  - Injects retrieved context
  - Injects the user question
  - Restricts the model to answer **only from provided context**
- This is the core mechanism that prevents hallucination.

---

## Step 8: LLM Initialization (Hugging Face)

- A chat-based Hugging Face LLM is initialized using:
  - `ChatHuggingFace`
  - `HuggingFaceEndpoint`
- The model is accessed via Hugging Face Inference API.
- API key is loaded securely from `.env`.

---

## Step 9: LCEL RAG Chain

- The RAG pipeline is built using **LangChain Expression Language (LCEL)**.
- The chain consists of:
  - Question passthrough
  - Retriever
  - Document formatter
  - Prompt template
  - LLM
  - Output parser
- Each step is modular, inspectable, and composable.

---

## Step 10: Running the RAG System

- A user question is passed to the LCEL chain.
- Relevant chunks are retrieved from FAISS.
- Context is injected into the prompt.
- The LLM generates a grounded answer.
- Final output is returned as plain text.

---

## Key Concepts Demonstrated

- End-to-end RAG design
- Semantic search with FAISS
- LCEL-based chain construction
- Metadata-aware retrieval
- Grounded prompting
- Modern Hugging Face + LangChain integration

---




