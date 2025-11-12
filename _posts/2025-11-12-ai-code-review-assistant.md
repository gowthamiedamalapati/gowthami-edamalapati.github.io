---
layout: post
title: "AI Code Review Assistant — LLM + RAG with OpenAI & Pinecone"
date: 2025-11-12
tags: [LLM, RAG, OpenAI, Pinecone, React, Node, Serverless]
---

I built a lightweight **AI Code Review Assistant** that analyzes code, detects risks (security, correctness, performance, style), and can **auto-fix** snippets. It uses **OpenAI** for reasoning and **Pinecone** for retrieval-augmented generation (RAG) to ground suggestions in known patterns.

---

## What it does
- Paste code → get a **structured review** (bugs, security, performance, style)
- Click **Fix Code** → returns a corrected snippet (same API unless unsafe)
- Remembers common issues via **Pinecone** so reviews get better over time

---

## Architecture (high-level)

**Frontend (React + Vite)**
- Textarea input, language selector, file name
- Calls `/analyze` and `/fix`
- Displays structured review and fixed code

**Backend (Node.js + Express)**
- `POST /analyze`: validate → retrieve similar patterns (Pinecone) → call LLM
- `POST /fix`: ask LLM for raw corrected code only
- Input validation with Zod

**RAG (OpenAI + Pinecone)**
- We store “patterns” (e.g., SQL injection, unsafe concatenation)
- When new code arrives, we embed it and **query Pinecone** for similar patterns
- We augment the prompt with retrieved patterns to reduce hallucination and give targeted advice

---

## Key Endpoints

```http
POST /analyze
Body: { code, language, filename }
Resp: { ok, review: { raw, structured } }

POST /fix
Body: { code, language, filename }
Resp: { ok, code }

