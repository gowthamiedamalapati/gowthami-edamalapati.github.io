

---
layout: post
title: "AI Code Review Assistant — LLM + RAG with OpenAI & Pinecone"
date: 2025-11-12
categories: [AI, LLM, RAG, Pinecone, OpenAI]
---

## 🚀 Project Overview

As part of my learning journey in AI integrations, I built a full-stack **AI Code Review Assistant** — a tool that analyzes source code, detects security or performance issues, and suggests automated fixes.

It uses:
- **OpenAI GPT models** for reasoning and feedback generation  
- **Pinecone** for vector storage and similarity search (RAG)  
- **React + Tailwind** for the frontend  
- **FastAPI / Express (backend)** for handling requests and prompt orchestration  

---

## ⚙️ Architecture


- The **frontend** lets users paste code and click “Analyze” or “Fix.”  
- The **backend** validates input and sends it to OpenAI, optionally enriching context from Pinecone.  
- The **RAG pipeline** retrieves semantically similar code patterns to ground AI responses in realistic examples.  

---

## 🧠 Retrieval-Augmented Generation (RAG)

To improve accuracy and reduce hallucinations:
- I stored embeddings of known code “patterns” (e.g., SQL injection, unsafe concatenation, missing error handling) in Pinecone.
- When new code is submitted, it’s embedded using OpenAI’s `text-embedding-3-small` model.
- Pinecone returns the top-k most similar snippets.
- These retrieved examples are added to the OpenAI prompt for better reasoning and contextual review.

This gives the model real world grounding before generating suggestions, similar to how a senior engineer references prior code reviews.

---

## 🔌 Backend Endpoints

```http
POST /analyze
Body: { code, language, filename }
Resp: { ok, review: { raw, structured } }

POST /fix
Body: { code, language, filename }
Resp: { ok, code }

💡 Learnings and Takeaways
🧩 1. AI Integration Patterns

LLMs alone are stateless, but connecting them with a vector database (Pinecone) transforms them into a reasoning system with memory.
Building a small RAG pipeline gave me deeper understanding of embedding storage, similarity search, and context injection.

🧠 2. Prompt Engineering Matters

Different instructions drastically changed the quality of suggestions.
Adding “explain like a senior engineer reviewing production code” improved tone and structure.

⚡ 3. End-to-End Ownership

I built both frontend and backend — integrating Tailwind, React hooks (useState, useEffect), and asynchronous fetch requests to verify system health.
Debugging full-stack OpenAI + Pinecone integration taught me how to trace data flow and manage environment variables securely.

🪄 Future Improvements

- Deploy backend to Render or AWS Lambda

- Add user authentication for personalized context memory

- Implement LLM feedback fine-tuning based on user-rated responses

🧾 Conclusion

This project helped me explore LLM integration engineering — connecting AI reasoning models with external data sources for intelligent automation.
It reinforced how RAG pipelines, when implemented thoughtfully, can make AI assistants context-aware and highly reliable.