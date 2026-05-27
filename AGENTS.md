# Agents Instructions

- **Root_Folder**: `/`
- **Agents_Folder**: `.agents`
- **Agents_file**: `AGENTS.md`
- **Project_Folder**: `doc`

## Product Overview

demo-prompt is a repository for managing and demonstrating agent prompts and skills.

- Includes templates for skills like commit-changes, unit-testing, and more.
- Provides a structured way for agents to interact with the codebase.
  
## Technical Implementation

### Tech Stack

- **Language**: Markdown / Shell
- **Framework**: Gemini CLI / Custom Skills
- **Version Control**: Git

### Development workflow

```bash
# Set up the project
# No specific setup required beyond git
# Build/Compile the project
# N/A
# Run the project
# Use Gemini CLI or other agents to process prompts
# Test the project
# N/A
```

### Folder structure
```text
.                         # Project root  
├── AGENTS.md             # This file with instructions for AI agents
├── .agents/              # Agents related files (skills, specs, etc)
│   └── skills/           # Custom agent skills
├── doc/                  # Project related files (specs, plans, etc)
├── prompt/               # Reusable prompts directory
├── README.md             # Human friendly project overview
└── .gitignore            # Git ignore file
```

## Environment
- **OS dev**: `Windows`
- **Terminal**: `PowerShell`
- **Default branch**: `main` 

## Behavior Guidelines

- Code and documentation must be in English.
- Chat responses must be in the language of the user prompt.
- Sacrifice grammar for conciseness when needed to fit response limits.
- When using templates, ensure to replace {placeholders} with actual values.

### Naming Conventions

Use slugs with hyphens for any identifiers or non code file names.

Prefix specifications, branches, and commit messages with the following tags:

- `feat` : For new features or significant changes.
- `fix` : For bug fixes or minor improvements.
- `chore` : For routine tasks, maintenance, or non-functional changes.
