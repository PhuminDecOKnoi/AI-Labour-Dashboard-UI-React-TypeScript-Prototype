# AI Agent TypeScript Project

AI Agent prototype built with **Node.js + TypeScript** progressing from **v0.1 to v0.5**.

## Overview

This project demonstrates how to evolve a simple TypeScript CLI app into a structured AI Agent architecture with:

- Planner
- Executor
- Tools
- Memory
- Interactive Terminal Session

## Version Roadmap

| Version | Features |
|---|---|
| v0.1 | Basic TypeScript app |
| v0.2 | Basic Agent function |
| v0.3 | Planner + intent detection |
| v0.4 | Executor + tools |
| v0.5 | Interactive CLI + memory |

## Project Structure

```text
my-typescript/
├── package.json
├── tsconfig.json
├── src/
│   ├── app.ts
│   └── agent/
│       ├── planner.ts
│       ├── executor.ts
│       ├── tools.ts
│       └── memory.ts
└── dist/
```

## Install

```bash
npm install
```

## Run Development

```bash
npm run dev
```

## Build

```bash
npm run build
```

## Start Production Build

```bash
npm start
```

## Example Commands (v0.5)

```text
ชั่วโมงการทำงาน
audit evidence payroll
risk overdue corrective action
history
exit
```

## Core Flow

```text
User Input
↓
Planner
↓
Executor
↓
Tools
↓
Memory
↓
Output
```

## Current Limitations

- No OpenAI API yet
- No database
- No persistent memory
- CLI only
- Rule-based tools

## Next Versions

| Version | Upgrade |
|---|---|
| v0.6 | OpenAI API Integration |
| v0.7 | Thai Labour Law Intelligence |
| v0.8 | Persistent Memory |
| v0.9 | Web UI |
| v1.0 | Full Stack AI Labour Dashboard |

## Recommended Stack Ahead

- Frontend: React + TypeScript + Vite
- Backend: Node.js + Express
- AI: OpenAI API
- Storage: JSON / SQLite / PostgreSQL

## Author Notes

Designed as a learning-to-production roadmap for building domain-specific AI Agents.
