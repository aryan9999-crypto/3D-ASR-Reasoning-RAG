<div align="center">

# 3D ASR - AI Maintenance Assistant

**5-Stage Reasoning RAG | Bilingual Voice Pipeline | Air-Gapped LLM Inference**

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-0467DF?style=flat-square&logo=meta&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

*An offline, air-gapped AI assistant for the 3D Air Surveillance Radar system - enabling hands-free field maintenance with voice interaction, step-by-step guided procedures, and automatic visual reference retrieval. Zero cloud dependency.*

</div>

---

## Pipeline Architecture

```
                         +-----------------+
                         |   User Query    |
                         |  (Voice / Text) |
                         +--------+--------+
                                  |
                    +-------------+--------------+
                    |                            |
              +-----v------+           +--------v--------+
              | Faster-    |           | 3-Layer         |
              | Whisper    |           | Bilingual       |
              | ASR        |           | Classifier      |
              |            |           | (EN/Hinglish)   |
              +-----+------+           +--------+--------+
                    +-------------+--------------+
                                  |
            +---------------------v---------------------+
            |         5-STAGE REASONING RAG             |
            +-------------------------------------------+
            |                                           |
            |  Stage 1: Query Decomposition             |
            |           Sub-question generation         |
            |                    |                      |
            |                    v                      |
            |  Stage 2: Multi-Pass FAISS Retrieval      |
            |           all-MiniLM-L6-v2 embeddings     |
            |                    |                      |
            |                    v                      |
            |  Stage 3: Context Grading                 |
            |           Heuristic + LLM scoring         |
            |                    |                      |
            |                    v                      |
            |  Stage 4: Reasoning Chain                 |
            |           Qwen2.5-14B (GGUF + CUDA)      |
            |                    |                      |
            |                    v                      |
            |  Stage 5: Self-Verification               |
            |           Completeness check              |
            |                                           |
            +---------------------+---------------------+
                                  |
                    +-------------v--------------+
                    |                            |
              +-----v------+           +--------v--------+
              | SSE Token  |           | Visual          |
              | Streaming  |           | References      |
              | Response   |           | (Auto-Retrieved)|
              +------------+           +-----------------+
```

---

## Key Features

### 5-Stage Reasoning RAG Pipeline
Unlike naive RAG (retrieve -> generate), this system implements a **closed-loop reasoning engine**:
1. **Query Decomposition** - Breaks complex questions into targeted sub-queries
2. 2. **Multi-Pass FAISS Retrieval** - Semantic search with `all-MiniLM-L6-v2` embeddings across 11 radar subsystem documentation sets
   3. 3. **Context Grading** - Heuristic + LLM-based relevance scoring with 3-tier classification (Relevant / Partial / Irrelevant)
      4. 4. **Reasoning Chain** - Qwen2.5-14B-Instruct (Q4_K_M GGUF) with CUDA offloading generates grounded answers
         5. 5. **Self-Verification** - Completeness validation with automatic retry on insufficient responses
           
            6. ### Bilingual Voice Pipeline
            7. - **Faster-Whisper ASR** with dual-pass transcription (auto-detect -> re-transcribe for Hinglish)
               - - **3-Layer language classifier**: 120+ keyword dictionary, suffix pattern matching, bigram analysis
                 - - **Cross-lingual FAISS retrieval**: Hinglish queries automatically translated to English for semantic search
                   - - **Language-aware TTS response routing**
                    
                     - ### Interactive Guided Maintenance
                     - - **Semantic procedural intent detection** via cosine similarity over anchor embeddings
                       - - **Single-step extraction** with operator confirmation flow
                         - - **Completion guards** preventing premature advancement
                           - - **Automatic image retrieval** with visual references for each maintenance step
                            
                             - ### Performance
                             - - **Turbo Mode**: ~0.5s time-to-first-token for simple queries
                               - - **Full Pipeline**: Complete 5-stage reasoning for complex technical questions
                                 - - **Thread-safe inference**: Concurrent request handling with LLM locking
                                   - - **Response caching**: Language-aware cache keys for repeat queries
                                    
                                     - ---

                                     ## Tech Stack

                                     | Component | Technology |
                                     |---|---|
                                     | LLM Inference | llama-cpp-python (GGUF + CUDA) |
                                     | Embedding Model | all-MiniLM-L6-v2 (SentenceTransformers) |
                                     | Vector Store | FAISS |
                                     | Speech-to-Text | Faster-Whisper |
                                     | Text-to-Speech | pyttsx3 |
                                     | API Server | FastAPI (SSE streaming) |
                                     | Frontend | HTML/CSS/JavaScript |

                                     ---

                                     ## Project Structure

                                     ```
                                     3D-ASR-Reasoning-RAG/
                                     +- reasoning_rag/
                                     |  +- query_decomposer.py    # Stage 1: Sub-question generation
                                     |  +- retriever.py           # Stage 2: Multi-pass FAISS retrieval
                                     |  +- context_grader.py      # Stage 3: Relevance scoring
                                     |  +- reasoning_chain.py     # Stage 4: LLM reasoning with visual term extraction
                                     |  +- self_verifier.py       # Stage 5: Response completeness check
                                     +- modules/
                                     |  +- keyword_manager.py     # Hinglish keyword dictionary management
                                     |  +- language_detector.py   # 3-layer bilingual classification
                                     +- frontend/
                                     |  +- index.html             # Web interface
                                     |  +- styles.css             # UI styling
                                     +- server.py                  # FastAPI server with SSE streaming
                                     +- offline_chatbot.py         # Core chatbot logic + procedural engine
                                     +- config.py                  # Centralized configuration
                                     +- requirements.txt
                                     ```

                                     ---

                                     ## Quick Start

                                     ```bash
                                     # Clone the repository
                                     git clone https://github.com/aryan9999-crypto/3D-ASR-Reasoning-RAG.git
                                     cd 3D-ASR-Reasoning-RAG

                                     # Install dependencies
                                     pip install -r requirements.txt

                                     # Start the server
                                     python server.py

                                     # Access the interface
                                     # Open http://localhost:8000 in your browser
                                     ```

                                     ---

                                     ## Supported Radar Subsystems

                                     | Code | Subsystem |
                                     |---|---|
                                     | PCR | Power Combiner Receiver |
                                     | TCU | Transmitter Control Unit |
                                     | AAG | Anti-Aircraft Gun |
                                     | CPR | Central Processor |
                                     | EDR | Exciter Driver |
                                     | + 6 more | Full coverage across 11 subsystems |

                                     ---

                                     ## How It Works

                                     1. **Input** -> User speaks or types a question (English or Hinglish)
                                     2. 2. **Detect** -> 3-layer classifier identifies language; Whisper transcribes voice
                                        3. 3. **Decompose** -> Complex queries split into focused sub-questions
                                           4. 4. **Retrieve** -> FAISS searches across all subsystem documentation
                                              5. 5. **Grade** -> Each retrieved chunk scored for relevance (3-tier)
                                                 6. 6. **Reason** -> Qwen2.5-14B generates grounded answer from top chunks
                                                    7. 7. **Verify** -> Self-verification ensures completeness; retries if needed
                                                       8. 8. **Stream** -> Response streamed token-by-token via SSE with visual references
                                                         
                                                          9. ---
                                                         
                                                          10. <div align="center">

                                                          **Fully Offline | Air-Gapped | Zero Cloud Dependency**

                                                          Built at [EdgeForce Solutions](https://www.linkedin.com/in/aryan-gaur-ai/)

                                                          </div>
