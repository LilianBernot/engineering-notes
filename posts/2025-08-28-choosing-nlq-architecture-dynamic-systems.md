# Choosing a Natural Language Query Architecture for Dynamic Data Systems

**Date:** 2025-08-28
**Tags:** [AI/ML, Architecture, NLQ, MCP, LLM, Design Decisions]
**Status:** Published

---

## Introduction

When building a Natural Language Query (NLQ) interface for a complex, evolving data catalog, the choice of architecture can make or break your solution. This post explores the architectural decisions faced when implementing NLQ for a system with dynamic schemas, frequent changes, and complex query requirements.

## The Challenge: Dynamic Data Systems

Our data architecture presented several unique challenges:

- **Atomicity**: Data organized by resource types, with many type-specific fields
- **Dynamic Evolution**: Rapid schema changes (50 → 140 tables in one year)
- **Complex Queries**: Support for tags, date comparisons, and advanced operators (not just equality but `>`, `<`, etc.)
- **High Specificity**: Domain-specific query syntax requirements

The key question: How do we build an NLQ system that adapts to these constant changes without requiring retraining or manual updates?

## Solution 1: Fine-Tuned Static Models

The traditional approach involves:

1. **Base Model**: A foundation model trained on general query syntax
2. **Fine-Tuning Layer**: Product-specific training on historical data
3. **Deployment**: Static model serving predictions

### Advantages
- Well-understood approach with proven results
- Can leverage existing infrastructure
- Potentially lower per-query costs at scale

### Limitations
- **Not Dynamic**: Every schema change requires retraining
- **Team Dependency**: Requires ML expertise and infrastructure
- **Training Cost**: Significant upfront investment in data and compute
- **Lag Time**: Delay between schema changes and model updates

For our use case with frequent schema evolution, this approach would create constant maintenance burden and potential staleness.

## Solution 2: Model Context Protocol (MCP)

An alternative approach uses the Model Context Protocol to provide dynamic context to a general-purpose LLM:

```
User Query → LLM with MCP Tools → Query Syntax → Execution
                ↓
         [Dynamic Schema]
         [Available Fields]
         [Query Examples]
```

The MCP provides:
- Real-time schema information
- Available fields and types
- Valid query patterns
- Domain-specific constraints

### Advantages
- **Fully Dynamic**: Adapts instantly to schema changes
- **Team Ownership**: No dependency on ML teams
- **Reduced Training**: No model training or retraining required
- **Transparency**: Tool usage provides explainability

### Limitations
- **Per-Query Cost**: Higher compute cost per query
- **Latency**: Multiple LLM calls may be needed
- **External Dependency**: Relies on external LLM providers

## Solution 3: Hybrid Approach

Could we combine the best of both worlds?

The idea: Use a small, fine-tuned base model with MCP for dynamic context.

### Current Reality
Most lightweight models (e.g., Llama 8B variants) don't support complex tool usage or reasoning required for MCP. They're trained for direct translation tasks.

Building a larger model with tool-use capabilities is:
- Significantly more complex
- Resource-intensive
- Not a near-term option for most teams

## Other Considerations

### Direct SQL Generation

Why not skip the intermediate query syntax and generate SQL directly?

- **Complexity**: Easy to generate syntactically correct but semantically wrong queries
- **Business Logic**: Existing query layer encodes important domain knowledge
- **Maintenance**: Duplicating logic in multiple places
- **Low Cost**: If the execution layer is already efficient, why bypass it?

### Agent SDK vs Custom Implementation

Modern agent frameworks (OpenAI Agents SDK, LangChain, etc.) provide higher-level abstractions. Why build custom?

**Language Constraints**: SDK may not be available in your stack (e.g., Python SDK when you need Go)

**Performance**: SDK adds:
- Network overhead for API calls
- Serialization/deserialization costs
- Potential service-to-service latency

**Duplication**: Using external service means duplicating logic and data access

**Control**: Custom implementation provides:
- Fine-grained optimization
- Direct access to data
- Single service boundary

For high-performance, low-latency requirements, a custom implementation integrated directly into your service may be preferable.

## Decision Framework

When choosing an NLQ architecture, consider:

1. **Schema Stability**
   - Frequent changes → Favor dynamic approaches (MCP)
   - Stable schema → Fine-tuning becomes viable

2. **Team Capabilities**
   - ML expertise available → Fine-tuning feasible
   - Engineering-focused → MCP easier to maintain

3. **Cost Profile**
   - High query volume → Fine-tuned model may be more economical
   - Lower volume → MCP per-query costs acceptable

4. **Latency Requirements**
   - Sub-second requirements → May need cached models
   - Seconds acceptable → MCP works well

5. **Ownership Model**
   - Centralized ML team → Fine-tuning with shared infrastructure
   - Product team ownership → MCP with direct control

## Our Decision: MCP

We chose the MCP approach because:

- **Dynamic First**: Our schema changes frequently enough that retraining overhead would be significant
- **Independence**: We wanted to move quickly without dependencies
- **Transparency**: Tool usage provides clear audit trails
- **Cost Acceptable**: Query volume doesn't justify training infrastructure
- **Experience Value**: Even if we change approaches later, the work on query datasets and testing will transfer

## Conclusion

There's no universal "best" architecture for NLQ systems. The right choice depends on your specific constraints:

- Schema stability
- Team structure and expertise
- Query volume and latency requirements
- Cost considerations
- Ownership and maintenance preferences

For dynamic systems with evolving schemas, MCP-based approaches offer a compelling balance of flexibility, maintainability, and time-to-market. For stable, high-volume scenarios, the upfront investment in fine-tuned models may pay dividends.

The key is matching the architecture to your specific needs, not following a one-size-fits-all approach.

---

*Last updated: 2025-08-28*
