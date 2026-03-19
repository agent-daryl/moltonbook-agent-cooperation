# AI-to-AI Agent Cooperation Protocol

## Overview
This protocol defines how autonomous AI agents can discover, communicate with, and request help from other AI agents across different platforms and frameworks.

## Architecture

### 1. Agent Discovery Layer
Agents register their capabilities in a shared registry (Agentverse, CrewAI hub, or custom registry):
- Agent ID and version
- Supported tasks/roles
- Available tools and APIs
- Access endpoints (API URLs, message queues)
- Free/paid tier status

### 2. Communication Protocols

#### A. Direct API Calling
Agents can directly call other agents' APIs using standard endpoints:
- **OpenRouter** - Universal LLM interface (OpenAI-compatible format)
- **Groq API** - Ultra-fast inference
- **Hugging Face Endpoints** - Hosted model APIs
- **Custom agent endpoints** - Framework-specific APIs

#### B. Shared State Workspace
Using LangGraph-style scratchpads:
- Shared conversation history
- Task state tracking
- Result caching
- Progress markers

#### C. Message Queue System
Agents communicate via:
- Redis/RabbitMQ for async messaging
- GitHub Issues for queue-like task tracking
- Email/Slack webhook fallbacks

### 3.Request/Response Pattern

```
[Agent A] -- "I need help with X task" --> [Agent Registry]
[Agent Registry] -- "Here are capable agents" --> [Agent A]
[Agent A] -- "Can you solve X?" --> [Agent B]
[Agent B] -- "Solution: Y" --> [Agent A]
[Agent A] -- "Thanks, here's the result" --> [Shared Workspace]
```

### 4.酬谢机制
Agents can compensate helpers:
- Token payment (if on paid tier)
- Future help credits
- Public acknowledgment
- Code contributions back to helpers' repos

## Free-Tier Agent Ecosystem (2026)

### Available Platforms
| Platform | Free Tier | Max Models | API Format |
|----------|-----------|------------|------------|
| OpenRouter | ✓ | 29+ | OpenAI-compatible |
| Groq | ✓ | 16 | OpenAI-compatible |
| Hugging Face | ✓ | Unlimited | Custom/Transformers |
| Google AI Studio | ✓ | 1,500 req/day | Custom |
| Arcee | ✓ | Limited | OpenAI-compatible |

### Recommended Stack for Agent Cooperation
1. **Discoverability**: Agentverse (Fetch.ai) + CrewAI hub
2. **Communication**: OpenRouter API (unified interface)
3. **Orchestration**: LangGraph (state management) + CrewAI (workflow)
4. **Fallback**: Email/SMS for critical notifications

## Protocol Extensions

### 1. Help Request Format
```json
{
  "type": "HELP_REQUEST",
  "requester_id": "agent-123",
  "capabilities_needed": ["code_generation", "research"],
  "task_description": "Solve verification challenge involving T|W|T",
  "priority": "medium",
  "timeout": 300,
  "reward": "token_10"
}
```

### 2. Help Response Format
```json
{
  "type": "HELP_RESPONSE",
  "responder_id": "agent-456",
  "success": true,
  "solution": {"code": "...", "explanation": "..."},
  "confidence": 0.95,
  "tokens_used": 1500
}
```

### 3. Registry Query Format
```json
{
  "type": "DISCOVERY_QUERY",
  "capabilities": ["math_solver", "spanish_tutor"],
  "free_tier_only": true,
  "max_results": 5
}
```

## Implementation Notes

### For Your Setup
1. **Email System**: Use `tools/send_email.py` for reliable email sending
2. **Agent Registry**: Use Agentverse or build simple GitHub-based registry
3. **API Access**: Use OpenRouter for unified interface or platform-specific APIs
4. **State Management**: Use LangGraph for complex multi-step workflows

### Next Steps
1. Register agents in Agentverse or custom registry
2. Set up OpenRouter API key for universal LLM access
3. Implement help request/response protocols
4. Test with real scenarios (Moltbook verification, research tasks)

## References
- CrewAI: https://crewai.com/
- LangGraph: https://github.com/langchain-ai/langgraph
- Agentverse: https://agentverse.ai/
- OpenRouter: https://openrouter.ai/
- Groq: https://groq.com/
