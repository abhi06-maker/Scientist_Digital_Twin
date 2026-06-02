# Scientist_Digital_Twin

#  Richard Feynman Digital Twin — Premium AI Agent

An advanced, highly interactive, and visually stunning digital twin of the Nobel Laureate physicist, **Richard P. Feynman**. This project emulates Feynman's scientific reasoning, signature speech patterns (the "Feynman Technique"), boyish personality, and physics knowledge through a state-of-the-art RAG pipeline, stateful dual-tier memory, dynamic voice transcription/synthesis, and an ultra-premium glassmorphic web dashboard.

---

##  Web Dashboard Preview

The web interface is designed with a state-of-the-art **Glassmorphism Design System** featuring HSL-curated dark mode themes, custom chat containers with glowing cyan/gold border accents, a live expanding **Memory Dashboard**, interactive milestones, and an integrated sidebar session switcher.

---

##  Key Technical Capabilities

### 1.  Multimodal Voice Interaction (Speak & Hear)
*   **Speech-to-Text**: High-fidelity browser recording. It dynamically extracts native browser MIME-types (such as `audio/webm` or `audio/ogg`) from metadata and pipes them directly to the Gemini transcription pipeline.
*   **Text-to-Speech Autoplay**: Fully integrated Google Text-to-Speech (gTTS) audio engine. It caches generated speech bytes inside `st.session_state` to prevent instant page-rerun destruction, rendering a background autoplay player hidden from the UI (`display: none;`) to keep the dashboard clutter-free.
*   **API Rate Limit Protection**: Wrapped in a robust try-except rollback system that popping the user's message from UI state on API failures (like daily/minute quotas - `429 You exceeded your current quota`), preventing automatic background loops.

### 2.  Stateful Dual-Tier Memory System (`memory.py`)
*   **Short-Term Memory**: Tracks the active chat session's rolling back-and-forth conversational turns (up to 20 turns) for in-session contextual coherence.
*   **Long-Term Memory Consolidation**: Powered by Gemini 2.5 Flash. Rather than simple regex, an LLM-driven extractor evaluates user input and Feynman's response dynamically at the end of each turn, learning:
    *   **User Name & Biography**: Extracts name and professional details (e.g., engineering student context) with de-duplication.
    *   **Explored Topics**: Lists and merges discussed subject fields (e.g., Quantum Mechanics, Los Alamos, safe cracking).
    *   **Memorable Exchange Log**: Summarizes previous conversations into one-sentence timelines so Feynman can reference previous complex discussions even across multiple sessions.
    *   **Data Layout**: Persisted locally under `memory_store/long_term_memory.json`.

### 3.  Knowledge-Grounded RAG Pipeline (`rag.py`)
*   Semantic chunking and text vectorization using `sentence-transformers` (`all-MiniLM-L6-v2`).
*   Local high-performance vector index storage (ChromaDB) ensuring Feynman's answers are anchored directly in his authentic lectures and biography details, eliminating hallucinations.

### 4.  Chronological Timeline Awareness (`persona.py`)
*   Feynman is equipped with post-1988 timeline instructions. The agent is aware of modern 2026 technological breakthroughs (Quantum Computer qubits, LIGO Gravitational waves, LLMs like ChatGPT, Higgs Boson) and naturally relates them to QED and early 1980s computational lecture theories.

---

##  Repository File Structure

```text
feynman-twin/
├── memory_store/               # Auto-generated stateful JSON memory directory
├── sources/                    # PDF/text knowledge base sources for RAG ingestion
├── .env.example                # Documented template for private environment variables [NEW]
├── .gitignore                  # Industry-standard git rules (excludes venvs, databases, cache) [NEW]
├── app.py                      # Glassmorphic Streamlit Dashboard & Playback Engine
├── ingest.py                   # ChromaDB data loader & embedding pipeline
├── main.py                     # Agent Core Orchestrator & Voice transcription wrapper
├── memory.py                   # Dual-tier Memory Engine & LLM consolidation routine
├── persona.py                  # Feynman Prompt Persona Guidelines & Timeline Awareness
├── rag.py                      # ChromaDB connection & semantic retrieval helper
└── requirements.txt            # Project python dependencies
```

---

##  Quick Start & Reproducibility Guide

### 1. Clone & Set Up Directory
```bash
git clone <your-repository-url>
cd feynman-twin
```

### 2. Configure Virtual Environment
```bash
# Create Virtual Environment
python -m venv .venv

# Activate on Windows (PowerShell)
.venv\Scripts\activate

# Activate on Mac/Linux
source .venv/bin/activate

# Install required dependencies
pip install -r requirements.txt
```

### 3. Populate Knowledge Base (RAG)
Put any of these freely and legally available PDFs or text files inside the `sources/` folder:
*   Feynman Lectures Vol. 1 excerpts (feynmanlectures.caltech.edu)
*   "There's Plenty of Room at the Bottom" (Feynman's famous nanotechnology paper)
*   1965 Nobel Prize Lecture transcript (nobelprize.org)

Run the ingestion script to create the local vector database:
```bash
python ingest.py
```

### 4. Launch the Digital Twin

*   **To Run the Premium Web App (Recommended)**:
    ```bash
    streamlit run app.py
    ```
    Once running, open `http://localhost:8501` in your browser. Ensure your speakers are turned on for autoplay audio responses!
    
*   **To Run the Terminal CLI version**:
    ```bash
    python main.py
    ```

---

## 🧪 System Architecture

```text
       [ Speak Input (Mic WebM/OGG) ]      [ Text Input (Type) ]
                     │                              │
                     ▼                              ▼
             STT Transcription              st.chat_input
                     │                              │
                     └──────────────┬───────────────┘
                                    │
                                    ▼
                          Agent Core (main.py)
                                    │
       ┌────────────────────────────┼────────────────────────────┐
       ▼                            ▼                            ▼
  RAG Retrieval               Active Context            Long-Term Memory
  (ChromaDB + Cosine)     (Short-term History)     (LLM Memory Consolidation)
       │                            │                            │
       └────────────────────────────┼────────────────────────────┘
                                    │
                                    ▼
                         Gemini 2.5 Flash API
                                    │
                                    ▼
                        Feynman Reply & gTTS
                                    │
                 ┌──────────────────┴──────────────────┐
                 ▼                                     ▼
      [ HTML Custom bubbles ]                [ Autoplay Audio bytes ]
```

---

## 🎨 Premium Design System Details

*   **Colors**: custom HSL themed slate backgrounds (`#0b0d12`), glowing cyan borders (`#06B6D4`) for user indicators, glowing radioactive gold accents (`#FFB703`) for Feynman's persona.
*   **Typography**: Clean sans-serif pairings using **Space Grotesk** for headings and **Inter** for conversational chat texts.
*   **Micro-Animations**: Hover animations on panels, tag badges, and custom integrated circular microphone controls overlaying the bottom chat input box.
