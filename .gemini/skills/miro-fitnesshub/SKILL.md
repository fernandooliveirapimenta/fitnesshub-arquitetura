---
name: miro-fitnesshub
description: Management and automation of the FitnessHub architecture Miro board. Use this skill when the user wants to interact with the Miro board at https://miro.com/app/board/uXjVGuXmKuM=/.
---

# Miro FitnessHub

This skill automates interactions with the FitnessHub architecture board on Miro.

## Target Board
The board URL for all operations is: `https://miro.com/app/board/uXjVGuXmKuM=/`

## Workflows

### 1. Board Exploration
When asked to explore the board, use `mcp_Miro_context_explore` with the board URL.

### 2. Diagram Creation
To create diagrams:
1. Always call `mcp_Miro_diagram_get_dsl` first for the specific diagram type.
2. Generate the DSL based on the requirements.
3. Use `mcp_Miro_diagram_create` with the board URL.

### 3. Documentation
To manage documents on the board:
- Use `mcp_Miro_doc_create` or `mcp_Miro_doc_update` always specifying the board URL.

## Guidelines
- Always use the URL `https://miro.com/app/board/uXjVGuXmKuM=/` for any tool call requiring a `miro_url`.
- If a specific widget ID is needed, find it first using exploration tools.
