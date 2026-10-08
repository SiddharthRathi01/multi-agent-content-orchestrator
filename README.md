# Multi-Agent Content Orchestrator

## Overview

The Multi-Agent Content Orchestrator is a multi-agent automation system built with n8n.

The system uses an Orchestrator Agent to coordinate specialized AI agents for research and newsletter creation.

Instead of handling every task through a single AI Agent, the workflow separates responsibilities across specialized agents:

- Orchestrator Agent
- Research Agent
- Newsletter Agent

The Research Agent uses Tavily for web research, while the Newsletter Agent creates newsletter content and uses an MCP Client to access its connected tools for newsletter delivery and document updates.

## Problem Statement

Content creation often requires multiple steps, including researching a topic, organizing the information, creating the final content, and distributing it.

Handling all of these responsibilities inside a single AI workflow can make the system difficult to manage and extend.

This project demonstrates an agentic approach where an Orchestrator coordinates specialized agents based on the task being requested.

## Solution

The Orchestrator Agent acts as the central coordination layer.

Based on the user's request, it can call the appropriate specialized agent:

- The Research Agent handles web research using Tavily.
- The Newsletter Agent creates newsletter content and uses its connected MCP Client to perform supported newsletter delivery and Google Drive operations.

This creates a modular workflow where each agent has a clearly defined responsibility.

## Architecture

    User
      |
      v
    Chat Interface
      |
      v
    Orchestrator Agent
      |
      +--------------------------+
      |                          |
      v                          v
    Research Agent          Newsletter Agent
      |                          |
      v                          v
    Tavily                   MCP Client
                                 |
                                 +---- Email
                                 |
                                 +---- Google Drive

The Orchestrator coordinates the specialized agents and determines which agent should be used for the requested task.

The Research Agent uses Tavily for web research.

The Newsletter Agent uses an MCP Client as its tool connection for the supported newsletter delivery and Google Drive operations.

## Agent Structure

### Orchestrator Agent

The Orchestrator is the main AI Agent responsible for coordinating the other agents.

It receives the user's request and determines which specialized agent is required.

The Orchestrator uses the specialized agents as tools and coordinates the overall workflow.

It is connected to:

- OpenAI Chat Model
- Simple Memory
- Research Agent
- Newsletter Agent

The Orchestrator acts as the central reasoning and routing layer of the system.

### Research Agent

The Research Agent is responsible for researching topics using the web.

It uses Tavily as its web research tool to retrieve relevant information.

The Research Agent provides researched information back to the Orchestrator so it can be used in downstream tasks.

It is connected to:

- OpenAI Chat Model
- Simple Memory
- Search in Tavily

### Newsletter Agent

The Newsletter Agent is responsible for creating and delivering newsletters.

It can:

- Create newsletter content
- Generate beautiful HTML newsletter content
- Send the newsletter through email
- Update an existing Google Drive document with the same newsletter content

The Newsletter Agent uses an MCP Client as its tool connection.

It is connected to:

- OpenAI Chat Model
- Simple Memory
- MCP Client

The Newsletter Agent is called when the user wants to create, send, or upload a newsletter.

## MCP Client

The MCP Client is connected to the Newsletter Agent as its tool.

It provides the Newsletter Agent with access to the tools required for its supported operations.

The MCP Client is used by the Newsletter Agent for actions such as:

- Newsletter email delivery
- Updating an existing Google Drive document

This allows the Newsletter Agent to interact with external services through a tool-based MCP connection rather than implementing each service directly inside the agent.

## End-to-End Workflow

### 1. User Request

The user provides a natural-language request to the Orchestrator.

### 2. Request Analysis

The Orchestrator analyzes the request and determines which specialized agent is required.

### 3. Agent Selection

The Orchestrator can call:

- Research Agent for web research
- Newsletter Agent for newsletter creation and delivery

### 4. Research

When research is required, the Research Agent uses Tavily to gather relevant information.

### 5. Newsletter Creation

When a newsletter is required, the Newsletter Agent creates structured and visually appealing newsletter content.

### 6. Tool Execution

The Newsletter Agent uses its MCP Client to access the connected tools required for supported operations.

### 7. Delivery and Document Update

The Newsletter Agent can send the newsletter through email and update an existing Google Drive document with the same newsletter content.

### 8. Final Response

The result is returned through the Orchestrator to the user.

## Research Agent

The Research Agent uses Tavily for web research.

Tavily provides the web-search capability required by the agent to gather information relevant to the requested topic.

The Research Agent is focused specifically on research rather than newsletter formatting or delivery.

## Newsletter Agent

The Newsletter Agent is designed specifically for newsletter-related tasks.

Its responsibilities include:

### Newsletter Content Creation

Creates clear, engaging, and structured newsletter content based on the information provided.

### HTML Generation

Generates visually appealing HTML content suitable for email delivery.

### Email Delivery

Uses the connected MCP-based tools to send the completed newsletter through email.

### Google Drive Update

Uses the connected MCP-based tools to update an existing Google Drive document with the same newsletter content when requested.

The email and Google Drive versions are intended to remain consistent with the final newsletter content.

## Orchestration Pattern

The workflow follows an agent-as-tool architecture.

The Orchestrator acts as the primary agent and uses specialized agents as tools.

The Research Agent and Newsletter Agent each have their own responsibilities, models, memory, and tools.

This separation allows the system to keep research, content creation, and external actions modular while maintaining a single conversational entry point for the user.

## Key Capabilities

### Multi-Agent Architecture

Uses multiple specialized AI agents instead of placing all responsibilities inside one workflow.

### Agent Orchestration

The Orchestrator determines which specialized agent should handle a request.

### Web Research

The Research Agent uses Tavily to gather information from the web.

### Newsletter Generation

The Newsletter Agent can create structured and visually appealing newsletter content.

### HTML Newsletter

The Newsletter Agent can generate newsletter content in HTML format for email delivery.

### MCP-Based Tool Access

The Newsletter Agent uses an MCP Client to access the tools required for its supported operations.

### Email Automation

The Newsletter Agent can send completed newsletters through its connected tools.

### Google Drive Integration

The Newsletter Agent can update an existing Google Drive document with the newsletter content.

## Technology Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow automation and agent orchestration |
| OpenAI | AI reasoning and agent execution |
| Tavily | Web research |
| Model Context Protocol | Tool connectivity for the Newsletter Agent |
| MCP Client | Connects the Newsletter Agent to its available tools |
| Gmail / Email Tool | Newsletter delivery |
| Google Drive | Newsletter document updates |
| Simple Memory | Conversational context for the agents |

## Project Structure

    multi-agent-content-orchestrator/
    |
    ├── README.md
    ├── orchestrator.json
    ├── research-agent.json
    └── newsletter-agent.json

## Setup

### Prerequisites

- n8n instance
- OpenAI credentials
- Tavily API credentials
- MCP configuration
- Email credentials
- Google Drive credentials

### Installation

1. Import the Orchestrator workflow into n8n.
2. Import the Research Agent workflow.
3. Import the Newsletter Agent workflow.
4. Configure the required credentials.
5. Configure Tavily for the Research Agent.
6. Configure the MCP Client used by the Newsletter Agent.
7. Configure the email and Google Drive connections required by the Newsletter Agent.
8. Configure the Orchestrator to use the Research Agent and Newsletter Agent as tools.
9. Test the individual agents.
10. Test the complete end-to-end workflow.

## Security

Credentials and authentication information are not included in this repository.

Never commit:

- API keys
- OAuth tokens
- Access tokens
- Passwords
- Private credentials
- Session tokens
- Environment files containing secrets

Use n8n's credential management system for authentication.

## Testing

The system can be tested by verifying:

1. The Orchestrator receives the user's request.
2. The correct specialized agent is selected.
3. The Research Agent can perform web research through Tavily.
4. Research results are returned to the Orchestrator.
5. The Newsletter Agent can generate newsletter content.
6. HTML newsletter content is generated correctly.
7. The Newsletter Agent can access its MCP Client.
8. The newsletter can be sent through the connected email tool.
9. The newsletter content can be updated in the existing Google Drive document.
10. The complete agent-to-agent workflow executes successfully.

## Project Objective

The objective of this project is to demonstrate how multiple specialized AI agents can be coordinated through an Orchestrator using n8n.

The architecture separates research, content creation, and external tool execution into dedicated components while providing the user with a single conversational interface.

This demonstrates a practical agent-as-tool architecture for building modular AI automation workflows with web research and MCP-based tool integration.

## Author

Siddharth Rathi
