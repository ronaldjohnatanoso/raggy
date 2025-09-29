# Raggy - RAG Application

A powerful Retrieval-Augmented Generation (RAG) application that allows you to upload PDF documents, process them into searchable chunks, and ask questions about their content using AI.

## 🚀 Features

- **PDF Document Ingestion**: Upload and process PDF files into searchable chunks
- **Vector Search**: Efficient similarity search using Qdrant vector database
- **AI-Powered Q&A**: Ask questions about your documents using OpenAI's GPT models
- **Streamlit Web Interface**: User-friendly web interface for document upload and querying
- **Workflow Orchestration**: Robust processing pipeline using Inngest
- **FastAPI Backend**: RESTful API with automatic documentation

## 🏗️ Architecture

The application consists of several key components:

- **FastAPI Backend** (`main.py`): Handles API endpoints and Inngest workflow functions
- **Streamlit Frontend** (`steamlit_app.py`): Web interface for user interactions
- **Document Processing** (`data_loader.py`): PDF parsing and text chunking using LlamaIndex
- **Vector Database** (`vector_db.py`): Qdrant integration for vector storage and retrieval
- **Type Definitions** (`custom_types.py`): Pydantic models for type safety

## 📋 Prerequisites

- Python 3.12+
- Qdrant vector database (running locally or remotely)
- OpenAI API key
- Inngest development server (for workflow orchestration)

## 🛠️ Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd raggy
   ```

2. **Install dependencies**:
   ```bash
   # Using uv (recommended)
   uv sync

   # Or using pip
   pip install -r requirements.txt
   ```

3. **Set up environment variables**:
   Create a `.env` file in the project root:
   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   INNGEST_API_BASE=http://127.0.0.1:8288/v1  # Optional, defaults to local dev server
   ```

4. **Start Qdrant database**:
   ```bash
   # Using Docker
   docker run -p 6333:6333 qdrant/qdrant
   ```

5. **Start Inngest development server**:
   ```bash
   # Install Inngest CLI if not already installed
   npm install -g inngest-cli
   
   # Start the development server
   inngest dev
   ```

## 🚀 Usage

### Starting the Application

1. **Start the FastAPI backend**:
   ```bash
   uvicorn main:app --reload --port 8000
   ```

2. **Start the Streamlit frontend**:
   ```bash
   streamlit run steamlit_app.py
   ```

3. **Access the application**:
   - Streamlit UI: http://localhost:8501
   - FastAPI docs: http://localhost:8000/docs
   - Inngest dashboard: http://localhost:8288

### Using the Web Interface

1. **Upload a PDF**:
   - Navigate to the Streamlit interface
   - Use the file uploader to select a PDF document
   - The system will automatically process and index the document

2. **Ask Questions**:
   - Enter your question in the text input field
   - Adjust the number of chunks to retrieve (default: 5)
   - Click "Ask" to get an AI-generated answer based on your documents

### API Endpoints

The FastAPI backend provides the following Inngest workflow endpoints:

- **PDF Ingestion**: Triggered by `rag/ingest_pdf` event
- **Document Query**: Triggered by `rag/query_pdf_ai` event

## 🔧 Configuration

### Vector Database Settings

Modify the `QdrantStorage` class in `vector_db.py` to customize:
- Database URL (default: `http://localhost:6333`)
- Collection name (default: `docs`)
- Vector dimensions (default: `3072` for OpenAI's text-embedding-3-large)

### Text Processing Settings

Adjust chunking parameters in `data_loader.py`:
- Chunk size (default: `1000`)
- Chunk overlap (default: `0`)
- Embedding model (default: `text-embedding-3-large`)

### AI Model Settings

Configure the OpenAI model in `main.py`:
- Model (default: `gpt-4o-mini`)
- Max tokens (default: `1024`)
- Temperature (default: `0.2`)

## 📁 Project Structure

```
raggy/
├── main.py              # FastAPI application with Inngest functions
├── steamlit_app.py      # Streamlit web interface
├── data_loader.py       # PDF processing and embedding
├── vector_db.py         # Qdrant vector database operations
├── custom_types.py      # Pydantic type definitions
├── pyproject.toml       # Project configuration
├── requirements.txt     # Python dependencies
├── uploads/             # Directory for uploaded PDFs
└── README.md           # This file
```

## 🔍 How It Works

1. **Document Ingestion**:
   - PDF files are uploaded through the Streamlit interface
   - Documents are parsed using LlamaIndex's PDFReader
   - Text is split into chunks using SentenceSplitter
   - Chunks are embedded using OpenAI's embedding model
   - Embeddings are stored in Qdrant vector database

2. **Question Answering**:
   - User questions are embedded using the same model
   - Similar document chunks are retrieved from Qdrant
   - Retrieved context is combined with the question
   - OpenAI's GPT model generates an answer based on the context

## 🐛 Troubleshooting

### Common Issues

1. **Qdrant Connection Error**:
   - Ensure Qdrant is running on the correct port (6333)
   - Check if the URL in `vector_db.py` matches your setup

2. **OpenAI API Errors**:
   - Verify your API key is correctly set in the `.env` file
   - Check your OpenAI account has sufficient credits

3. **Inngest Workflow Issues**:
   - Ensure the Inngest development server is running
   - Check the Inngest dashboard for workflow execution logs

4. **PDF Processing Errors**:
   - Ensure uploaded PDFs are not password-protected
   - Check that PDFs contain extractable text (not just images)

### Logs and Debugging

- FastAPI logs: Check the uvicorn server output
- Streamlit logs: Check the streamlit server output
- Inngest logs: Available in the Inngest dashboard at http://localhost:8288

