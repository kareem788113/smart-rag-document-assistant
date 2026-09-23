# Smart RAG Document Assistant

## Overview

The Smart RAG Document Assistant is a local Retrieval-Augmented Generation (RAG) system that answers questions using information retrieved from a collection of PDF documents.

The system is designed to keep answers grounded in the provided documents. The LLM receives the retrieved context together with the user's question and is instructed not to use outside knowledge.

### Core Pipeline

```text
PDF Documents
      ↓
Text Extraction
      ↓
Cleaning & Chunking
      ↓
Sentence Transformers
      ↓
ChromaDB Vector Store
      ↓
Semantic Retrieval
      ↓
Retrieved Context
      ↓
Ollama / Llama 3.2 3B
      ↓
Grounded Answer + Sources
      ↓
Streamlit Frontend
````

---

## Project Status

The project is implemented end-to-end and ready for demonstration.

### Completed

* PDF document processing
* Text extraction and cleaning
* Recursive document chunking
* Local sentence embeddings
* Persistent ChromaDB vector store
* Semantic document retrieval
* Local LLM generation using Ollama
* Grounded answer generation
* Source and page information returned with answers
* FastAPI backend
* `GET /health` endpoint
* `POST /query` endpoint
* CORS configuration
* FastAPI lifespan initialization
* Backend API validation and tests
* Streamlit chat-style frontend
* Environment-based frontend API configuration
* 10-question RAG evaluation
* Persistent vector store
* Local end-to-end testing


## Track

This project follows the **Core Track** of the RAG Document Assistant project.

The project focuses on document-based text RAG, including:
- PDF text extraction
- Text chunking
- Embeddings
- Persistent vector storage
- Semantic retrieval
- Grounded answer generation
- Source citations
- FastAPI backend
- Streamlit frontend

The Extended Track features such as image upload, computer vision, and YOLO-based detection/classification are not included.

---

## Architecture

```text
                    ┌─────────────────────┐
                    │    PDF Documents    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Text Extraction   │
                    │       PyPDF         │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Cleaning & Chunking │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Sentence Transformers│
                    │  all-MiniLM-L6-v2   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │      ChromaDB       │
                    │ Persistent Vector DB│
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Semantic Retrieval  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Context + Question  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Ollama Llama 3.2 3B │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Answer + Sources    │
                    └─────────────────────┘
                               ▲
                               │
                    ┌──────────┴──────────┐
                    │      FastAPI        │
                    │       Backend       │
                    └──────────▲──────────┘
                               ▲
                               │
                    ┌──────────┴──────────┐
                    │      Streamlit      │
                    │      Frontend       │
                    └─────────────────────┘
```

---

## Tech Stack

| Component            | Technology                   |
| -------------------- | ---------------------------- |
| Programming Language | Python 3.10                  |
| Notebook             | Jupyter                      |
| PDF Processing       | PyPDF                        |
| Text Chunking        | Recursive Chunking           |
| Embeddings           | Sentence Transformers        |
| Embedding Model      | `all-MiniLM-L6-v2`           |
| Vector Database      | ChromaDB                     |
| LLM Runtime          | Ollama                       |
| LLM                  | `llama3.2:3b`                |
| Backend API          | FastAPI                      |
| Frontend             | Streamlit                    |
| Configuration        | YAML / Environment Variables |
| Testing              | Pytest                       |
| HTTP Client          | Requests                     |

---

## Dataset

The project uses a collection of **5 PDF documents totaling 15 pages**.

The document topics include:

* Machine Learning
* Quantum Mechanics
* Ancient Egypt
* Nutrition
* Climate Change

The documents are processed locally and indexed into a persistent ChromaDB vector store.

---

## RAG Configuration

The main RAG configuration is stored in:

```text
backend/config.yaml
```

Current configuration:

```yaml
rag:
  chunk_size: 700
  chunk_overlap: 100
  embedding_model: "all-MiniLM-L6-v2"
  ollama_model: "llama3.2:3b"
  collection_name: "science_docs"
  top_k: 4
  ollama_url: "http://localhost:11434"
```

### Configuration Explanation

* `chunk_size`: Size used when creating document chunks.
* `chunk_overlap`: Overlap between consecutive chunks.
* `embedding_model`: Sentence Transformer model used to create embeddings.
* `ollama_model`: Local LLM used for answer generation.
* `collection_name`: ChromaDB collection containing the indexed documents.
* `top_k`: Number of relevant chunks retrieved for a query.
* `ollama_url`: Local Ollama API endpoint.

---


## Backend

The backend is implemented using FastAPI.

### API Endpoints

#### Health Check

```http
GET /health
```

Example response:

```json
{
  "status": "ok",
  "service": "Smart RAG Document Assistant"
}
```

#### Query

```http
POST /query
```

Request:

```json
{
  "question": "What is machine learning?",
  "top_k": 4
}
```

Response:

```json
{
  "question": "What is machine learning?",
  "answer": "....",
  "sources": [
    "machine_learning_basics.pdf (Page 1)"
  ]
}
```

### API Documentation

When the backend is running, FastAPI provides interactive API documentation at:

```text
http://localhost:8000/docs
```

---

## Frontend

The frontend is implemented using Streamlit.

The interface provides:

* Chat-style interaction
* User question input
* Loading indicator
* Generated answer display
* Retrieved document sources
* Conversation history during the current session
* Friendly backend connection errors
* API error handling

The frontend reads the backend URL from an environment variable.

### Frontend Environment Variable

Create:

```text
frontend/.env
```

with:

```env
API_BASE_URL=http://localhost:8000
```

The `.env` file should not be committed to Git.

---

## Installation

### 1. Clone the Repository

```bash
git clone <[(https://github.com/kareem788113/smart-rag-document-assistant.git)]>
cd smart-rag-document-assistant-main
```

### 2. Create Virtual Environment

```bash
python -m venv .venv
```

Activate it on Windows:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Install Backend Requirements

```powershell
pip install -r backend/requirements.txt
```

### 4. Install Frontend Requirements

```powershell
pip install -r frontend/requirements.txt
```

### 5. Install Ollama

Install Ollama and make sure it is running locally.

Pull the required model:

```powershell
ollama pull llama3.2:3b
```

---

## Running the Project

### Start Backend

Open a terminal:

```powershell
cd backend
$env:PYTHONPATH="."
python -m uvicorn app.main:app --reload
```

Backend:

```text
http://localhost:8000
```

API documentation:

```text
http://localhost:8000/docs
```

### Start Frontend

Open another terminal:

```powershell
cd frontend
python -m streamlit run app.py
```

Frontend:

```text
http://localhost:8501
```

---

## Testing

The backend includes automated API tests.

Run:

```powershell
cd backend
$env:PYTHONPATH="."
pytest -v
```

Current tests cover:

* Successful query request
* Invalid query request / validation error

Expected result:

```text
2 passed
```

---

## RAG Evaluation

The RAG pipeline was evaluated using a set of **10 questions** covering different query types, including:

* Normal questions
* Complex questions
* Out-of-domain questions

The evaluation checks:

* Whether relevant context is retrieved
* Whether generated answers are grounded in the retrieved documents
* Whether unsupported information is rejected
* Whether source information is returned

Evaluation results are stored in:

```text
notebooks/evaluation_results.csv
```

---

## Grounding Behavior

The generation prompt instructs the LLM to answer using only the retrieved document context.

When the required information is not available in the provided documents, the system returns:

```text
Information not found in the provided documents.
```

This behavior helps keep generated responses grounded in the indexed document collection.

---

## Local Vector Store

The project uses a persistent ChromaDB vector store located at:

```text
backend/data/vector_store/
```

The backend loads the persisted vector store during application startup.

The vector store does not need to be rebuilt for every query.

---

## Example RAG Query

The core RAG module can also be used directly:

```python
from app.services.rag_core import rag_core

response = rag_core.rag_query(
    "What is machine learning?"
)

print(response)
``

Example response structure:

```python
{
    "question": "...",
    "answer": "...",
    "sources": [...]
}
```

---

## Environment Variables

### Backend

File:

```text
backend/.env.example
```

```env
APP_NAME=Smart RAG Document Assistant
FRONTEND_ORIGIN=http://localhost:8501
```

### Frontend

File:

```text
frontend/.env.example
```

```env
API_BASE_URL=http://localhost:8000
```

---

## Security and Git Hygiene

The repository excludes local environment and generated files such as:

```text
.env
.venv/
.venv-1/
__pycache__/
*.pyc
.pytest_cache/
*.log
```

Environment files containing local configuration should not be committed.

---

## Limitations

* The system answers only from the indexed document collection.
* Answer quality depends on retrieval quality and the available document content.
* Ollama and the selected LLM must be available locally.
* The current system is designed for local execution.

---

## Future Improvements

Possible future improvements include:

* Hybrid keyword and semantic retrieval
* Retrieval reranking
* Streaming LLM responses
* Authentication
* Cloud deployment
* Larger document collections
* More extensive automated evaluation
* Improved citation formatting
* Multimodal document support

---

## Project Validation

The project has been validated locally through:

* FastAPI application import
* Persistent ChromaDB loading
* Ollama connectivity
* End-to-end RAG queries
* Streamlit frontend interaction
* Automated backend API tests

The backend test suite currently passes:

```text
2 passed
```

---

## License

This project was developed as part of an **ITI 2026 graduation project**.

```

