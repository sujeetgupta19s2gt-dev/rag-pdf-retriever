# RAG PDF & Text Retriever Pipeline

A modular Python-based Retrieval-Augmented Generation (RAG) backend and similarity search engine built using LangChain, Sentence Transformers, and ChromaDB[cite: 1, 2, 3, 4]. This project implements the core retrieval architecture required to feed accurate, context-rich information to Large Language Models (LLMs)[cite: 1, 2, 3, 4].

---

## 🏗️ Architecture & Pipeline Overview

The project follows a standard RAG pipeline architecture consisting of three main phases[cite: 1, 2, 3, 4]:

1. **Data Ingestion & Chunking**: Loads raw documents (PDFs and Text files), extracts text content alongside rich metadata, and splits them into manageable chunks using `RecursiveCharacterTextSplitter`[cite: 1, 3, 4].
2. **Embedding & Vector Storage**: Generates dense vector representations using the `all-MiniLM-L6-v2` sentence transformer model and stores them persistently using **ChromaDB**.
3. **Retrieval Pipeline**: Accepts natural language user queries, computes similarity scores, and filters/ranks the most relevant document chunks to serve as context.

---

## 📂 Project Structure

```text
rag-pdf-retriever/
│
├── data/
│   ├── pdfs/               # Place your input PDF documents here
│   └── Python.txt          # Sample text data file
│
├── main.py                 # Core Python script containing classes and pipeline logic
├── requirements.txt        # Project dependencies
└── README.md               # Project documentation
⚙️ Prerequisites & Installation
Ensure you have Python installed, then install the required dependencies:

Bash
pip install -r requirements.txt
Dependencies List (requirements.txt)
langchain

langchain-core

langchain-community

langchain-text-splitters

pypdf

pymupdf

sentence-transformers

chromadb

scikit-learn

ipywidgets

🚀 How It Works (Code Modules)
1. Ingestion Pipeline
Loads documents from local directories and parses content:

Text Loader: Parses plain text files like Python.txt.   
JPG

PyPDF Loader: Automatically iterates through the data/pdfs/ folder, extracting text and page metadata page-by-page.   
JPG

2. Chunking
Breaks down large documents into smaller chunks to optimize embedding quality and stay within token windows:

Python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500, chunk_overlap=50
)
3. Embedding Manager (EmbeddingManager)
Encodes text chunks into 384-dimensional dense vectors using Hugging Face's all-MiniLM-L6-v2 model[cite: 3, 4].

4. Vector Store Manager (VectorStoreManager)
Handles local persistent storage using ChromaDB, ensuring your embedded documents are saved to disk under data/vector_store.   
JPG

5. Retrieval Engine (RAGRetriever)
Queries the vector database based on semantic similarity and calculates scores to return the top relevant context segments[cite: 4]:

Python
rag_retriever = RAGRetriever(embedding_manager, vector_store)
results = rag_retriever.retrieve("Your query here", top_k=5)
🛠️ Future Enhancements
[ ] Integrate an LLM (e.g., OpenAI, Groq, or Local Llama) to handle the Generation Pipeline step.

[ ] Build a simple Streamlit or Gradio UI for interactive querying.
