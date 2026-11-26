# Advanced Multilingual RAG System 🌍🤖

### Retrieval-Augmented Generation for English, French, and Arabic Documents

An advanced **Retrieval-Augmented Generation (RAG)** system built with Python and Google Colab. It allows users to upload PDF documents and ask questions in **English, French, or Arabic**, receiving context-aware answers grounded in the uploaded documents.

The system combines multilingual semantic search, keyword-based retrieval, hybrid ranking, cross-encoder reranking, and a Large Language Model (LLM) to improve the relevance and accuracy of generated answers.

---

## 🚀 Project Overview

Traditional RAG systems often rely only on semantic similarity to retrieve relevant documents. This project improves the retrieval pipeline by combining multiple techniques.

The system follows these main steps:

1. Extract text from PDF documents.
2. Split extracted text into manageable chunks.
3. Generate multilingual embeddings using BGE-M3.
4. Build a FAISS vector index for semantic search.
5. Apply BM25 for keyword-based retrieval.
6. Combine retrieval results using Reciprocal Rank Fusion (RRF).
7. Rerank retrieved passages using a multilingual Cross-Encoder.
8. Generate grounded answers using Qwen 2.5 3B Instruct.
9. Display answers and document references through a Gradio interface.

## 🏗️ System Architecture

```text
             PDF Documents
                   |
                   v
          PDF Text Extraction
              (PyMuPDF)
                   |
                   v
          Text Preprocessing
                   |
                   v
          Intelligent Chunking
                   |
                   v
          Multilingual Embeddings
                (BGE-M3)
                   |
             +-----+-----+
             |           |
             v           v
        FAISS Search   BM25 Search
        (Semantic)     (Keywords)
             |           |
             +-----+-----+
                   |
                   v
       Reciprocal Rank Fusion
                  (RRF)
                   |
                   v
        Cross-Encoder Reranking
                   |
                   v
          Relevant Context
                   |
                   v
        Qwen 2.5 3B Instruct
                   |
                   v
          Grounded Answer
                   |
                   v
           Gradio Interface
```

## ✨ Main Features

- **Multilingual support:** Ask questions in English, French, and Arabic.
- **PDF document processing:** Extract text from multiple PDF files.
- **Structure-aware chunking:** Split documents using paragraph and sentence boundaries.
- **Semantic retrieval:** Understand the meaning of user questions using BGE-M3 embeddings.
- **Keyword retrieval:** Retrieve passages using BM25.
- **Hybrid search:** Combine semantic and lexical retrieval using RRF.
- **Cross-Encoder reranking:** Improve the ordering of retrieved passages.
- **Query rewriting:** Reformulate follow-up questions using conversation history.
- **Grounded generation:** Generate answers based on retrieved document evidence.
- **Source references:** Display the original PDF filename and page number.
- **Interactive interface:** Ask questions through a Gradio chat interface.

## 🛠️ Technologies and Models

| Component | Technology |
|---|---|
| Programming language | Python |
| Environment | Google Colab |
| GPU | NVIDIA T4 (recommended) |
| PDF extraction | PyMuPDF |
| Embeddings | BAAI/bge-m3 |
| Vector database | FAISS |
| Keyword retrieval | BM25 |
| Ranking fusion | Reciprocal Rank Fusion |
| Reranking | BAAI/bge-reranker-v2-m3 |
| Language model | Qwen/Qwen2.5-3B-Instruct |
| Quantization | 4-bit (BitsAndBytes) |
| User interface | Gradio |

## ⚙️ Installation and Execution

### 1. Download the Notebook

Download the notebook:

`Advanced_Multilingual_RAG_One_Cell.ipynb`

### 2. Open Google Colab

Open [Google Colab](https://colab.research.google.com/) and upload the downloaded notebook.

### 3. Enable GPU

Navigate to:

`Runtime → Change runtime type → T4 GPU`

A GPU is recommended for faster model loading, embedding generation, reranking, and text generation.

### 4. Execute the Notebook

The project is implemented in **one executable code cell**.

Run the cell. It will:

- Install the required Python libraries.
- Ask you to upload PDF documents.
- Extract and process document text.
- Generate embeddings and build the retrieval indexes.
- Load the reranker and language model.
- Initialize the RAG pipeline.
- Launch the Gradio interface.

### 5. Ask Questions

Once the interface is running, ask questions about your uploaded documents in English, French, or Arabic.

Example questions:

**English:**
- What are the main concepts discussed in this document?
- Explain the advantages of the proposed method.

**French:**
- Quels sont les principaux objectifs de ce document ?
- Expliquez les avantages de cette méthode.

**Arabic:**
- ما هي الأفكار الرئيسية في هذا المستند؟
- اشرح أهم مزايا هذه الطريقة.

## 📂 Project Structure

```text
Advanced-Multilingual-RAG/
│
├── Advanced_Multilingual_RAG_One_Cell.ipynb
│
├── README.md
│
└── documents/                 # PDF documents uploaded at runtime
```

The notebook also generates intermediate files in the Colab environment:

- `rag_chunks.json` — extracted and processed text chunks.
- `rag_faiss.index` — FAISS vector index.

These files are generated during execution and are not automatically included in the GitHub repository.

## 🔍 Retrieval Strategy

The system uses a hybrid retrieval approach.

**1. Dense Retrieval — BGE-M3**

Converts document chunks and user questions into multilingual vector representations. This helps retrieve passages based on semantic meaning rather than exact word matching.

**2. Sparse Retrieval — BM25**

Uses keyword matching to retrieve passages containing important terms, technical expressions, and identifiers.

**3. Reciprocal Rank Fusion — RRF**

Combines the rankings produced by dense retrieval and BM25 to create a unified candidate list.

**4. Cross-Encoder Reranking**

Evaluates the relevance between the question and retrieved passages to improve the final ranking.

## 🧠 Answer Generation

The final retrieved passages are provided to **Qwen 2.5 3B Instruct**.

The generation prompt instructs the model to:

- Answer in the same language as the user's question.
- Use the retrieved document context for factual claims.
- Include source references such as `[S1]` and `[S2]`.
- Avoid inventing information.
- Indicate when the available context is insufficient.

## ⚠️ Current Limitations

- Scanned PDFs and image-only pages require OCR, which is not implemented in the current version.
- Answer quality depends on document quality and retrieval relevance.
- The system requires an internet connection to install dependencies and download models.
- GPU execution is recommended; CPU execution may be significantly slower.
- Conversation history is temporary and is not preserved after a Colab runtime reset.
- Source citations identify the retrieved PDF and page, but do not guarantee that every generated claim is correct.
- The current implementation does not include a formal RAG evaluation benchmark.

## 🔮 Future Improvements

Possible future developments include:

- OCR integration for scanned PDF documents.
- Semantic chunking and document structure detection.
- Query decomposition for complex questions.
- Metadata filtering and multi-document comparison.
- Improved Arabic text normalization.
- Retrieval evaluation using Recall@K, MRR, and nDCG.
- Answer evaluation using RAGAS.
- Persistent vector storage and conversation memory.
- Support for additional document formats.

## 🎯 Project Objective

The objective of this project is to develop a more capable multilingual RAG system that improves document retrieval and question answering by combining semantic search, keyword retrieval, ranking fusion, reranking, and grounded language generation.

It demonstrates how modern NLP and Large Language Models can be integrated into a practical document question-answering application.

## 👨‍💻 Author

**Your Name**

Computer Science / Artificial Intelligence

## 📜 License

This project is intended for educational and research purposes. Check the licenses of the individual models and dependencies before redistribution or commercial use.
