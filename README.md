# RAG PDF & Text Retriever Pipeline

A modular Python-based Retrieval-Augmented Generation (RAG) backend and similarity search engine built using LangChain, Sentence Transformers, ChromaDB, and Groq LLMs. This project implements the core retrieval and generation architecture required to feed accurate, context-rich information to Large Language Models (LLMs).

---

## 🏗️ Architecture & Pipeline Overview

The project follows a standard RAG pipeline architecture consisting of four main phases:
* **Data Ingestion & Chunking**: Loads raw documents (PDFs and Text files), extracts text content alongside rich metadata, and splits them into manageable chunks using `RecursiveCharacterTextSplitter`.
* **Embedding & Vector Storage**: Generates dense vector representations using the `all-MiniLM-L6-v2` sentence transformer model and stores them persistently using ChromaDB.
* **Retrieval Pipeline**: Accepts natural language user queries, computes similarity scores, and filters/ranks the most relevant document chunks to serve as context.
* **Generation Pipeline**: Combines the retrieved context with the user query into a structured prompt, feeding it to a high-performance LLM via the Groq API to generate accurate answers.

---

## 📂 Project Structure

```text
rag-pdf-retriever/
│
├── data/
│   ├── pdfs/               # Place your input PDF documents here
│   ├── vector_store/       # Local ChromaDB persistent database files (ignored by git)
│   └── Python.txt          # Sample text data file
│
├── .env                    # Secure environment file for API keys (ignored by git)
├── .gitignore              # Git ignore rules for sensitive and cache files
├── main.py (or Notebook)   # Core Python script / Jupyter notebook containing logic
└── README.md               # Project documentation
⚙️ Prerequisites & Installation
Ensure you have Python installed, then clone or set up your repository and install the required dependencies:

Bash
pip install langchain langchain-core langchain-community langchain-text-splitters pypdf pymupdf sentence-transformers chromadb scikit-learn python-dotenv langchain-groq ipywidgets
🔐 Environment Setup (Security Best Practice)
To protect your API keys from public exposure, this project uses a .env file rather than hardcoded credentials.

Create a file named .env in the root folder of your project.

Add your Groq API key inside the file:

Code snippet
GROQ_API_KEY=your_actual_groq_api_key_here
Ensure .env is included in your .gitignore file so it never gets pushed to GitHub.

🚀 How It Works (Code Modules)
1. Ingestion Pipeline
Loads documents from local directories and parses content:

Text Loader: Parses plain text files like Python.txt.

PyPDF Loader: Automatically iterates through the data/pdfs/ folder, extracting text and page metadata page-by-page.

2. Chunking
Breaks down large documents into smaller chunks to optimize embedding quality and stay within token windows:

Python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500, 
    chunk_overlap=50
)
chunks = text_splitter.split_documents(all_pdf_documents)
3. Embedding Manager (EmbeddingManager)
Encodes text chunks into 384-dimensional dense vectors using Hugging Face's all-MiniLM-L6-v2 model.

4. Vector Store Manager (VectorStoreManager)
Handles local persistent storage using ChromaDB, ensuring your embedded documents are saved to disk under data/vector_store.

5. Retrieval Engine (RAGRetriever)
Queries the vector database based on semantic similarity, converts distances to similarity scores, and returns the top relevant context segments:

Python
rag_retriever = RAGRetriever(embedding_manager, vector_store)
results = rag_retriever.retrieve("What is RAG?", top_k=3)
6. LLM Integration (Groq & LangChain)
Loads credentials securely using python-dotenv and integrates with Groq's high-speed inference engine to generate context-grounded responses:

Python
import os
from dotenv import load_dotenv
from langchain_groq import ChatGroq

load_dotenv()
api_key = os.getenv("GROQ_API_KEY")

llm = ChatGroq(
    groq_api_key=api_key,
    model="qwen/qwen2.5-7b-instruct",
    temperature=0,
    max_tokens=1024
)
🛠️ Future Enhancements
[x] Integrate an LLM (Groq API) to handle the Generation Pipeline step.

[ ] Build a simple Streamlit or Gradio UI for interactive querying.

[ ] Add automatic source citation (displaying filenames and page numbers) directly in LLM responses.
