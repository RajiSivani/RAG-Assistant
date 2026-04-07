# National Park RAG Assistant

A fully **offline** Retrieval-Augmented Generation (RAG) chatbot for U.S. National Park information — no internet required at query time.

Built as part of the Agentic AI course at UT Dallas (BUAN 6V99).

---

## What it does

Ask any question about U.S. National Parks and get grounded, accurate answers from a local knowledge base — no hallucinations, no cloud dependency.

Example queries:
- *"What wildlife can I see in Yellowstone?"*
- *"Is Zion National Park suitable for beginners?"*
- *"What are the entry fees for Grand Canyon?"*

---

## Tech Stack

| Component | Tool |
|---|---|
| LLM (local inference) | Ollama + Gemma 2B |
| Embeddings | nomic-embed-text (via Ollama) |
| Vector store | ChromaDB (persistent, local) |
| Knowledge base | 63 U.S. National Park `.txt` documents |
| Interface | Jupyter Notebook with conversational history |

---

## Architecture

```
documents/*.txt
      ↓
  Text Chunker (150-word chunks, 15-word overlap)
      ↓
  ChromaDB (cosine similarity vector store)
      ↓
  User Query → nomic-embed-text → top-7 chunk retrieval
      ↓
  Gemma 2B (via Ollama) → grounded response
```

---

## How to Run

### Prerequisites

1. Install [Ollama](https://ollama.com) and pull the required models:
```bash
ollama pull gemma2:2b
ollama pull nomic-embed-text
```

2. Install Python dependencies:
```bash
pip install ollama chromadb
```

### Setup

1. Clone this repo:
```bash
git clone https://github.com/RajiSivani/RAG-Assistant.git
cd RAG-Assistant
```

2. Update the paths in the `CONFIG` block inside the notebook to point to your local `documents/` and a `chroma_db/` folder.

3. Open and run the notebook:
```bash
jupyter notebook "Lion king National park RAG.ipynb"
```

Run cells in order — the notebook will load documents, chunk them, index into ChromaDB, and start the chat interface.

---

## Knowledge Base

63 U.S. National Parks covered, including Yellowstone, Yosemite, Grand Canyon, Zion, Denali, Everglades, and more. Each park has a dedicated `.txt` file in the `documents/` folder.

---

## Key Design Decisions

- **100% local** — Ollama runs the LLM and embeddings on your machine; no API keys or internet needed at query time
- **Persistent ChromaDB** — vectors are stored on disk; no re-indexing needed after the first run
- **Conversation memory** — retains last 6 exchanges for contextual follow-up questions
- **Cosine similarity retrieval** — retrieves top-7 most relevant chunks per query
