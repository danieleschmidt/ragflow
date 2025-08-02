# ADR-0001: Adopt Elasticsearch for Vector Storage with Infinity as Alternative

Date: 2025-01-26

## Status

Accepted

## Context

RAGFlow requires a vector database solution to store and search document embeddings efficiently. The system needs to handle:

1. Large-scale vector similarity search
2. Hybrid search (vector + keyword)
3. Metadata filtering and faceted search
4. High availability and horizontal scaling
5. Integration with existing search infrastructure

Key considerations:
- Performance at scale (millions of documents)
- Operational complexity and maintenance
- Development team expertise
- Integration with existing tech stack
- Cost and licensing

## Decision

We have decided to use **Elasticsearch** as the primary vector storage solution with **Infinity** as an optional high-performance alternative.

### Primary Choice: Elasticsearch
- Leverage Elasticsearch's vector search capabilities (available since 8.x)
- Unified solution for both vector and keyword search
- Rich ecosystem and tooling
- Team familiarity with Elasticsearch operations

### Alternative: Infinity
- Custom-built vector database optimized for performance
- Designed specifically for vector operations
- Better performance characteristics for pure vector search
- Developed by the same team (InfiniFlow)

## Consequences

### Positive
- **Unified Search**: Single system for vector and keyword search reduces complexity
- **Mature Ecosystem**: Extensive tooling, monitoring, and operational knowledge
- **Flexibility**: Easy to switch between Elasticsearch and Infinity based on needs
- **Hybrid Queries**: Native support for combining vector and keyword search
- **Scalability**: Proven horizontal scaling capabilities

### Negative
- **Performance Trade-off**: Elasticsearch may be slower than specialized vector databases for pure vector operations
- **Resource Usage**: Higher memory and CPU requirements compared to lightweight alternatives
- **Complexity**: Elasticsearch configuration and tuning requires expertise
- **Vendor Lock-in**: Some dependency on Elastic's ecosystem and licensing

### Neutral
- **Migration Path**: Clear upgrade path to Infinity for performance-critical deployments
- **Configuration**: Users can choose their preferred vector storage backend
- **Development**: Abstraction layer allows supporting multiple vector stores

## References

- [Elasticsearch Vector Search Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/knn-search.html)
- [Infinity Vector Database](https://github.com/infiniflow/infinity)
- [RAGFlow Vector Storage Configuration](../configurations.md)