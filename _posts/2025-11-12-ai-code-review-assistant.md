---
layout: post
title: "AI Code Review Assistant — LLM + RAG with OpenAI & Pinecone"
date: 2025-11-12
categories: [AI, LLM, RAG, Pinecone, OpenAI]
---

## Project Overview
As part of my learning journey in AI integrations, I built a full-stack **AI Code Review Assistant** — a tool that analyzes source code, detects security or performance issues, and suggests automated fixes.  
<!--more-->

It uses:
- **OpenAI GPT models** for reasoning and feedback generation  
- **Pinecone** for vector storage and similarity search (RAG)  
- **React + Tailwind** for the frontend  
- **Express (backend)** for handling requests and prompt orchestration

## Architecture
The **frontend** lets users paste code and click “Analyze” or “Fix.”  
The **backend** validates input and sends it to OpenAI, optionally enriching context from Pinecone.  
The **RAG pipeline** retrieves semantically similar code patterns to ground AI responses in realistic examples.

## Retrieval-Augmented Generation (RAG)
- Store embeddings of known code patterns (e.g., SQL injection, unsafe concatenation, missing error handling) in Pinecone.  
- Embed incoming code with `text-embedding-3-small`.  
- Retrieve top-k similar snippets.  
- Add them to the OpenAI prompt for grounded advice.

## Backend Endpoints
```http
POST /analyze
Body: { code, language, filename }
Resp: { ok, review: { raw, structured } }

POST /fix
Body: { code, language, filename }
Resp: { ok, code }
