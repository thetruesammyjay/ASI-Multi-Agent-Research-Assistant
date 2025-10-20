# ASI Multi-Agent Research Assistant

[![Fetch.ai](https://img.shields.io/badge/Fetch.ai-Framework-blue)](https://fetch.ai/)
[![uAgents](https://img.shields.io/badge/uAgents-Framework-00D4FF)](https://fetch.ai/docs)
[![ASI Alliance](https://img.shields.io/badge/ASI-Alliance-blue)](https://superintelligence.io/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-yellow.svg)](https://opensource.org/licenses/Apache-2.0)

A production-grade decentralized multi-agent system for autonomous research that coordinates specialized agents to conduct comprehensive information gathering, analysis, and validation. Built on Fetch.ai's uAgents framework with enterprise-level resilience patterns, comprehensive observability, and state management.

## Overview

This project implements a multi-agent research system with enterprise-grade infrastructure. The system decomposes research queries into discrete tasks handled by specialized agents that communicate via standardized protocols with comprehensive fault tolerance, observability, and state management.

### Problem Statement

Traditional research workflows require manual coordination across multiple tasks: information gathering, content analysis, fact verification, and synthesis. This system automates the entire pipeline through intelligent agent collaboration with production-grade reliability patterns, reducing research time from hours to minutes while maintaining quality through validation mechanisms.

### Solution Architecture

Four specialized agents work in concert with comprehensive infrastructure support:

- **Orchestrator Agent**: Coordinates workflow, manages sessions, aggregates results with confidence scoring
- **Search Agent**: Multi-provider web search with caching, rate limiting, and content extraction
- **Analysis Agent**: LLM-powered content processing with connection pooling and token tracking
- **Validation Agent**: Cross-reference validation across sources, assesses information credibility, checks for contradictions, and assigns confidence scores to validated facts.

### Infrastructure Layer

- **Resilience Patterns**: Circuit breakers, retry strategies, timeouts, bulkheads, rate limiting
- **State Management**: Session persistence, checkpoints, multi-level caching (Memory/Disk/Redis)
- **Observability**: Structured logging, metrics collection, distributed tracing, health checks
- **Protocol Layer**: Message validation, version control, delivery guarantees, deduplication

## Key Features

### Production Infrastructure

**Resilience & Fault Tolerance**
- Circuit breaker pattern prevents cascade failures
- Exponential backoff retry with jitter for transient errors
- Hierarchical timeout management (operation/request/total)
- Bulkhead pattern for resource isolation
- Token bucket rate limiting for API calls

**State Management & Persistence**
- Session state persistence with agent.storage integration
- Checkpoint/restore for workflow recovery
- Multi-level caching: Memory (LRU) → Disk → Distributed (Redis)
- Automatic cleanup of expired data
- State migrations and versioning

**Observability & Monitoring**
- Structured JSON logging with correlation IDs
- Metrics collection: counters, gauges, histograms (P50/P95/P99)
- OpenTelemetry distributed tracing
- Health checks: liveness and readiness probes
- Component-level health monitoring

**Agent Communication Protocol (ACP)**
- Message envelopes with metadata and signatures
- At-least-once delivery with deduplication
- Priority-based message queuing
- Message expiry and TTL handling
- Multi-hop routing support

### Agent Capabilities

**Orchestrator Agent**
- Workflow state machine with session management
- Dynamic agent discovery via Almanac
- Parallel task execution with asyncio
- Result aggregation with confidence scoring
- Checkpoint and resume capabilities

**Search Agent**
- Multi-provider search (Brave, Google, Bing)
- Query expansion and reformulation
- Result deduplication and relevance scoring
- Content extraction and enrichment
- Search result caching with TTL

**Analysis Agent**
- LLM client connection pooling
- Request batching and throttling
- Structured output with schema validation
- Content chunking for large documents
- Token usage tracking and cost optimization

**Validation Agent**
- Cross-reference validation across sources
- Source credibility assessment
- Semantic similarity with sentence transformers
- Claim decomposition and verification
- Confidence scoring with uncertainty quantification

## Technology Stack

### Core Framework

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Agent Framework | Fetch.ai uAgents 0.12+ | Core agent infrastructure and lifecycle management |
| Communication | ACP + Research Protocol | Standardized inter-agent messaging with validation |
| Language Models | OpenAI API / Anthropic | Natural language processing and generation |
| Web Search | Brave Search API | Information retrieval from the web |
| Deployment | Agentverse / Docker | Cloud-based or containerized hosting |
| Blockchain | Fetch.ai Ledger | Decentralized agent registry (Almanac) |
| Language | Python 3.10+ | Implementation language |

### Infrastructure

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Resilience | pybreaker | Circuit breaker pattern implementation |
| Caching | Redis + diskcache | Multi-level caching infrastructure |
| Observability | OpenTelemetry + structlog | Metrics, logs, and distributed tracing |
| Validation | Pydantic 2.0 | Data validation and settings management |
| Semantic Search | sentence-transformers | Embedding-based similarity for validation |
| Rate Limiting | asyncio-throttle | API rate limiting and throttling |

## Prerequisites

### Required
- Python 3.10 or higher
- API credentials:
  - Brave Search API key
  - OpenAI API key (or Anthropic API key)
  - Agent seeds for each agent (generated or configured)

### Optional
- Fetch.ai account for Agentverse deployment
- Redis server for distributed caching
- OpenTelemetry collector for distributed tracing

## Installation

### Local Development Setup

Clone the repository:
```bash
git clone https://github.com/thetruesammyjay/ASI-Multi-Agent-Research-Assistant.git
cd asi-multi-agent-research-assistant
```

Create and activate virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

Install dependencies:
```bash
pip install -r requirements.txt
```

Configure environment variables:
```bash
cp .env.example .env
# Edit .env with your API keys and configuration
```

### Running Locally

Start individual agents using the runner script (each in a separate terminal):
```bash
# Terminal 1: Start orchestrator
python run_agents.py orchestrator

# Terminal 2: Start search agent
python run_agents.py search

# Terminal 3: Start analysis agent
python run_agents.py analysis

# Terminal 4: Start validation agent
python run_agents.py validation
```

Execute a test query:
```bash
python examples/simple_query.py --query "What are the benefits of decentralized AI?"
```

### Docker Deployment

Build and run with Docker Compose:
```bash
# Build containers
docker-compose build

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

## Agentverse Deployment

### Deploy Agents

Authenticate with Agentverse:
```bash
agentverse login
```

Deploy each agent:
```bash
agentverse deploy agents/orchestrator_agent.py
agentverse deploy agents/search_agent.py
agentverse deploy agents/analysis_agent.py
agentverse deploy agents/validation_agent.py
```

### Verify Deployment

Update `config/agent_config.py` with deployed agent addresses:
```python
ORCHESTRATOR_ADDRESS = "agent1q..."
SEARCH_AGENT_ADDRESS = "agent1q..."
ANALYSIS_AGENT_ADDRESS = "agent1q..."
VALIDATION_AGENT_ADDRESS = "agent1q..."
```

Verify deployment:
```bash
python tests/test_deployed.py
```

## Project Structure
```
asi-multi-agent-research-assistant/
│
├── agents/                      # Agent implementations
│   ├── base_agent.py           # Production-grade base class with lifecycle management
│   ├── orchestrator_agent.py   # Workflow coordinator with state machine
│   ├── search_agent.py         # Multi-provider search with caching
│   ├── analysis_agent.py       # LLM integration with pooling
│   └── validation_agent.py     # Cross-reference validation
│
├── protocols/                   # Communication protocols
│   ├── acp_wrapper.py          # Agent Communication Protocol implementation
│   ├── research_protocol.py    # Enhanced research protocol with versioning
│   └── chat_protocol.py        # Chat protocol wrapper
│
├── utils/                       # Infrastructure utilities
│   ├── resilience.py           # Circuit breakers, retries, timeouts, bulkheads
│   ├── state_manager.py        # Session management, checkpoints, caching
│   ├── telemetry.py            # Logging, metrics, tracing, health checks
│   ├── llm_client.py           # LLM API integration
│   ├── web_search.py           # Search API wrapper
│   ├── web_fetcher.py          # Content retrieval
│   ├── validators.py           # Validation utilities
│   └── logger.py               # Logging configuration
│
├── models/                      # Data models
│   ├── messages.py             # Message definitions with Pydantic validation
│   ├── research_models.py      # Research-specific models
│   └── config_models.py        # Configuration models
│
├── config/                      # Configuration management
│   ├── settings.py             # Production settings with environment support
│   └── agent_config.py         # Agent-specific configuration
│
├── tests/                       # Test suite
│   ├── test_agents.py          # Agent unit tests
│   ├── test_protocols.py       # Protocol tests
│   ├── test_integration.py     # Integration tests
│   └── test_resilience.py      # Resilience pattern tests
│
├── examples/                    # Usage examples
│   ├── simple_query.py         # Basic usage
│   ├── advanced_query.py       # Advanced features
│   └── batch_queries.py        # Batch processing
│
├── docs/                        # Documentation
│   ├── ARCHITECTURE_PRODUCTION.md  # Production architecture
│   ├── API.md                  # API reference
│   ├── DEPLOYMENT.md           # Deployment guide
│   └── DEVELOPMENT.md          # Development guide
│
├── deployment/                  # Deployment configurations
│   ├── docker-compose.yml      # Local Docker deployment
│   └── agentverse/             # Agentverse deployment scripts
│
├── .checkpoints/               # Workflow checkpoints (generated)
├── .cache/                     # Disk cache (generated)
├── requirements.txt            # Python dependencies
├── .env.example               # Environment template
├── run_agents.py              # Agent runner script
├── IMPLEMENTATION_STATUS.md   # Implementation progress
├── README.md                  # This file
└── LICENSE                    # Apache 2.0 License
```

## Usage Examples

### Basic Research Query
```python
from agents.orchestrator_agent import OrchestratorAgent

orchestrator = OrchestratorAgent()

query = "What are the benefits of decentralized AI?"
result = await orchestrator.research(query)

print(result.summary)
print(result.sources)
print(f"Confidence: {result.validation_score}")
```

### Advanced Configuration
```python
research_config = {
    "query": "Compare proof-of-work and proof-of-stake consensus mechanisms",
    "depth": "detailed",
    "max_sources": 15,
    "enable_validation": True,
    "timeout": 300
}

result = await orchestrator.research(**research_config)
```

### Streaming Results
```python
async for update in orchestrator.research_stream(query):
    print(f"Stage: {update.stage}")
    print(f"Progress: {update.progress}%")
    if update.partial_results:
        print(f"Finding: {update.partial_results}")
```

## Performance Characteristics

### Latency Targets

| Operation | P50 | P95 | P99 |
|-----------|-----|-----|-----|
| Search | 500ms | 1s | 2s |
| Analysis | 2s | 4s | 6s |
| Validation | 1s | 2s | 3s |
| Full Workflow | 3s | 5s | 8s |

### Throughput & Reliability

- **Concurrent Queries**: 10-50 (configurable)
- **Requests/Second**: 5-20 per agent
- **Cache Hit Rate**: >80% for repeated queries
- **Availability Target**: 99.9% uptime
- **Error Rate**: <1% of requests

### Resource Usage

- **Memory**: 256MB-1GB per agent
- **CPU**: 0.5-2 cores per agent
- **Storage**: 100MB-1GB for cache/checkpoints

## Testing

Run the complete test suite:
```bash
pytest tests/
```

Run specific test categories:
```bash
pytest tests/test_agents.py          # Agent tests
pytest tests/test_protocols.py       # Protocol tests
pytest tests/test_integration.py     # Integration tests
pytest tests/test_resilience.py      # Resilience pattern tests
```

Generate coverage report:
```bash
pytest --cov=agents --cov=protocols --cov=utils tests/
```

## Documentation

Comprehensive documentation is available in the `docs/` directory:

- [Production Architecture](docs/ARCHITECTURE_PRODUCTION.md) - System design and infrastructure
- [Implementation Status](IMPLEMENTATION_STATUS.md) - Current implementation progress
- [API Reference](docs/API.md) - Complete API documentation (pending)
- [Deployment Guide](docs/DEPLOYMENT.md) - Production deployment instructions (pending)
- [Development Guide](docs/DEVELOPMENT.md) - Contributing and development setup (pending)

## Configuration

The system supports environment-specific configuration through environment variables:

### Required Environment Variables

```bash
# API Keys
BRAVE_API_KEY=your_brave_api_key
OPENAI_API_KEY=your_openai_api_key

# Agent Seeds (generate unique values)
ORCHESTRATOR_SEED=orchestrator_seed_value
SEARCH_AGENT_SEED=search_agent_seed_value
ANALYSIS_AGENT_SEED=analysis_agent_seed_value
VALIDATION_AGENT_SEED=validation_agent_seed_value
```

### Optional Configuration

```bash
# Environment
ENVIRONMENT=development  # development, staging, production, test
DEBUG=false
LOG_LEVEL=INFO

# Resilience
MAX_RETRIES=3
CIRCUIT_BREAKER_THRESHOLD=5
CIRCUIT_BREAKER_TIMEOUT=60

# Caching
ENABLE_CACHE=true
CACHE_TTL=3600
REDIS_URL=redis://localhost:6379

# Observability
ENABLE_METRICS=true
ENABLE_TRACING=false
OTEL_EXPORTER_ENDPOINT=http://localhost:4317

# Feature Flags
ENABLE_VALIDATION=true
ENABLE_SEMANTIC_SEARCH=false
ENABLE_MULTI_PROVIDER_SEARCH=false
```

## Monitoring & Observability

### Health Checks

```bash
# Liveness probe (is agent running?)
curl http://localhost:8000/health/live

# Readiness probe (can agent handle requests?)
curl http://localhost:8000/health/ready
```

### Metrics

Metrics are collected automatically and can be exported to Prometheus:

- Agent state and health
- Message counts by type and status
- Operation latency (P50, P95, P99)
- Circuit breaker states
- Cache hit rates

### Logs

Structured JSON logs with correlation IDs for request tracing:

```json
{
  "timestamp": "2024-01-01T12:00:00Z",
  "level": "INFO",
  "correlation_id": "abc123",
  "session_id": "xyz789",
  "agent": "orchestrator",
  "message": "Research query received"
}
```

## Contributing

Contributions are welcome. Please review [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on code standards, testing requirements, and the pull request process.

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

## Acknowledgments

This project builds upon technologies and research from:

- **ASI Alliance** for advancing decentralized artificial intelligence
- **Fetch.ai** for the uAgents framework and agent infrastructure
- **SingularityNET** for contributions to AGI research and MeTTa
- **Superteam** for supporting open-source development through bounties

## Contact

**Developer**: sammyjayisthename

- GitHub: [@thetruesammyjay](https://github.com/thetruesammyjay)
- Email: sammyjayisthename@gmail.com
- Twitter: [@thatbwoysammyjay]

## Support

- **Bug Reports**: [GitHub Issues](https://github.com/thetruesammyjay/ASI-Multi-Agent-Research-Assistant/issues)
- **Feature Requests**: [GitHub Discussions](https://github.com/thetruesammyjay/ASI-Multi-Agent-Research-Assistant/discussions)
- **General Questions**: Email or GitHub Discussions

---

**Built for the Superteam ASI Agents Track | Powered by ASI Alliance Technologies**
