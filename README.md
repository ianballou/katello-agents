# katello-agents
LLM agents and the like for Katello development.

## Available Agents

### 🔧 katello-debugger
A specialized Claude agent for debugging and fixing Katello issues. Located in `agents/katello-debugger/`.

**Capabilities:**
- Repository synchronization troubleshooting
- Content view and publishing issues
- Host registration and subscription problems  
- Performance analysis and optimization
- Database debugging and query optimization
- Integration issue resolution (Foreman, Pulp, Candlepin)

**Usage:** Copy the agent instructions from `agents/katello-debugger/agent.md` into your Claude conversation and describe your Katello issue.

## Getting Started

1. Navigate to the `agents/` directory
2. Choose the appropriate agent for your needs
3. Follow the agent-specific README and instructions
4. Copy the agent prompt into Claude and start debugging!
