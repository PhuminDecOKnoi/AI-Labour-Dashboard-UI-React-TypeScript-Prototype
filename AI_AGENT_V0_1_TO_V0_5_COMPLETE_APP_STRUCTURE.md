# AI Agent TypeScript v0.1–v0.5 — Complete App Structure

> One-page Markdown สำหรับเก็บเป็นคู่มือ/README ของโปรเจกต์ AI Agent ตั้งแต่เวอร์ชัน v0.1 ถึง v0.5  
> Stack: Node.js + TypeScript  
> สถานะ: Rule-based Agent / ยังไม่เชื่อม OpenAI API

---

## 0. เป้าหมายของโปรเจกต์

โปรเจกต์นี้เริ่มจาก TypeScript app ขนาดเล็ก แล้วค่อย ๆ พัฒนาเป็น AI Agent prototype ที่มีองค์ประกอบหลักดังนี้

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
Terminal Output / Interactive Session
```

---

## 1. Version Roadmap

| Version | แนวคิดหลัก | สิ่งที่เพิ่มเข้ามา |
|---|---|---|
| v0.1 | Basic TypeScript App | เริ่มจาก `app.ts` และ compile เป็น JavaScript |
| v0.2 | Basic Agent Function | รับ goal และแสดงผลแบบง่าย |
| v0.3 | Planner | แยก intent และสร้าง plan |
| v0.4 | Executor + Tools | รัน plan ตาม step และแยก tools |
| v0.5 | Interactive Agent + Memory | พิมพ์คุยต่อเนื่องผ่าน terminal และจำ session ได้ |

---

## 2. Final App Structure v0.5

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
    └── ... compiled JavaScript files
```

---

## 3. package.json

```json
{
  "name": "my-typescript",
  "version": "1.0.0",
  "description": "AI Agent prototype with Node.js and TypeScript",
  "main": "dist/app.js",
  "scripts": {
    "build": "tsc",
    "start": "node dist/app.js",
    "dev": "nodemon --exec ts-node src/app.ts"
  },
  "dependencies": {},
  "devDependencies": {
    "@types/node": "^25.6.0",
    "nodemon": "^3.1.14",
    "ts-node": "^10.9.2",
    "typescript": "^6.0.3"
  }
}
```

### คำอธิบาย

| Script | หน้าที่ |
|---|---|
| `npm run build` | compile TypeScript เป็น JavaScript ใน `dist/` |
| `npm start` | รันไฟล์ compiled แล้วจาก `dist/app.js` |
| `npm run dev` | รันแบบ development ด้วย `ts-node` + `nodemon` |

---

## 4. tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "./src",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "skipLibCheck": true
  },
  "include": ["src"]
}
```

### คำอธิบาย

| Option | ความหมาย |
|---|---|
| `target` | กำหนด JavaScript output เป็น ES2020 |
| `module` | ใช้ CommonJS เพื่อให้รันกับ Node.js ง่าย |
| `rootDir` | source code อยู่ใน `src` |
| `outDir` | compiled output อยู่ใน `dist` |
| `strict` | เปิด strict type checking |
| `esModuleInterop` | ช่วยให้ import package บางตัวง่ายขึ้น |

---

# v0.1 — Basic TypeScript App

## โครงสร้าง

```text
src/
└── app.ts
```

## src/app.ts

```ts
/**
 * v0.1 Basic TypeScript App
 * -------------------------
 * เป้าหมาย:
 * - ทดสอบว่า TypeScript compile และ Node.js run ได้
 */

const message: string = "Hello TypeScript AI Agent";

console.log(message);
```

## วิธีรัน

```bash
npm run build
npm start
```

---

# v0.2 — Basic Agent Function

## แนวคิด

เริ่มสร้าง function สำหรับ Agent แบบง่าย โดยยังไม่มี planner/executor แยกไฟล์

## src/app.ts

```ts
/**
 * v0.2 Basic Agent Function
 * -------------------------
 * เป้าหมาย:
 * - สร้าง function runAgent()
 * - รับ goal เป็น string
 * - แสดงผลการวิเคราะห์แบบง่าย
 */

function runAgent(goal: string): void {
  console.log("=== AI Agent v0.2 ===");
  console.log(`Goal: ${goal}`);
  console.log("Status: Agent received the goal successfully.");
}

runAgent("ตรวจความเสี่ยงด้านแรงงาน");
```

---

# v0.3 — Planner

## โครงสร้าง

```text
src/
├── app.ts
└── agent/
    └── planner.ts
```

---

## src/agent/planner.ts

```ts
/**
 * planner.ts
 * -------------------------
 * หน้าที่:
 * - วิเคราะห์ intent ของผู้ใช้
 * - สร้าง plan เบื้องต้นให้ Agent
 *
 * Library/API:
 * - ไม่มี external library
 * - ใช้ TypeScript type และ string matching
 */

export type AgentIntent = "compliance" | "audit" | "risk" | "general";

export type PlanStep = {
  id: number;
  action: string;
  description: string;
  input: string;
};

export function detectIntent(goal: string): AgentIntent {
  const text = goal.toLowerCase();

  if (
    text.includes("compliance") ||
    text.includes("legal") ||
    text.includes("กฎหมาย") ||
    text.includes("แรงงาน") ||
    text.includes("license") ||
    text.includes("ใบอนุญาต")
  ) {
    return "compliance";
  }

  if (
    text.includes("audit") ||
    text.includes("ตรวจ") ||
    text.includes("ตรวจสอบ") ||
    text.includes("finding") ||
    text.includes("evidence")
  ) {
    return "audit";
  }

  if (
    text.includes("risk") ||
    text.includes("ความเสี่ยง") ||
    text.includes("overdue") ||
    text.includes("gap")
  ) {
    return "risk";
  }

  return "general";
}

export function makePlan(goal: string): { intent: AgentIntent; plan: PlanStep[] } {
  const intent = detectIntent(goal);

  const plan: PlanStep[] = [
    {
      id: 1,
      action: "analyze",
      description: "วิเคราะห์คำขอของผู้ใช้",
      input: goal,
    },
    {
      id: 2,
      action: "summarize",
      description: "สรุปผลลัพธ์ให้ผู้ใช้",
      input: goal,
    },
  ];

  return { intent, plan };
}
```

---

## src/app.ts

```ts
/**
 * v0.3 Planner Version
 * -------------------------
 * เป้าหมาย:
 * - เรียก makePlan()
 * - แสดง intent และ plan
 */

import { makePlan } from "./agent/planner";

const goal = "ตรวจความเสี่ยงด้านกฎหมายแรงงาน";
const { intent, plan } = makePlan(goal);

console.log("=== AI Agent v0.3 ===");
console.log(`Goal: ${goal}`);
console.log(`Detected intent: ${intent}`);

console.log("Plan:");
for (const step of plan) {
  console.log(`- [${step.id}] ${step.action}: ${step.description}`);
}
```

---

# v0.4 — Executor + Tools

## โครงสร้าง

```text
src/
├── app.ts
└── agent/
    ├── planner.ts
    ├── executor.ts
    └── tools.ts
```

---

## src/agent/planner.ts

```ts
/**
 * planner.ts
 * -------------------------
 * หน้าที่:
 * - วิเคราะห์ intent
 * - สร้าง execution plan ให้ executor
 */

export type AgentIntent = "compliance" | "audit" | "risk" | "general";

export type PlanStep = {
  id: number;
  action:
    | "analyze"
    | "search_compliance"
    | "search_audit"
    | "search_risk"
    | "summarize";
  description: string;
  input: string;
};

export function detectIntent(goal: string): AgentIntent {
  const text = goal.toLowerCase();

  if (
    text.includes("compliance") ||
    text.includes("legal") ||
    text.includes("กฎหมาย") ||
    text.includes("แรงงาน") ||
    text.includes("license") ||
    text.includes("ใบอนุญาต")
  ) {
    return "compliance";
  }

  if (
    text.includes("audit") ||
    text.includes("ตรวจ") ||
    text.includes("ตรวจสอบ") ||
    text.includes("finding") ||
    text.includes("evidence")
  ) {
    return "audit";
  }

  if (
    text.includes("risk") ||
    text.includes("ความเสี่ยง") ||
    text.includes("overdue") ||
    text.includes("gap")
  ) {
    return "risk";
  }

  return "general";
}

export function makePlan(goal: string): { intent: AgentIntent; plan: PlanStep[] } {
  const intent = detectIntent(goal);

  const plan: PlanStep[] = [
    {
      id: 1,
      action: "analyze",
      description: "วิเคราะห์คำขอของผู้ใช้",
      input: goal,
    },
  ];

  if (intent === "compliance") {
    plan.push({
      id: 2,
      action: "search_compliance",
      description: "ค้นหาข้อมูลด้าน compliance / legal register / license",
      input: goal,
    });
  } else if (intent === "audit") {
    plan.push({
      id: 2,
      action: "search_audit",
      description: "ค้นหาข้อมูลด้าน audit / evidence / finding",
      input: goal,
    });
  } else if (intent === "risk") {
    plan.push({
      id: 2,
      action: "search_risk",
      description: "ค้นหาข้อมูลด้านความเสี่ยง / overdue / gap",
      input: goal,
    });
  } else {
    plan.push({
      id: 2,
      action: "search_risk",
      description: "ค้นหาข้อมูลทั่วไปเบื้องต้น",
      input: goal,
    });
  }

  plan.push({
    id: 3,
    action: "summarize",
    description: "สรุปผลลัพธ์ให้ผู้ใช้",
    input: goal,
  });

  return { intent, plan };
}
```

---

## src/agent/tools.ts

```ts
/**
 * tools.ts
 * -------------------------
 * หน้าที่:
 * - รวม tools ที่ Agent ใช้ทำงานจริง
 * - v0.4 ยังเป็น rule-based / hardcoded output
 */

export function analyzeRequest(input: string): string {
  return `วิเคราะห์คำขอแล้ว: "${input}"`;
}

export function searchCompliance(input: string): string {
  return [
    `Compliance review สำหรับ: "${input}"`,
    "- ประเด็นหลัก: legal register, license expiry, regulatory update",
    "- สิ่งที่ควรตรวจ: สถานะใบอนุญาต, action overdue, owner ของแต่ละ requirement",
    "- ข้อสังเกตเบื้องต้น: ถ้าไม่มีทะเบียนกฎหมายล่าสุดหรือไม่มีผู้รับผิดชอบชัดเจน ถือว่าเสี่ยง",
  ].join("\n");
}

export function searchAudit(input: string): string {
  return [
    `Audit review สำหรับ: "${input}"`,
    "- ประเด็นหลัก: evidence review, control gap, finding classification",
    "- สิ่งที่ควรตรวจ: เอกสารหลักฐาน, control owner, due date ของ corrective action",
    "- ข้อสังเกตเบื้องต้น: ถ้าหลักฐานไม่ครบหรือปิด action ไม่ทัน อาจเกิด repeat finding",
  ].join("\n");
}

export function searchRisk(input: string): string {
  return [
    `Risk review สำหรับ: "${input}"`,
    "- ประเด็นหลัก: overdue items, unresolved gap, operational/legal risk",
    "- สิ่งที่ควรตรวจ: อายุงานค้าง, impact, likelihood, responsible owner",
    "- ข้อสังเกตเบื้องต้น: งานค้างนาน + ไม่มี owner ชัด = ความเสี่ยงสูง",
  ].join("\n");
}

export function summarizeResult(input: string, previousResults: string[]): string {
  return [
    `สรุปผลสำหรับเป้าหมาย: "${input}"`,
    "",
    "ผลการประเมิน:",
    ...previousResults.map((item, index) => `${index + 1}. ${item}`),
    "",
    "ข้อเสนอแนะเบื้องต้น:",
    "- ยืนยัน owner ของแต่ละประเด็น",
    "- จัดลำดับความสำคัญตามความเสี่ยง",
    "- กำหนด due date และติดตามสถานะ",
  ].join("\n");
}
```

---

## src/agent/executor.ts

```ts
/**
 * executor.ts
 * -------------------------
 * หน้าที่:
 * - รับ plan จาก planner
 * - รันทีละ step
 * - เรียก tool ตาม action
 */

import { PlanStep } from "./planner";
import {
  analyzeRequest,
  searchAudit,
  searchCompliance,
  searchRisk,
  summarizeResult,
} from "./tools";

export async function executePlan(goal: string, plan: PlanStep[]): Promise<string[]> {
  const results: string[] = [];

  for (const step of plan) {
    let output = "";

    switch (step.action) {
      case "analyze":
        output = analyzeRequest(step.input);
        break;

      case "search_compliance":
        output = searchCompliance(step.input);
        break;

      case "search_audit":
        output = searchAudit(step.input);
        break;

      case "search_risk":
        output = searchRisk(step.input);
        break;

      case "summarize":
        output = summarizeResult(goal, results);
        break;

      default:
        output = `ไม่รู้จัก action: ${String(step.action)}`;
    }

    console.log(`Step ${step.id}: ${step.description}`);
    console.log(`${output}\n`);

    results.push(output);
  }

  return results;
}
```

---

## src/app.ts

```ts
/**
 * v0.4 Executor + Tools Version
 * -------------------------
 * เป้าหมาย:
 * - สร้าง plan
 * - execute plan
 * - แสดง final results
 */

import { executePlan } from "./agent/executor";
import { makePlan } from "./agent/planner";

async function main(): Promise<void> {
  const goal = "ตรวจความเสี่ยงด้านกฎหมายแรงงาน";

  const { intent, plan } = makePlan(goal);

  console.log("=== AI Agent v0.4 ===");
  console.log(`Goal: ${goal}`);
  console.log(`Detected intent: ${intent}\n`);

  console.log("Plan:");
  for (const step of plan) {
    console.log(`- [${step.id}] ${step.action}: ${step.description}`);
  }
  console.log("");

  const results = await executePlan(goal, plan);

  console.log("=== Final Results ===");
  results.forEach((result, index) => {
    console.log(`\n[Result ${index + 1}]`);
    console.log(result);
  });
}

main().catch((error) => {
  console.error("Agent error:", error);
});
```

---

# v0.5 — Interactive Agent + Memory

## โครงสร้าง

```text
src/
├── app.ts
└── agent/
    ├── planner.ts
    ├── executor.ts
    ├── tools.ts
    └── memory.ts
```

---

## src/agent/memory.ts

```ts
/**
 * memory.ts
 * -------------------------
 * หน้าที่:
 * - เก็บ memory ของ Agent ระหว่างที่โปรแกรมยังรันอยู่
 * - ใช้เป็น in-memory store ชั่วคราว
 *
 * ข้อจำกัด:
 * - ข้อมูลจะหายเมื่อปิดโปรแกรม
 * - ยังไม่ใช่ persistent memory
 */

export type MemoryRecord = {
  goal: string;
  intent: string;
  results: string[];
  timestamp: string;
};

const memoryStore: MemoryRecord[] = [];

export function saveMemory(record: MemoryRecord): void {
  memoryStore.push(record);
}

export function getLatestMemory(): MemoryRecord | null {
  if (memoryStore.length === 0) {
    return null;
  }

  return memoryStore[memoryStore.length - 1];
}

export function getAllMemories(): MemoryRecord[] {
  return memoryStore;
}

export function formatMemoryHistory(): string {
  if (memoryStore.length === 0) {
    return "ยังไม่มีประวัติการทำงาน";
  }

  return memoryStore
    .map((item, index) => {
      return [
        `[${index + 1}]`,
        `Goal: ${item.goal}`,
        `Intent: ${item.intent}`,
        `Time: ${item.timestamp}`,
      ].join("\n");
    })
    .join("\n\n");
}
```

---

## src/agent/planner.ts

```ts
/**
 * planner.ts
 * -------------------------
 * หน้าที่:
 * - วิเคราะห์ intent
 * - สร้าง plan 4 step:
 *   1) recall memory
 *   2) analyze
 *   3) search tool
 *   4) summarize
 */

export type AgentIntent = "compliance" | "audit" | "risk" | "general";

export type PlanStep = {
  id: number;
  action:
    | "recall_memory"
    | "analyze"
    | "search_compliance"
    | "search_audit"
    | "search_risk"
    | "summarize";
  description: string;
  input: string;
};

export function detectIntent(goal: string): AgentIntent {
  const text = goal.toLowerCase();

  if (
    text.includes("compliance") ||
    text.includes("กฎหมาย") ||
    text.includes("ข้อกฎหมาย") ||
    text.includes("legal") ||
    text.includes("license") ||
    text.includes("ใบอนุญาต") ||
    text.includes("แรงงาน") ||
    text.includes("ชั่วโมงการทำงาน") ||
    text.includes("เวลาทำงาน") ||
    text.includes("ค่าจ้าง") ||
    text.includes("ค่าล่วงเวลา") ||
    text.includes("ot") ||
    text.includes("วันหยุด") ||
    text.includes("ลา") ||
    text.includes("เลิกจ้าง") ||
    text.includes("ประกันสังคม")
  ) {
    return "compliance";
  }

  if (
    text.includes("audit") ||
    text.includes("ตรวจ") ||
    text.includes("ตรวจสอบ") ||
    text.includes("finding") ||
    text.includes("evidence")
  ) {
    return "audit";
  }

  if (
    text.includes("risk") ||
    text.includes("ความเสี่ยง") ||
    text.includes("overdue") ||
    text.includes("gap")
  ) {
    return "risk";
  }

  return "general";
}

export function makePlan(goal: string): { intent: AgentIntent; plan: PlanStep[] } {
  const intent = detectIntent(goal);

  const base: PlanStep[] = [
    {
      id: 1,
      action: "recall_memory",
      description: "ดึงบริบทล่าสุดจาก memory",
      input: goal,
    },
    {
      id: 2,
      action: "analyze",
      description: "วิเคราะห์คำขอของผู้ใช้",
      input: goal,
    },
  ];

  if (intent === "compliance") {
    base.push({
      id: 3,
      action: "search_compliance",
      description: "ค้นหาข้อมูลด้าน compliance / legal register / license",
      input: goal,
    });
  } else if (intent === "audit") {
    base.push({
      id: 3,
      action: "search_audit",
      description: "ค้นหาข้อมูลด้าน audit / evidence / finding",
      input: goal,
    });
  } else if (intent === "risk") {
    base.push({
      id: 3,
      action: "search_risk",
      description: "ค้นหาข้อมูลด้านความเสี่ยง / overdue / gap",
      input: goal,
    });
  } else {
    base.push({
      id: 3,
      action: "search_risk",
      description: "ค้นหาข้อมูลทั่วไปเบื้องต้น",
      input: goal,
    });
  }

  base.push({
    id: 4,
    action: "summarize",
    description: "สรุปผลลัพธ์ให้ผู้ใช้",
    input: goal,
  });

  return { intent, plan: base };
}
```

---

## src/agent/tools.ts

```ts
/**
 * tools.ts
 * -------------------------
 * หน้าที่:
 * - รวม tools ที่ executor เรียกใช้
 * - v0.5 ใช้ memory เพื่อ recall context ล่าสุด
 */

import { getLatestMemory } from "./memory";

export function recallLatestContext(): string {
  const latest = getLatestMemory();

  if (!latest) {
    return "ไม่พบ memory ก่อนหน้า";
  }

  return [
    "พบบริบทล่าสุด:",
    `- Goal ล่าสุด: ${latest.goal}`,
    `- Intent ล่าสุด: ${latest.intent}`,
    `- เวลา: ${latest.timestamp}`,
  ].join("\n");
}

export function analyzeRequest(input: string): string {
  const latest = getLatestMemory();

  if (!latest) {
    return `วิเคราะห์คำขอแล้ว: "${input}"`;
  }

  return [
    `วิเคราะห์คำขอแล้ว: "${input}"`,
    `บริบทก่อนหน้า: "${latest.goal}" (${latest.intent})`,
  ].join("\n");
}

export function searchCompliance(input: string): string {
  return [
    `Compliance review สำหรับ: "${input}"`,
    "- ประเด็นหลัก: legal register, license expiry, regulatory update, labour law",
    "- สิ่งที่ควรตรวจ: สถานะใบอนุญาต, action overdue, owner ของแต่ละ requirement",
    "- ข้อสังเกตเบื้องต้น: ถ้าไม่มีทะเบียนกฎหมายล่าสุดหรือไม่มีผู้รับผิดชอบชัดเจน ถือว่าเสี่ยง",
  ].join("\n");
}

export function searchAudit(input: string): string {
  return [
    `Audit review สำหรับ: "${input}"`,
    "- ประเด็นหลัก: evidence review, control gap, finding classification",
    "- สิ่งที่ควรตรวจ: เอกสารหลักฐาน, control owner, due date ของ corrective action",
    "- ข้อสังเกตเบื้องต้น: ถ้าหลักฐานไม่ครบหรือปิด action ไม่ทัน อาจเกิด repeat finding",
  ].join("\n");
}

export function searchRisk(input: string): string {
  return [
    `Risk review สำหรับ: "${input}"`,
    "- ประเด็นหลัก: overdue items, unresolved gap, operational/legal risk",
    "- สิ่งที่ควรตรวจ: อายุงานค้าง, impact, likelihood, responsible owner",
    "- ข้อสังเกตเบื้องต้น: งานค้างนาน + ไม่มี owner ชัด = ความเสี่ยงสูง",
  ].join("\n");
}

export function summarizeResult(input: string, previousResults: string[]): string {
  return [
    `สรุปผลสำหรับเป้าหมาย: "${input}"`,
    "",
    "ผลการประเมิน:",
    ...previousResults.map((item, index) => `${index + 1}. ${item}`),
    "",
    "ข้อเสนอแนะเบื้องต้น:",
    "- ยืนยัน owner ของแต่ละประเด็น",
    "- จัดลำดับความสำคัญตามความเสี่ยง",
    "- กำหนด due date และติดตามสถานะ",
  ].join("\n");
}
```

---

## src/agent/executor.ts

```ts
/**
 * executor.ts
 * -------------------------
 * หน้าที่:
 * - รับ plan จาก planner
 * - เรียก tools ตาม action
 * - เก็บผลลัพธ์แต่ละ step
 */

import { PlanStep } from "./planner";
import {
  analyzeRequest,
  recallLatestContext,
  searchAudit,
  searchCompliance,
  searchRisk,
  summarizeResult,
} from "./tools";

export async function executePlan(goal: string, plan: PlanStep[]): Promise<string[]> {
  const results: string[] = [];

  for (const step of plan) {
    let output = "";

    switch (step.action) {
      case "recall_memory":
        output = recallLatestContext();
        break;

      case "analyze":
        output = analyzeRequest(step.input);
        break;

      case "search_compliance":
        output = searchCompliance(step.input);
        break;

      case "search_audit":
        output = searchAudit(step.input);
        break;

      case "search_risk":
        output = searchRisk(step.input);
        break;

      case "summarize":
        output = summarizeResult(goal, results);
        break;

      default:
        output = `ไม่รู้จัก action: ${String(step.action)}`;
    }

    console.log(`Step ${step.id}: ${step.description}`);
    console.log(`${output}\n`);

    results.push(output);
  }

  return results;
}
```

---

## src/app.ts

```ts
/**
 * app.ts
 * -------------------------
 * หน้าที่:
 * - เป็น entry point ของโปรแกรม
 * - เปิด interactive terminal
 * - รับคำสั่งผู้ใช้
 * - เรียก planner + executor
 * - บันทึก memory
 *
 * Library/API:
 * - readline เป็น Node.js built-in module สำหรับรับ input จาก terminal
 */

import readline from "readline";
import { executePlan } from "./agent/executor";
import { formatMemoryHistory, getAllMemories, saveMemory } from "./agent/memory";
import { makePlan } from "./agent/planner";

function askQuestion(rl: readline.Interface, question: string): Promise<string> {
  return new Promise((resolve) => {
    rl.question(question, resolve);
  });
}

async function runAgent(goal: string): Promise<void> {
  const { intent, plan } = makePlan(goal);

  console.log("\n=== AI Agent v0.5 ===");
  console.log(`Goal: ${goal}`);
  console.log(`Detected intent: ${intent}\n`);

  console.log("Plan:");
  for (const step of plan) {
    console.log(`- [${step.id}] ${step.action}: ${step.description}`);
  }
  console.log("");

  const results = await executePlan(goal, plan);

  saveMemory({
    goal,
    intent,
    results,
    timestamp: new Date().toISOString(),
  });

  console.log("=== Final Results ===");
  results.forEach((result, index) => {
    console.log(`\n[Result ${index + 1}]`);
    console.log(result);
  });

  console.log("\n=== Memory Count ===");
  console.log(getAllMemories().length);
  console.log("");
}

async function main(): Promise<void> {
  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  console.log("Interactive AI Agent started.");
  console.log('พิมพ์คำสั่งได้เลย');
  console.log('- พิมพ์ "history" เพื่อดูประวัติ');
  console.log('- พิมพ์ "exit" เพื่อออก\n');

  while (true) {
    const input = (await askQuestion(rl, "> ")).trim();

    if (!input) {
      continue;
    }

    if (input.toLowerCase() === "exit") {
      console.log("Goodbye.");
      break;
    }

    if (input.toLowerCase() === "history") {
      console.log("\n=== Memory History ===");
      console.log(formatMemoryHistory());
      console.log("");
      continue;
    }

    await runAgent(input);
  }

  rl.close();
}

main().catch((error) => {
  console.error("Agent error:", error);
});
```

---

# 5. คำสั่งติดตั้งและรัน

```bash
npm install
npm run build
npm start
```

หรือโหมด dev:

```bash
npm run dev
```

---

# 6. ตัวอย่างการทดสอบ v0.5

```text
ชั่วโมงการทำงาน
audit evidence payroll
risk overdue corrective action
history
exit
```

---

# 7. ข้อจำกัดของ v0.1–v0.5

| ประเด็น | สถานะ |
|---|---|
| OpenAI API | ยังไม่เชื่อม |
| Database | ยังไม่มี |
| Persistent Memory | ยังไม่มี |
| Web UI | ยังไม่มี |
| Real Legal Knowledge Base | ยังไม่มี |
| Dashboard | ยังไม่มี |

---

# 8. แนวทางต่อยอด

| เวอร์ชันถัดไป | สิ่งที่ควรเพิ่ม |
|---|---|
| v0.6 | เชื่อม OpenAI API |
| v0.7 | Thai Labour Law Domain Intelligence |
| v0.8 | Persistent Memory JSON / DB |
| v0.9 | Website UI |
| v1.0 | Full Stack AI Labour Dashboard |

---

# 9. สรุป

AI Agent v0.1–v0.5 เป็นรากฐานสำคัญของระบบ โดยเริ่มจาก TypeScript app ธรรมดา แล้วค่อย ๆ แยกเป็น architecture ที่มี Planner, Executor, Tools และ Memory

แม้ยังเป็น rule-based prototype แต่โครงสร้างนี้เหมาะสำหรับต่อยอดไปสู่ production system เช่น

- Labour Compliance Copilot
- HR Audit Assistant
- Legal Register AI Agent
- AI Labour Dashboard
- Knowledge Management Agent
