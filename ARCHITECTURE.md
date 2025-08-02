# RAGFlow Architecture

## System Overview

RAGFlow is an open-source RAG (Retrieval-Augmented Generation) engine designed to provide truthful question-answering capabilities through deep document understanding. The system combines LLMs with sophisticated document processing and retrieval mechanisms.

## High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Web Frontend  │    │   API Gateway   │    │  Backend Core   │
│   (React/TS)    │◄──►│   (Flask)       │◄──►│   (Python)      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │              ┌─────────────────┐              │
         └──────────────►│  Authentication │◄─────────────┘
                        │   & Session     │
                        └─────────────────┘
                                 │
        ┌────────────────────────┼────────────────────────┐
        │                       │                        │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Document      │    │   Vector Store  │    │   LLM Services  │
│   Processing    │    │ (Elasticsearch/ │    │   (Multiple     │
│   (DeepDoc)     │    │   Infinity)     │    │   Providers)    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │              ┌─────────────────┐              │
         └──────────────►│   Data Storage  │◄─────────────┘
                        │ (MySQL, MinIO,  │
                        │   Redis)        │
                        └─────────────────┘
```

## Core Components

### 1. Web Frontend (`/web`)
- **Technology**: React with TypeScript, Ant Design UI
- **Responsibilities**: User interface, file management, chat interface, configuration
- **Key Features**: Document viewer, knowledge base management, agent configuration

### 2. API Layer (`/api`)
- **Technology**: Flask with RESTful APIs
- **Components**:
  - `apps/`: Application-specific endpoints
  - `db/`: Database models and services
  - `utils/`: Utility functions and helpers
- **Authentication**: JWT-based session management

### 3. Document Processing (`/deepdoc`)
- **Technology**: Python with OCR, layout recognition
- **Capabilities**:
  - Multi-format parsing (PDF, DOCX, Excel, HTML, etc.)
  - Layout recognition and table structure detection
  - Resume parsing with entity extraction
  - Vision-based document understanding

### 4. RAG Engine (`/rag`)
- **Technology**: Python with ML frameworks
- **Components**:
  - `app/`: Document type-specific processors
  - `llm/`: LLM abstraction layer
  - `nlp/`: Natural language processing utilities
  - `utils/`: Storage and connection utilities

### 5. Agent System (`/agent`)
- **Technology**: Python with DSL for workflow definition
- **Features**: Configurable agent templates, component-based workflows

### 6. SDK (`/sdk`)
- **Technology**: Python SDK for programmatic access
- **Capabilities**: Complete API access for integration

## Data Flow

### Document Ingestion
1. **Upload** → Frontend file upload interface
2. **Storage** → MinIO object storage
3. **Processing** → DeepDoc parsing and extraction
4. **Chunking** → Template-based intelligent chunking
5. **Embedding** → Vector generation via embedding models
6. **Indexing** → Storage in Elasticsearch/Infinity

### Query Processing
1. **Query** → User question via chat interface
2. **Retrieval** → Vector similarity search + keyword search
3. **Reranking** → Result fusion and reranking
4. **Generation** → LLM-based answer generation with citations
5. **Response** → Formatted answer with source references

## Storage Systems

### Database (MySQL)
- User accounts and permissions
- Knowledge base metadata
- Chat history and sessions
- System configurations

### Object Storage (MinIO)
- Original document files
- Processed document artifacts
- System backups and exports

### Vector Database (Elasticsearch/Infinity)
- Document embeddings
- Full-text search indices
- Metadata and chunk information

### Cache (Redis)
- Session data
- Temporary processing states
- API rate limiting

## Deployment Architecture

### Container-Based Deployment
```
┌─────────────────────────────────────────────────────────────┐
│                     Docker Compose                         │
├─────────────────┬─────────────────┬─────────────────────────┤
│   ragflow-web   │  ragflow-api    │    ragflow-task         │
│   (Frontend)    │  (Backend)      │    (Background Jobs)    │
├─────────────────┼─────────────────┼─────────────────────────┤
│   nginx         │  elasticsearch  │    mysql                │
│   (Proxy)       │  (Search/Vector)│    (Database)           │
├─────────────────┼─────────────────┼─────────────────────────┤
│   minio         │  redis          │    infinity (optional)  │
│   (Storage)     │  (Cache)        │    (Vector DB)          │
└─────────────────┴─────────────────┴─────────────────────────┘
```

### Scaling Considerations
- **Horizontal**: Multiple API/task worker instances
- **Vertical**: GPU acceleration for embedding/LLM inference
- **Storage**: Distributed storage for large document collections
- **Caching**: Redis clustering for high availability

## Security Architecture

### Authentication & Authorization
- JWT-based session management
- Role-based access control
- API key management for integrations

### Data Protection
- Encryption at rest (MinIO/database)
- TLS encryption in transit
- Input validation and sanitization

### Privacy
- Local deployment option
- Data residency controls
- Audit logging capabilities

## Integration Points

### LLM Providers
- OpenAI, Anthropic, Google Gemini
- Azure OpenAI, AWS Bedrock
- Local models via Ollama, Xinference
- Custom API endpoints

### Embedding Models
- Sentence Transformers
- OpenAI embeddings
- Local models via HuggingFace
- Custom embedding services

### External APIs
- Search: Bing, Google, DuckDuckGo
- Translation: DeepL, Baidu
- Financial: Tushare, Yahoo Finance
- Academic: arXiv, PubMed

## Performance Characteristics

### Throughput
- **Document Processing**: 1-100 docs/minute (depends on size/complexity)
- **Query Response**: <2 seconds for typical queries
- **Concurrent Users**: 100+ with proper scaling

### Resource Requirements
- **Minimum**: 4 CPU cores, 16GB RAM, 50GB storage
- **Recommended**: 8+ CPU cores, 32GB+ RAM, 200GB+ storage
- **GPU**: Optional for local LLM/embedding inference

## Monitoring & Observability

### Metrics
- API response times and error rates
- Document processing throughput
- Vector search performance
- Resource utilization

### Logging
- Structured application logs
- Audit trails for data access
- Error tracking and alerting

### Health Checks
- Service availability monitoring
- Database connection health
- External API availability