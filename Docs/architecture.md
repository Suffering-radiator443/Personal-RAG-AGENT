# Personal RAG Agent — Architecture

## 1. System Overview

The Personal RAG Agent is built as a document-based Retrieval-Augmented Generation system.

The architecture has two primary workflows:

1. **Document Ingestion Workflow** — prepares knowledge documents, generates embeddings, and stores them in Supabase Vector Store.
2. **RAG Retrieval Workflow** — receives a user query, retrieves relevant document chunks, and uses the retrieved context to generate an answer.

```text
                 ┌──────────────────┐
                 │    Google Drive  │
                 │ PDF / Markdown   │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │       n8n        │
                 │ Ingestion        │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ Content          │
                 │ Extraction       │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ Text Splitter    │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ OpenAI           │
                 │ Embeddings       │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ Supabase         │
                 │ Vector Store     │
                 └────────┬─────────┘
                          │
                          │ Retrieval
                          ▼
                 ┌──────────────────┐
                 │ RAG Retrieval    │
                 │ Workflow         │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ LLM / AI Agent   │
                 └────────┬─────────┘
                          │
                          ▼
                    Final Answer
```

---

## 2. Architecture Components

| Component | Responsibility |
|---|---|
| Google Drive | Source for personal knowledge documents |
| n8n | Workflow automation and orchestration |
| Extract From File | Extracts content from supported documents |
| Text Splitter | Divides documents into smaller chunks |
| OpenAI Embeddings | Converts text chunks into vector representations |
| Supabase | Stores vectors and document information |
| RAG Retrieval | Finds semantically relevant document chunks |
| LLM / AI Agent | Generates the final contextual response |

---

# 3. Document Ingestion Architecture

The ingestion workflow converts documents from Google Drive into searchable vector data.

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
    Document Content
          ↓
Split Content into Chunks
          ↓
Generate Embeddings
          ↓
Store Document Vectors
```

### 3.1 Discover Knowledge Files

The workflow searches the configured Google Drive knowledge-base folders.

The knowledge base can contain personal information and project documentation.

Example:

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

### 3.2 Process Each File

The workflow processes discovered files individually so that each document can pass through the appropriate extraction and embedding pipeline.

### 3.3 Download Knowledge File

The selected Google Drive file is downloaded before content extraction.

### 3.4 Detect File Type

The workflow determines which extraction path should be used.

```text
Downloaded File
      ↓
Detect File Type
    ↙       ↘
  PDF       Markdown
   ↓           ↓
PDF Extract  Text Extract
```

### 3.5 Extract Content

PDF documents and Markdown/text documents use the appropriate extraction process.

The result is normalized into document content that can be processed by the chunking stage.

---

# 4. Text Chunking

Large documents are divided into smaller chunks before embedding.

```text
Large Document
      ↓
   Text Splitter
      ↓
 ┌────────┬────────┬────────┐
 │Chunk 1 │Chunk 2 │Chunk 3 │ ...
 └────────┴────────┴────────┘
```

### Why chunking is required

Chunking helps the retrieval system:

- Find specific sections of a document
- Improve semantic search relevance
- Avoid sending an entire document to the LLM
- Keep retrieved context focused

The exact chunk size and overlap can be tuned later based on retrieval quality.

---

# 5. Embedding Architecture

Each document chunk is converted into a vector representation.

```text
Document Chunk
      ↓
OpenAI Embedding Model
      ↓
Vector Representation
      ↓
Supabase Vector Store
```

The embedding represents the semantic meaning of the text.

The same embedding approach is used when converting a user query into a vector during retrieval.

This allows the system to compare the semantic similarity between the query and stored document chunks.

---

# 6. Vector Database Architecture

Supabase acts as the vector storage layer.

Conceptually, the stored information contains:

```text
Supabase Vector Store
│
├── Document Content
├── Embedding Vector
├── Metadata
└── Document Information
```

The embedding vectors are used for semantic similarity search.

When a user asks a question, the retrieval workflow searches these stored vectors to find relevant knowledge.

---

# 7. RAG Retrieval Architecture

The retrieval workflow converts a user question into a search query and retrieves relevant knowledge.

```text
User Question
      ↓
Prepare Query
      ↓
Generate Query Embedding
      ↓
Supabase Vector Search
      ↓
Relevant Document Chunks
      ↓
Retrieved Context
      ↓
LLM / AI Agent
      ↓
Final Answer
```

### Retrieval process

1. The user submits a question.
2. The query is converted into an embedding.
3. Supabase performs semantic similarity search.
4. Relevant document chunks are retrieved.
5. Retrieved chunks are provided as context to the LLM.
6. The LLM generates the final response using the retrieved knowledge.

---

# 8. End-to-End Data Flow

The complete system can be represented as:

```text
                    INGESTION
                       │
                       ▼
                 Google Drive
                       │
                       ▼
                  n8n Workflow
                       │
                       ▼
               Document Extraction
                       │
                       ▼
                  Text Chunks
                       │
                       ▼
              OpenAI Embeddings
                       │
                       ▼
             Supabase Vector Store
                       │
                       │
                       │
                    RETRIEVAL
                       │
                       ▼
                  User Question
                       │
                       ▼
              Query Embedding
                       │
                       ▼
             Semantic Vector Search
                       │
                       ▼
              Relevant Chunks
                       │
                       ▼
                    LLM
                       │
                       ▼
                Final Answer
```

---

# 9. Why RAG?

Without retrieval, an LLM mainly relies on its existing model knowledge.

```text
User
  ↓
LLM
  ↓
Generic Response
```

With RAG:

```text
User
  ↓
Query
  ↓
Knowledge Retrieval
  ↓
Personal Documents
  ↓
Relevant Context
  ↓
LLM
  ↓
Contextual Response
```

RAG allows the application to use information from the user's own knowledge base without requiring that information to be part of the model's original training data.

---

# 10. Current V1 Architecture Scope

### Included

- Google Drive document ingestion
- PDF ingestion
- Markdown ingestion
- Document extraction
- Text chunking
- OpenAI embeddings
- Supabase Vector Store
- Semantic retrieval
- LLM-based answer generation

### Not Included in V1

- LinkedIn URL ingestion
- GitHub URL ingestion
- Website crawling
- Automatic web-content synchronization
- Multi-user architecture
- Production SaaS interface
- Advanced document version management

The current V1 is intentionally focused on building and validating the core document-based RAG pipeline.

---

# 11. Future Architecture

Future versions can extend the ingestion layer to support additional knowledge sources.

```text
Google Drive ───────┐
                    │
GitHub URLs ────────┤
                    │
LinkedIn/Data ──────┤
                    │
Websites ───────────┘
          ↓
 Universal Ingestion Layer
          ↓
     Content Extraction
          ↓
        Chunking
          ↓
      Embeddings
          ↓
 Supabase Vector Store
          ↓
    RAG Retrieval
          ↓
          LLM
```

Possible future capabilities:

- GitHub repository ingestion
- Website/documentation ingestion
- LinkedIn profile data ingestion
- Automatic synchronization
- Document update detection
- User authentication
- Web-based chat interface
- Multi-user knowledge bases
- Production deployment

---

# 12. Design Principles

The V1 architecture follows these principles:

### Separation of ingestion and retrieval

Ingestion and retrieval are separate workflows.

This makes the system easier to debug, maintain, and extend.

### Reusable knowledge base

Both ingestion and retrieval use the same Supabase vector store.

### Source-independent retrieval

The retrieval workflow does not need to know whether the knowledge originated from a PDF or Markdown document. It searches the resulting vector representations.

### Modular workflow design

Individual processing stages can be replaced or upgraded without redesigning the entire system.

---

# 13. Technology Stack

```text
Google Drive
     ↓
n8n
     ↓
Document Extraction
     ↓
Text Splitter
     ↓
OpenAI Embeddings
     ↓
Supabase / pgvector
     ↓
RAG Retrieval
     ↓
LLM
```

### Technologies

- **n8n** — workflow automation
- **Google Drive** — document source
- **OpenAI Embeddings** — embedding generation
- **Supabase** — vector database
- **PostgreSQL / pgvector** — vector storage layer
- **RAG** — retrieval architecture
- **LLM** — response generation

---

# 14. V1 Status

The core V1 pipeline is complete and tested for the supported document-based workflow.

The project is intentionally frozen at this stage so that the implementation can be documented, versioned, and published before adding larger V2 features.
