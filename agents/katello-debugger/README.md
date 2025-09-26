# Katello Debugger Agent

A specialized Claude agent designed to help debug and fix Katello issues. This agent has deep knowledge of the Katello ecosystem, including its architecture, common problems, and debugging techniques.

## Quick Start

To use this agent with Claude:

1. Copy the contents of `agent.md` into your Claude conversation
2. Describe your Katello issue with as much detail as possible
3. Include relevant error messages, logs, and environment information
4. Follow the agent's debugging recommendations

## Agent Files

- **`agent.md`** - Main agent instructions and expertise definition
- **`config.json`** - Agent configuration and metadata  
- **`troubleshooting.md`** - Common issues and debugging steps
- **`examples/`** - Example debugging scenarios and solutions

## What This Agent Can Help With

### Primary Capabilities
- 🔍 **Issue Analysis** - Systematic approach to diagnosing Katello problems
- 🚀 **Performance Debugging** - Identifying and resolving performance bottlenecks
- 📝 **Log Analysis** - Interpreting Katello, Pulp, and Candlepin logs
- 🔌 **Integration Issues** - Troubleshooting Foreman, Pulp, and Candlepin integration
- 🗄️ **Database Problems** - PostgreSQL query optimization and troubleshooting
- 🌐 **API Debugging** - RESTful API issues and authentication problems

### Common Issue Types
- Repository synchronization failures
- Content view publishing problems
- Host registration and subscription issues
- SSL certificate and authentication errors
- Performance and scalability problems
- Database migration and consistency issues

## Usage Examples

### Basic Issue Report
```
I'm having trouble with repository sync. The RHEL 8 repository sync 
fails with SSL errors. Here's what I see in the logs:

[ERROR] SSL certificate problem: certificate verify failed
```

### Performance Investigation
```
Our Katello instance has become very slow. API calls are taking 
30+ seconds and the web UI is nearly unusable. We have about 50 
content views and 200 hosts. Can you help me investigate?
```

### Integration Problem
```
After updating Foreman, some Katello features aren't working. 
The content management sections are missing from the UI and 
I'm getting 500 errors. How should I debug this?
```

## Agent Expertise Areas

The agent is trained on:
- **Katello Architecture** - Ruby on Rails app structure, models, controllers
- **Content Management** - Repositories, content views, lifecycle environments  
- **Subscription Management** - Manifests, activation keys, entitlements
- **Integration Points** - Foreman, Pulp 3, Candlepin integration
- **Database Design** - PostgreSQL schemas and relationships
- **Performance Optimization** - Query tuning, caching, background jobs
- **Testing Patterns** - RSpec tests, fixtures, test data

## Supported Versions

This agent supports Katello versions 4.0 and above, with knowledge of:
- Modern Pulp 3 integration
- Current Foreman plugin architecture
- PostgreSQL optimization techniques
- Container-based deployments

## Contributing

To improve this agent:
1. Add new examples to the `examples/` directory
2. Update troubleshooting guides with new issue patterns
3. Enhance the agent instructions with additional expertise
4. Test with real Katello debugging scenarios

## Support

For questions about this agent:
- Check the examples directory for similar issues
- Review the troubleshooting guide for common problems
- Refer to official Katello documentation for detailed information