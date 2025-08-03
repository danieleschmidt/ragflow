# ADR-0003: Multi-Provider LLM Integration Architecture

## Status
Accepted

## Context
RAGFlow needs to integrate with multiple Large Language Model (LLM) providers to offer flexibility and avoid vendor lock-in. The system must support:
- Multiple LLM providers (OpenAI, Anthropic, Google, Azure, local models)
- Different model capabilities (chat, embedding, reranking)
- Fallback mechanisms for reliability
- Cost optimization through intelligent routing
- Privacy considerations for sensitive data

## Decision
We will implement a unified LLM abstraction layer with the following design:

1. **Provider Abstraction**:
   ```python
   BaseLLMProvider
   ├── OpenAIProvider
   ├── AnthropicProvider
   ├── GoogleProvider
   ├── AzureProvider
   ├── OllamaProvider (local)
   └── CustomProvider
   ```

2. **Model Types**:
   - Chat Models: For conversational interactions
   - Embedding Models: For vector generation
   - Reranking Models: For result optimization
   - Vision Models: For image understanding
   - Audio Models: For speech processing

3. **Request Router**:
   - Load balancing across providers
   - Automatic fallback on failures
   - Cost-based routing optimization
   - Latency-based selection
   - Privacy-aware routing for sensitive data

4. **Configuration Management**:
   ```yaml
   llm_providers:
     primary:
       provider: openai
       model: gpt-4
       api_key: ${OPENAI_API_KEY}
     fallback:
       provider: anthropic
       model: claude-3
     local:
       provider: ollama
       model: llama3
   ```

## Consequences

### Positive
- Provider independence and flexibility
- Improved reliability through fallbacks
- Cost optimization capabilities
- Support for on-premise deployments
- Easy addition of new providers

### Negative
- Increased complexity in request handling
- Need for provider-specific adaptations
- Potential inconsistencies across models
- Higher maintenance overhead

### Neutral
- Requirement for comprehensive provider testing
- Need for unified error handling
- Performance monitoring across providers

## Implementation Details

1. **Unified Interface**:
   - Standard request/response format
   - Provider-agnostic prompt templates
   - Consistent token counting
   - Unified streaming support

2. **Error Handling**:
   - Automatic retry with exponential backoff
   - Graceful degradation to fallback providers
   - Circuit breaker pattern for failing providers
   - Comprehensive error logging

3. **Performance Optimization**:
   - Response caching for identical queries
   - Batch processing where supported
   - Connection pooling for API calls
   - Rate limiting compliance

4. **Security Measures**:
   - API key encryption at rest
   - Request sanitization
   - PII detection and masking
   - Audit logging for compliance

## Migration Path
1. Implement base abstraction layer
2. Migrate existing OpenAI integration
3. Add new providers incrementally
4. Implement routing logic
5. Add monitoring and analytics

## References
- LangChain LLM abstractions
- OpenAI API documentation
- Anthropic Claude API
- Google Vertex AI documentation
- Azure OpenAI Service