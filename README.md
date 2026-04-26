# Phase 2: Real CrewAI Integration - Setup Guide

## 🎯 What's Been Implemented

**CrewAI Agent System**
- [`ai-service/src/agents/roles.py`](ai-service/src/agents/roles.py) - 5 specialized AI agents (Researcher, Developer, Social Manager, QA, Auditor)
- [`ai-service/src/agents/crew.py`](ai-service/src/agents/crew.py) - Crew orchestrator with auto-agent selection
- [`ai-service/src/main.py`](ai-service/src/main.py) - Updated FastAPI with real CrewAI execution

**Features**
- Auto-agent selection based on task keywords
- Single-agent and multi-agent task execution
- LLM provider configuration (OpenAI, Anthropic, Ollama)
- Comprehensive error handling and logging

## 🔧 Setup Instructions

### 1. Install Updated Dependencies

Stop the backend server (Ctrl+C in Terminal 1), then:

```bash
cd ai-service
pip install -r requirements.txt
```

### 2. Configure API Key

Edit [`ai-service/.env`](ai-service/.env) and add your OpenAI API key:

```env
OPENAI_API_KEY=sk-your-actual-api-key-here
OPENAI_MODEL=gpt-4o-mini
CREWAI_TELEMETRY=false
```

**Alternative LLM Providers:**

For Anthropic Claude:
```env
ANTHROPIC_API_KEY=your-key-here
ANTHROPIC_MODEL=claude-3-5-sonnet-20241022
```

For local Ollama:
```env
OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_MODEL=llama3.1
```

### 3. Restart Backend

```bash
cd ai-service
uvicorn src.main:app --reload --host 0.0.0.0 --port 8000
```

### 4. Test Real AI Agents

Open http://localhost:3000 and try these tasks:

**Research Task:**
- "Research the best practices for building scalable web applications"
- Should auto-select: Researcher agent

**Development Task:**
- "Implement a user authentication system with JWT tokens"
- Should auto-select: Developer agent

**Social Media Task:**
- "Create a social media post announcing our new product launch"
- Should auto-select: Social Manager agent

## 🤖 Available Agents

| Agent | Role | Expertise |
|-------|------|-----------|
| 🔍 Researcher | Research Specialist | Information gathering, analysis, insights |
| 💻 Developer | Senior Software Developer | Code implementation, architecture, debugging |
| 📱 Social Manager | Social Media Manager | Content creation, engagement, marketing |
| 🧪 QA | Quality Assurance Engineer | Testing, validation, bug detection |
| 🔒 Auditor | Security Auditor | Security review, compliance, vulnerabilities |

## 🎮 Auto-Agent Selection

The system automatically selects the best agent based on keywords:

- **research, analyze, investigate** → Researcher
- **code, develop, implement, build** → Developer
- **social, post, content, marketing** → Social Manager
- **test, qa, quality, bug** → QA Engineer
- **security, audit, vulnerability** → Auditor

## 📡 New API Endpoints

**GET /agents** - List all available agents and their capabilities
**GET /health** - Health check with configuration status

## ⚠️ Important Notes

- First execution may take 10-30 seconds as CrewAI initializes
- Ensure you have sufficient API credits for your LLM provider
- The backend will return an error if OPENAI_API_KEY is not configured
- Agent responses are now real AI-generated content, not mocks

## 🐛 Troubleshooting

**Error: "OPENAI_API_KEY not configured"**
- Edit [`ai-service/.env`](ai-service/.env) and add your API key

**Error: "Module not found"**
- Run `pip install -r requirements.txt` again

**Slow responses**
- Normal for first request (model initialization)
- Consider using gpt-4o-mini for faster responses
- Or use local Ollama for offline development

## 🚀 Next Steps (Phase 2 Continued)

- [ ] WebSocket/SSE for real-time agent status updates
- [ ] Task queue system for concurrent tasks
- [ ] Enhanced context system with project file analysis
- [ ] Agent collaboration (multi-agent workflows)
