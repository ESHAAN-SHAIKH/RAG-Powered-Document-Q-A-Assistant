# RAG-Powered Document Q&A Assistant

> Upload PDFs. Ask questions. Get grounded, accurate answers — powered by semantic search and LLMs.

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![Flask](https://img.shields.io/badge/Flask-REST_API-black?style=flat-square&logo=flask)
![LangChain](https://img.shields.io/badge/LangChain-Orchestration-green?style=flat-square)
![FAISS](https://img.shields.io/badge/FAISS-Vector_Search-purple?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

---

## Overview

A production-ready Retrieval-Augmented Generation (RAG) pipeline that lets 
users upload large PDF documents and query them in natural language. The system 
retrieves semantically relevant chunks using FAISS vector search and 
sentence-transformer embeddings, then passes grounded context to an LLM to 
generate faithful, hallucination-reduced answers.

Evaluated using the RAGAS framework — achieving a faithfulness score of 0.87.

---

## Architecture
