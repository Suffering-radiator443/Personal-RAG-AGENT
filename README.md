# Personal RAG Agent

A personal Retrieval-Augmented Generation (RAG) agent built with **n8n, Google Drive, OpenAI Embeddings, and Supabase Vector Store**.

The system ingests personal knowledge documents such as PDF and Markdown files, converts their content into vector embeddings, stores them in Supabase, and retrieves relevant information when the user asks a question.

> **Current version:** V1  
> **Scope:** Google Drive document ingestion + RAG retrieval

## 🚀 Overview

The Personal RAG Agent is designed as a personal knowledge assistant.

Instead of manually searching through resumes, profile information, and project documentation, the agent retrieves relevant information from an indexed knowledge base and uses it to generate contextual answers.

### Current knowledge sources

- Resume PDF
- Profile Markdown file
- Project Markdown files
- Other supported PDF/Markdown documents stored in Google Drive

### Current V1 limitation

URL-based ingestion such as LinkedIn and GitHub URLs is **not included in V1**. It is planned as a future enhancement.

## 🎯 Problem

Personal information and project documentation are often distributed across multiple files.

Manually searching these documents every time information is needed is repetitive and inefficient.

This project solves that problem by creating a searchable semantic knowledge base using RAG.

## 💡 Solution

```text
Google Drive
     ↓
Document Discovery
     ↓
File Processing
     ↓
Download File
     ↓
Detect File Type
     ↓
Extract Content
     ↓
Text Splitting
     ↓
OpenAI Embeddings
     ↓
Supabase Vector Store
     ↓
RAG Retrieval
     ↓
AI Generated Answer
```

## 🏗️ Architecture

### 1. Document Ingestion Workflow

```text
Start Document Ingestion
          ↓
Discover Knowledge Files
          ↓
Process Each File
          ↓
Download Knowledge File
          ↓
Detect File Type
       ↙       ↘
     PDF       Markdown
      ↓           ↓
Extract PDF   Extract Markdown
       ↘       ↙
    Split Content into Chunks
              ↓
      Generate Embeddings
              ↓
      Store Document Vectors
```

### 2. RAG Retrieval Workflow

```text
User Query
    ↓
Prepare Query
    ↓
Generate Query Embedding
    ↓
Retrieve Relevant Knowledge
    ↓
Provide Context to LLM
    ↓
Generate Final Answer
```

## 🧩 Main Components

### Google Drive
Acts as the document source for the knowledge base.

### n8n
Used as the workflow automation layer for file discovery, processing, extraction, embeddings, vector storage, and retrieval.

### OpenAI Embeddings
Converts document chunks and user queries into vector representations for semantic similarity search.

### Supabase
Stores vectors and the document content/metadata required for retrieval.

### RAG
Combines the user query, relevant retrieved knowledge, and an LLM to generate contextual answers.

## 📂 Project Structure

```text
personal-rag-agent/
│
├── README.md
│
├── workflows/
│   ├── ingestion-workflow.json
│   └── retrieval-workflow.json
│
├── screenshots/
│   ├── ingestion-workflow.png
│   ├── retrieval-workflow.png
│   └── supabase-vector-store.png
│
├── docs/
│   └── architecture.md
```

## ⚙️ Ingestion Workflow

1. **Discover Knowledge Files** — Search the configured Google Drive knowledge-base folders.
2. **Process Each File** — Process discovered files individually.
3. **Download Knowledge File** — Download the selected file.
4. **Detect File Type** — Route PDF and Markdown/text files to the appropriate extraction path.
5. **Extract Content** — Extract text from the document.
6. **Split Content into Chunks** — Divide large content into smaller retrieval-friendly chunks.
7. **Generate Embeddings** — Generate vector representations using an OpenAI embedding model.
8. **Store Document Vectors** — Store embeddings and associated information in Supabase Vector Store.

## 🔎 RAG Retrieval

```text
User Question
      ↓
Query Embedding
      ↓
Semantic Search
      ↓
Relevant Document Chunks
      ↓
LLM Context
      ↓
Final Answer
```

## 🗃️ Knowledge Base

Example Google Drive structure:

```text
Personal RAG Knowledge Base/
│
├── profile/
│   ├── profile.md
│   └── resume.pdf
│
└── project/
    ├── resume-analyzer.md
    └── auto-gmail-reply-agent.md
```


## 🧪 Testing

The V1 workflow has been tested for:

- PDF ingestion
- Markdown ingestion
- Google Drive file processing
- Content extraction
- Embedding generation
- Supabase vector storage
- RAG retrieval

## 📌 Current V1 Scope

### Included

- Google Drive integration
- PDF ingestion
- Markdown ingestion
- Document extraction
- Text chunking
- OpenAI embeddings
- Supabase Vector Store
- RAG retrieval
- AI-generated responses

### Not included in V1

- LinkedIn URL ingestion
- GitHub URL ingestion
- Website crawling
- Automatic URL synchronization
- Production SaaS interface
- Advanced document versioning

These can be considered for future versions.

## 🔮 Future Improvements

- LinkedIn profile ingestion
- GitHub repository ingestion
- Website/documentation ingestion
- Automatic document synchronization
- Better document version tracking
- Duplicate/update detection
- User authentication
- Web-based chat interface
- Production deployment
- Multi-user knowledge bases

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow automation |
| Google Drive | Knowledge source |
| OpenAI Embeddings | Vector generation |
| Supabase | Vector database |
| PostgreSQL / pgvector | Vector storage layer |
| RAG | Knowledge retrieval |
| LLM | Answer generation |

## 📖 How to Run

### 1. Prepare Google Drive
Create a knowledge-base folder and upload supported PDF/Markdown documents.

### 2. Configure n8n
Import the ingestion and retrieval workflow JSON files into n8n.

### 3. Configure credentials
Add Google Drive, OpenAI, and Supabase credentials in n8n.

### 4. Configure Supabase
Create the required vector-store table and vector functionality.

### 5. Run ingestion
Execute the ingestion workflow and verify that document embeddings are stored in Supabase.

### 6. Run retrieval
Execute the retrieval workflow and ask questions related to the indexed knowledge.

## 📸 Screenshots

After adding screenshots to `screenshots/`:

```markdown
![Ingestion Workflow](Screenshots/ingestion-workflow.png)

![Retrieval Workflow](Screenshots/retrieval-workflow.png)

![Supabase Vector Store](Screenshots/supabase-vector-store.png)
```

## 🎓 What This Project Demonstrates

- AI Agent workflow development
- Retrieval-Augmented Generation
- Vector databases
- Embeddings
- Semantic search
- Document processing
- n8n workflow automation
- Supabase
- LLM integration
- Knowledge-base architecture

## 👨‍💻 Author

**Shivam Kumar**

B.Tech — Computer Science & Engineering

