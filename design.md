# Project Name: Vanguard_Cosmos (The Digital Vishvakarma)

## 1. System Architecture
Vanguard_Cosmos follows a **Microservices Architecture** tailored for scalability and low-latency interaction.

### Core Components:
1.  **Frontend Client (React + Vite):** -   A lightweight, responsive UI designed for mobile and desktop (critical for Tier-2/3 access).
    -   Host: AWS Amplify / Vercel.
2.  **API Gateway (FastAPI):** -   Manages incoming requests, rate limiting, and routing to the ingestion engine.
    -   Host: AWS App Runner / Google Cloud Run.
3.  **The "Odin" Ingestion Engine:**
    -   **Cloner:** Fetches public Git repositories.
    -   **Parser:** Uses `tree-sitter` to generate Abstract Syntax Trees (AST) of the code.
    -   **Chunker:** Intelligent splitting of code based on function/class boundaries (not just arbitrary text).
4.  **Vector Database (Knowledge Store):**
    -   Stores code embeddings for semantic search.
    -   Technology: ChromaDB (Local/Dev) -> Amazon OpenSearch (Prod).
5.  **LLM Orchestrator (The Brain):**
    -   **Model:** Amazon Bedrock (Claude 3 Haiku / Llama 3) for high-speed code explanation.
    -   **Translation Layer:** AWS Translate / AI4Bharat IndicTrans API to convert technical explanations into Hindi, Tamil, Telugu, etc.

## 2. Data Flow
1.  **User Input:** User pastes a GitHub URL (e.g., `https://github.com/fastapi/fastapi`).
2.  **Ingestion:** The system clones the repo to a temporary storage container.
3.  **Analysis:** The `Ingestor` service maps the file structure and creates a "Dependency Graph."
4.  **Interaction:** -   User clicks a file (e.g., `main.py`).
    -   System retrieves the code + context.
    -   LLM generates a summary: *"This file is the entry point. It initializes the app..."*
    -   **Localization:** If "Hindi" is selected, the output is translated before reaching the frontend.

## 3. Technology Stack
* **Frontend:** React, Tailwind CSS, Lucide Icons.
* **Backend:** Python 3.11, FastAPI.
* **AI/ML:** LangChain, Amazon Bedrock (Concept), Sentence-Transformers.
* **DevOps:** Docker, GitHub Actions.

## 4. Scalability & Security
* **Ephemeral Storage:** Repo files are deleted after session expiry to ensure security.
* **Caching:** Redis cache for frequently accessed repositories to reduce API costs.