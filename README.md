# GitHub Agent

A repository exploring the **Model Context Protocol (MCP)** — an open standard for connecting AI assistants to external tools, data sources, and services.

---

## What is the Model Context Protocol (MCP)?

The **Model Context Protocol (MCP)** is an open protocol developed by Anthropic that standardizes how large language models (LLMs) communicate with external tools, data sources, and services. It is designed to make AI assistants more capable, extensible, and interoperable by providing a well-defined interface between AI models and the external world.

Think of MCP as a **universal adapter** — similar to how USB standardized device connections, MCP standardizes how AI models connect to context providers and tools.

---

## Why MCP?

Before MCP, integrating AI models with external systems required custom, one-off integrations for every tool or data source. This was:

- **Fragmented** — every tool required its own bespoke integration.
- **Expensive** — significant engineering effort for each new connection.
- **Inconsistent** — no shared standard across AI platforms.

MCP solves this by providing a single, reusable protocol that any AI system can implement to interact with any compliant tool or data source.

---

## Core Architecture

MCP is built around a **client-server architecture** with three main components:

### 1. MCP Host
The AI-powered application (e.g., Claude Desktop, VS Code Copilot, or a custom agent) that wants to access external context or capabilities.

### 2. MCP Client
A protocol client embedded within the host application. It establishes and manages connections to MCP servers.

### 3. MCP Server
A lightweight server that exposes specific capabilities (tools, resources, and prompts) to the AI model via the MCP protocol.

```
┌─────────────────────────────────────┐
│           MCP Host (AI App)         │
│  ┌──────────────┐  ┌─────────────┐  │
│  │  AI Model    │  │  MCP Client │  │
│  └──────────────┘  └──────┬──────┘  │
└─────────────────────────────────────┘
                             │  MCP Protocol
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │MCP Server│  │MCP Server│  │MCP Server│
        │(GitHub)  │  │(Database)│  │(File Sys)│
        └──────────┘  └──────────┘  └──────────┘
```

---

## Key Concepts

### Tools
Functions that an AI model can invoke to perform actions or retrieve computed data. Examples:
- Search a database
- Call a REST API
- Execute a shell command
- Create a GitHub issue

### Resources
Static or dynamic data that an AI model can read as context. Examples:
- File contents
- Database records
- API responses

### Prompts
Reusable prompt templates that MCP servers can expose to guide the AI model's behavior in specific contexts.

### Sampling
MCP also supports **sampling**, where an MCP server can request the AI model to generate text — enabling more advanced agentic workflows.

---

## Transport Layer

MCP supports two primary transport mechanisms:

| Transport | Description | Use Case |
|-----------|-------------|----------|
| **stdio** | Communication via standard input/output | Local processes, CLI tools |
| **HTTP + SSE** | HTTP with Server-Sent Events | Remote servers, web services |

---

## MCP in Action: GitHub MCP Server

This repository was created using the **GitHub MCP Server** — a prime example of MCP in practice. The GitHub MCP Server exposes GitHub's API as a set of MCP tools, allowing AI assistants to:

- Create and manage repositories
- Read and write files
- Open and close issues
- Create and review pull requests
- Search code and users

All of this is done through natural language instructions to the AI, which then calls the appropriate MCP tools.

---

## Getting Started with MCP

### 1. Install an MCP-compatible host
- [Claude Desktop](https://claude.ai/download)
- [VS Code with GitHub Copilot](https://code.visualstudio.com/)

### 2. Find or build an MCP Server
- Browse available servers: [MCP Server Registry](https://github.com/modelcontextprotocol/servers)
- Build your own using the [MCP SDK](https://github.com/modelcontextprotocol/typescript-sdk)

### 3. Configure your host
Add the server to your host's MCP configuration (e.g., `mcp.json` in VS Code or `claude_desktop_config.json` for Claude Desktop):

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>"
      }
    }
  }
}
```

---

## Official Resources

| Resource | Link |
|----------|------|
| MCP Specification | [spec.modelcontextprotocol.io](https://spec.modelcontextprotocol.io) |
| Official Documentation | [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| GitHub Organization | [github.com/modelcontextprotocol](https://github.com/modelcontextprotocol) |
| TypeScript SDK | [github.com/modelcontextprotocol/typescript-sdk](https://github.com/modelcontextprotocol/typescript-sdk) |
| Python SDK | [github.com/modelcontextprotocol/python-sdk](https://github.com/modelcontextprotocol/python-sdk) |
| MCP Servers Registry | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) |

---

## License

This project is licensed under the MIT License.
