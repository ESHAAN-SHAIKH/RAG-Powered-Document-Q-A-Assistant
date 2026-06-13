# RAG-Powered Document Q&A Assistant

> Upload PDFs. Ask questions. Get grounded, accurate answers — powered by semantic search and LLMs.

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![Flask](https://img.shields.io/badge/Flask-REST_API-black?style=flat-square&logo=flask)
![LangChain](https://img.shields.io/badge/LangChain-Orchestration-green?style=flat-square)
![FAISS](https://img.shields.io/badge/FAISS-Vector_Search-purple?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

---

## Overview

A production-ready Retrieval-Augmented Generation (RAG) pipeline that lets users
upload large PDF documents and query them in natural language. The system retrieves
semantically relevant chunks using FAISS vector search and sentence-transformer
embeddings, then passes grounded context to an LLM to generate faithful,
hallucination-reduced answers.

Evaluated using the RAGAS framework — achieving a faithfulness score of 0.87.

---

## Architecture

```
User Query
    │
    ▼
Embedding Model (all-MiniLM-L6-v2)
    │
    ▼
FAISS Vector Search → Top-K Relevant Chunks
    │
    ▼
Prompt Builder (Query + Context)
    │
    ▼
LLM (Groq / LLaMA3) → Grounded Answer
    │
    ▼
Flask REST API → JSON Response
```

---

## Tech Stack

| Component     | Technology                        |
|---------------|-----------------------------------|
| Orchestration | LangChain                         |
| Embeddings    | sentence-transformers (MiniLM-L6) |
| Vector Store  | FAISS                             |
| LLM           | Groq API (LLaMA3-8b)             |
| PDF Parsing   | pdfplumber                        |
| API Framework | Flask                             |
| Evaluation    | RAGAS                             |
| Language      | Python 3.10+                      |

---

## Setup

### 1. Clone the repository
```bash
git clone https://github.com/ESHAAN-SHAIKH/rag-document-qa.git
cd rag-document-qa
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure environment
```bash
cp .env.example .env
# Open .env and add your Groq API key
```

### 4. Run the API
```bash
python app/main.py
```
API will be live at `http://localhost:5000`

---

## API Usage

### Upload a PDF
```bash
curl -X POST http://localhost:5000/upload \
  -F "files=@your_document.pdf"
```
**Response:**
```json
{
  "status": "success",
  "chunks_indexed": 142,
  "documents": ["your_document.pdf"],
  "time_ms": 1840
}
```

### Ask a Question
```bash
curl -X POST http://localhost:5000/ask \
  -H "Content-Type: application/json" \
  -d '{"question": "What is the main conclusion of the document?"}'
```
**Response:**
```json
{
  "answer": "The main conclusion is...",
  "sources": [
    {
      "document": "your_document.pdf",
      "page": 4,
      "chunk": "...relevant extracted text..."
    }
  ],
  "response_time_ms": 287
}
```

### Check Health
```bash
curl http://localhost:5000/health
```

### Reset Index
```bash
curl -X DELETE http://localhost:5000/reset
```

---

## Evaluation Results (RAGAS)

Evaluated on 5 domain-specific QA pairs using the RAGAS framework:

| Metric           | Score |
|------------------|-------|
| Faithfulness     | 0.87  |
| Answer Relevancy | 0.83  |
| Context Recall   | 0.81  |

> Faithfulness measures whether the answer is grounded strictly in
> retrieved context — reducing hallucination.

---

## Project Structure

```
rag-document-qa/
├── app/
│   ├── main.py              # Flask entry point
│   ├── routes.py            # API endpoints
│   ├── rag_pipeline.py      # Retrieval + LLM logic
│   ├── embeddings.py        # FAISS indexing
│   └── utils.py             # PDF parsing + chunking
├── notebooks/
│   └── ragas_evaluation.ipynb
├── tests/
│   ├── test_pipeline.py
│   └── test_api.py
├── data/                    # Upload PDFs here
├── vectorstore/             # FAISS index stored here
├── requirements.txt
├── .env.example
└── README.md
```

---

## Key Features

- Multi-document ingestion with automatic chunking and indexing
- Semantic search via FAISS with cosine similarity thresholding
- Hallucination reduction by strictly grounding responses in context
- Sub-300ms end-to-end API response time
- Source attribution — every answer cites document and page number
- Evaluated with RAGAS for measurable answer quality

---

## License

MIT License — free to use, modify, and distribute.
