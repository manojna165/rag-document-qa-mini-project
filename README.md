# RAG-Based Document Question Answering System

## Overview

This project implements a Retrieval-Augmented Generation (RAG) system that answers questions based on the contents of PDF documents.

It extracts and cleans text from PDFs, divides the text into smaller chunks, generates embeddings, and stores them in ChromaDB. When a user asks a question, the system retrieves relevant passages and uses the Google Gemini API to generate a context-grounded answer with source references.

## Features

- PDF text extraction using PyPDF
- Text cleaning and chunking using LangChain text splitters
- Text embeddings using Sentence Transformers
- Persistent vector storage and similarity search using ChromaDB
- Context-based answer generation using Google Gemini
- FastAPI endpoints for document indexing and question answering
- Pydantic request and response validation
- Error handling for missing documents, invalid requests, and API failures
- Source metadata including document name, page number, chunk ID, and retrieval distance

## Technology Stack

- Python
- FastAPI
- Pydantic
- PyPDF
- LangChain Text Splitters
- Sentence Transformers (`all-MiniLM-L6-v2`)
- ChromaDB
- Google Gemini API
- python-dotenv

## Project Structure

```text
Task5-RAG-Document-QA/
├── main.py
├── document_processor.py
├── embeddings.py
├── vector_store.py
├── documents/
│   └── ar_vr_medical_training.pdf
├── .env.example
├── .gitignore
├── requirements.txt
├── rag_results.json
├── Task5_RAG_Screenshots.pdf
└── README.md
```

## Setup and Installation

### 1. Clone the repository

```bash
git clone https://github.com/manojna165/Retrieval-Augmented-Generation-RAG-.git
cd Retrieval-Augmented-Generation-RAG-
```

### 2. Create and activate a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file in the project root using `.env.example` as a template.

Add your own Gemini API key and the name of a Gemini model available to your API account:

```text
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=your_working_gemini_model
```

Do not commit your actual `.env` file or expose your API key.

### 5. Run the application

```bash
uvicorn main:app --reload
```

Open the interactive API documentation:

http://127.0.0.1:8000/docs

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Checks whether the API is running |
| GET | `/documents/count` | Returns the number of indexed chunks |
| POST | `/ask` | Retrieves relevant passages and generates an answer |

### Example request

`POST /ask`

```json
{
  "question": "What are the main advantages of using virtual reality for surgical training?",
  "top_k": 4
}
```

The response contains the original question, the generated answer, and a list of retrieved sources with document names, page numbers, chunk IDs, distances, and text.

## RAG Workflow

1. Load PDF documents.
2. Extract and clean text.
3. Split text into overlapping chunks.
4. Generate embeddings for each chunk.
5. Store the chunks and embeddings in ChromaDB.
6. Convert the user's question into an embedding.
7. Retrieve the most relevant chunks using similarity search.
8. Pass the question and retrieved context to Gemini.
9. Return the generated answer and source information through FastAPI.

## Testing

The system was tested using questions about the sample AR/VR medical training document, including surgical training benefits, motion tracking, and patient safety. An out-of-scope question about India's population was also used to test whether the system avoids answering when the document does not provide sufficient information.

Sample results and screenshots are included in the repository.

## Limitations

- Answers depend on the quality and content of the source documents.
- Text extraction may not capture information contained only in images or complex diagrams.
- Retrieved passages may not always contain enough information to answer a question fully.
- Answer generation depends on Gemini API availability and quota.
- Similarity distances help assess retrieval relevance but are not a guarantee of answer correctness.

## Future Improvements

- Support multiple document formats.
- Improve retrieval quality with reranking.
- Add automated tests for API endpoints.
- Improve document parsing for tables and image-based PDFs.
- Add a user interface for uploading documents and asking questions.

## Author

Developed as a mini-project demonstrating Retrieval-Augmented Generation (RAG), vector search, and LLM-based document question answering.