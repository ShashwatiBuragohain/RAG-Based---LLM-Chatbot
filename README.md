# RAG Based LLM Chatbot
Let's be real; we're drowning in PDFs. Textbooks, research papers, documentation, lecture notes, that random 50-page report your professor dropped at 11 PM. Reading through everything isn't realistic anymore, but we still need answers like we actually did the work.

We built an AI system that reads, understands, and converses with you based on your documents.This RAG Chatbot Project  lets users upload PDFs, retrieves the most relevant content, and generates context-aware answers using a large language model.
This is a document-aware conversational AI that:

* Ingests multiple PDFs and extracts their content
* Converts text into semantic embeddings for intelligent retrieval
* Searches through thousands of document chunks in milliseconds
* Generates contextually accurate answers using a powerful LLM
* Maintains conversation history for follow up questions
* Runs completely offline; data never leaves the machine

It's like having a research assistant who's actually read all our materials and can explain them back to us in plain English.

## System Architecture: 
**Phase 1**: Document Processing & Embedding Generation
When we upload PDFs, here's what happens behind the scenes:

1. Text Extraction: We use PyPDF2 or similar libraries to extract raw text from your PDF files, preserving structure and content integrity.
2. Intelligent Chunking: The extracted text gets split into smaller, semantically meaningful chunks (typically 500-1000 tokens). This is crucial because:

* LLMs have context limits
* Smaller chunks = more precise retrieval
* Each chunk represents a specific concept or topic

3. Embedding Generation: Every chunk passes through MiniLM (all-MiniLM-L6-v2), a lightweight but powerful sentence transformer model that:

* Converts text into 384-dimensional dense vectors
* Captures semantic meaning, not just keywords
* Enables similarity-based search rather than exact matching
* Processes embeddings blazingly fast (~14,000 sentences/second on CPU)

4. Vector Storage: All embeddings get indexed in FAISS (Facebook AI Similarity Search), a highly optimized library for similarity search that:

* Handles millions of vectors efficiently
* Performs approximate nearest neighbor search in milliseconds
* Uses advanced indexing structures (like IVF, HNSW) for speed
* Runs entirely in-memory for maximum performance




**Phase 2** : Query Processing & Retrieval
When we ask a question in the Gradio UI:

1. Query Embedding: Your question gets embedded using the same MiniLM model, ensuring semantic compatibility with document embeddings.
2. Similarity Search: FAISS compares your query vector against all stored document vectors using cosine similarity, returning the top-k most relevant chunks (typically k=3-5).
3. Context Assembly: Retrieved chunks get concatenated with your question to form a comprehensive context prompt.


**Phase 3**: Answer Generation
The magic happens here:

1. LLM Inference: The context + query gets fed into Mistral 7B, a state-of-the-art open-source large language model that:

* Understands nuanced questions
* Reasons across multiple retrieved chunks
* Generates coherent, contextually grounded answers
* Avoids hallucinations by staying anchored to provided context


2. Response Formatting: The model produces a natural language answer that:

* Directly addresses our question
* Cites relevant information from our documents
* Maintains conversational flow
* Can be followed up with additional questions


3. Conversation Memory: Previous exchanges are stored in session state, enabling multi-turn conversations where the bot remembers what you talked about.


## Tech Stack Breakdown
* Embeddings: MiniLM (sentence-transformers)-Fast, accurate, runs on CPU, perfect balance of speed/quality
* Vector Database: FAISS - Industry standard, Meta-backed, optimized for similarity search
* LLM: Mistral 7B - Powerful reasoning, open-source, runs locally, low resource usage
* Frontend: Gradio - Rapid prototyping, clean UI, zero frontend coding needed
* PDF Processing: PyPDF2 - Reliable text extraction, handles various PDF formats
* Orchestration: LangChain - Simplifies RAG pipeline, manages prompts, chains components

## Resource Requirements

Minimum: 8GB RAM, 4-core CPU
Recommended: 16GB RAM, 6+ core CPU (or GPU for faster inference)
Storage: ~4GB for models + PDF collection

## Future Enhancements & Roadmap
We're planning to add:

 * Multi-modal support (images, tables, charts in PDFs)
 * Advanced citation with page numbers and highlighting
 * Support for other document formats (Word, Markdown, HTML)
 * Conversation export and sharing
 * Fine-tuned ranking models for better retrieval
 * Hybrid search (combining semantic + keyword search)
 * User feedback loop for answer quality improvement
 * Web interface with authentication for team use
 * API endpoint for integration with other tools



