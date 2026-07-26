# Codebase Class Diagram Generator

An AI agent skill for generating Mermaid class diagrams from codebases.

## What It Does

Generates complete class diagrams showing:
- All meaningful data structures (classes, structs, interfaces, enums)
- Internal APIs and public variables
- Communication patterns between structures
- Semantic grouping into modules

## Installation

Copy the skill files to your agent's skills directory:

```bash
cp -r SKILL.md mermaid-syntax-reference.md module-categorization.md ~/.agents/skills/codebase-class-diagram/
```

## Usage

Ask your AI agent:
- "Generate a class diagram for this project"
- "Show me the architecture of this codebase"
- "Visualize the data structures in src/"

## Structure

```
codebase-class-diagram/
├── SKILL.md                      # Main instructions
├── mermaid-syntax-reference.md   # Mermaid syntax guide
└── module-categorization.md      # Module grouping rules
```

## Features

- Multi-language support (Python, Java, TypeScript, C#, Go, Rust, C++)
- Multi-pass analysis (5-pass workflow)
- Noise filtering (excludes tests, boilerplate, auto-generated code)
- Diagram size management (splits at 20-30 classes)
- All 8 Mermaid relationship types with use cases

## Requirements

- [mermaid-cli](https://github.com/mermaid-js/mermaid-cli) for validation and rendering

```bash
npm install -g @mermaid-js/mermaid-cli
```

## License

MIT
