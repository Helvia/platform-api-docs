# Architecture

Visual overview of the Helvia.ai Platform service topology, showing how services communicate and what infrastructure they depend on.

## Component Diagram

```mermaid
graph TB
    subgraph "Frontend"
        console["hbf-console<br/><i>React SPA</i>"]
    end

    subgraph "Core Services"
        core["hbf-core<br/><i>Java/Spring Boot</i>"]
        bot["hbf-bot<br/><i>TypeScript/Express</i>"]
        nlp["hbf-nlp<br/><i>TypeScript/NestJS</i>"]
    end

    subgraph "Supporting Services"
        sm["hbf-session-manager<br/><i>TypeScript/NestJS</i>"]
        lcm["hbf-lcm<br/><i>Livechat Manager</i>"]
        rag["helvia-rag-pipelines<br/><i>Python/FastAPI</i>"]
        seg["semantic-doc-segmenter<br/><i>Python/FastAPI</i>"]
    end

    subgraph "Data & Analytics Services"
        dm["hbf-data-manager<br/><i>Data & Exports</i>"]
        mm["hbf-media-manager<br/><i>Media Files</i>"]
        reports["hbf-reports<br/><i>Analytics</i>"]
    end

    subgraph "Infrastructure"
        mongo[("MongoDB<br/><i>replica set</i>")]
        redis[("Redis")]
        mysql[("MySQL")]
        qdrant[("Qdrant<br/><i>vector DB</i>")]
    end

    subgraph "External Services"
        azure["Azure OpenAI"]
        openai["OpenAI API"]
        gemini["Google Gemini"]
        livechat["Livechat Providers<br/><i>Zendesk, Cisco, Genesys</i>"]
        channels["Chat Channels<br/><i>Web, Viber, Messenger,<br/>Slack, Teams</i>"]
    end

    %% Frontend to Core
    console -->|"REST API"| core

    %% Bot interactions
    bot -->|"REST — tenant configs"| core
    bot -->|"REST — NLP processing"| nlp
    bot -->|"session cache"| redis
    bot -->|"livechat handover"| lcm
    channels -->|"webhooks / WebSocket"| bot

    %% Core interactions
    core --> mongo
    core --> redis
    core -->|"REST — semantic search"| rag

    %% NLP interactions
    nlp -->|"REST — tenant configs"| core
    nlp --> mysql
    nlp --> azure
    nlp --> openai
    nlp --> gemini

    %% Session Manager
    sm -->|"REST — session data"| core
    sm -->|"REST — pipeline checks"| nlp
    sm --> mysql

    %% RAG Pipelines
    rag --> mysql
    rag --> qdrant
    rag --> openai

    %% Document Segmenter
    seg --> mysql
    seg --> openai
    seg --> gemini

    %% Livechat Manager
    lcm --> livechat
    lcm --> mysql

    %% Data & Analytics
    dm -->|"REST"| core
    dm --> mysql
    mm --> mysql
    reports -->|"REST"| core
    reports --> mysql
```

## Service Roles

| Service | Role |
|---------|------|
| **hbf-core** | Central backend — single source of truth for all platform configuration and data |
| **hbf-console** | Frontend console — visual no-code editor for organizations, agents, and flows |
| **hbf-bot** | Chatbot executor — runs agent configurations in real-time chat channels |
| **hbf-nlp** | NLP/LLM processing — intent classification, text generation, pipeline execution |
| **hbf-session-manager** | Session lifecycle — inactivity detection, completion triggers, auto-retraining |
| **hbf-lcm** | Livechat routing — human agent handover via Zendesk, Cisco, Genesys, etc. |
| **helvia-rag-pipelines** | RAG & semantic search — embeddings, vector indexing, retrieval |
| **semantic-doc-segmenter** | Document processing — ingestion, markdown conversion, chunking, tagging |
| **hbf-data-manager** | Data operations — bulk exports and data processing for analytics |
| **hbf-media-manager** | Media management — file upload, storage, and retrieval |
| **hbf-reports** | Analytics — aggregated reports on conversations, agents, and engagement |
