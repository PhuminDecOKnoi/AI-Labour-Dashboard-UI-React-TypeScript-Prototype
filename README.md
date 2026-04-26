# AI Agent TypeScript Project (v0.1 - v0.6)

AI Agent prototype built with **Node.js + TypeScript** progressing from **v0.1 to v0.6**.

## Overview

This project demonstrates how to evolve a simple TypeScript CLI app into a structured AI Agent architecture with:

- Planner
- Executor
- Tools
- Memory
- Interactive Terminal Session
- OpenAI API Integration

---

## Version Roadmap

| Version | Features |
|---|---|
| v0.1 | Basic TypeScript app |
| v0.2 | Basic Agent function |
| v0.3 | Planner + intent detection |
| v0.4 | Executor + tools |
| v0.5 | Interactive CLI + memory |
| v0.6 | OpenAI API Integration |

---

## Project Structure

```text
my-typescript/
├── package.json
├── tsconfig.json
├── .env
├── src/
│   ├── app.ts
│   ├── agent/
│   │   ├── planner.ts
│   │   ├── executor.ts
│   │   ├── tools.ts
│   │   └── memory.ts
│   └── services/
│       └── openai.ts
└── dist/
```

---

## Install

```bash
npm install
```

Install OpenAI SDK:

```bash
npm install openai dotenv
```

---

## Environment Variables

Create `.env`

```env
OPENAI_API_KEY=your_api_key_here
```

---

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

---

# v0.6 OpenAI API Integration

## src/services/openai.ts

```ts
import OpenAI from "openai";
import dotenv from "dotenv";

dotenv.config();

export const client = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
});
```

---

## Example Tool Usage

```ts
import { client } from "../services/openai";

export async function askOpenAI(prompt: string): Promise<string> {
  const response = await client.responses.create({
    model: "gpt-5.5",
    input: prompt
  });

  return response.output_text;
}
```

---

## Example Agent Flow (v0.6)

```text
User Input
↓
Planner
↓
Executor
↓
OpenAI Tool Call
↓
Memory Save
↓
Final Output
```

---

## Example Commands

```text
ชั่วโมงการทำงาน
Explain Thai overtime law
Audit payroll risk
history
exit
```

---

## Recommended Models

| Use Case | Model |
|---|---|
| Fast General Tasks | gpt-5.3 |
| Strong Reasoning | gpt-5.4 |
| Deep Analysis | gpt-5.5 |
| Low Cost Tasks | mini models |

---

## Security Notes

- Never hardcode API keys
- Use `.env`
- Add `.env` to `.gitignore`
- Validate user input
- Add rate limiting in production

---

## Current Limitations

- CLI based
- No database
- In-memory memory only
- No web dashboard yet

---

## Next Versions

| Version | Upgrade |
|---|---|
| v0.7 | Thai Labour Law Intelligence |
| v0.8 | Persistent Memory |
| v0.9 | Website UI |
| v1.0 | Full Stack AI Labour Dashboard |

---

## Recommended Stack Ahead

- Frontend: React + TypeScript + Vite
- Backend: Node.js + Express
- AI: OpenAI API
- Storage: JSON / SQLite / PostgreSQL

---

## Author Notes

Designed as a learning-to-production roadmap for building domain-specific AI Agents.
