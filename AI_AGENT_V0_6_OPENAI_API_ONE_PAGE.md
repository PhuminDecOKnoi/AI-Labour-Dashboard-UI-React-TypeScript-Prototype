# AI Agent v0.6 — OpenAI API Integration (TypeScript)

> One-page Markdown สำหรับโปรเจกต์ AI Agent ที่เชื่อม OpenAI API จริงผ่าน Node.js + TypeScript

## 1) Objective

เป้าหมายของ v0.6 คืออัปเกรดจาก **AI Agent v0.5 แบบ rule-based / terminal prototype** ให้สามารถเรียก **OpenAI API** ได้จริง โดยยังคงโครงสร้างหลักเดิมไว้ ได้แก่ `planner`, `executor`, `tools`, และ `memory`

```text
User Input
  → planner.ts
  → executor.ts
  → tools.ts
  → services/openai.ts
  → OpenAI Responses API
  → Final Answer
```

## 2) Required Libraries

```bash
npm install openai dotenv
npm install -D typescript ts-node nodemon @types/node
```

| Library | Purpose |
|---|---|
| `openai` | Official OpenAI Node.js / TypeScript SDK |
| `dotenv` | โหลดค่า `.env` เช่น `OPENAI_API_KEY` |
| `typescript` | compile TypeScript เป็น JavaScript |
| `ts-node` | run TypeScript ระหว่าง dev |
| `nodemon` | restart app อัตโนมัติเมื่อแก้ไฟล์ |
| `@types/node` | Type definitions สำหรับ Node.js |

## 3) Environment Variables

สร้างไฟล์ `.env` ที่ root project

```env
OPENAI_API_KEY=sk-proj-your_api_key_here
OPENAI_MODEL=gpt-5.2
```

> Security note: ห้าม commit `.env` เข้า GitHub

เพิ่ม `.gitignore`

```gitignore
node_modules
dist
.env
```

## 4) Recommended File Structure

```text
src/
  app.ts
  agent/
    planner.ts
    executor.ts
    tools.ts
    memory.ts
  services/
    openai.ts
```

## 5) `src/services/openai.ts`

```ts
// โหลด environment variables จากไฟล์ .env
// ต้องอยู่ก่อนสร้าง OpenAI client เพื่อให้ process.env.OPENAI_API_KEY พร้อมใช้งาน
import "dotenv/config";

// Official OpenAI SDK สำหรับ Node.js / TypeScript
import OpenAI from "openai";

// สร้าง client สำหรับเรียก OpenAI API
// apiKey อ่านจาก process.env.OPENAI_API_KEY
const client = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

// อ่านชื่อ model จาก .env
// ถ้าไม่กำหนด จะใช้ fallback model
const model = process.env.OPENAI_MODEL || "gpt-5.2";

/**
 * askOpenAI()
 * -------------------------
 * หน้าที่:
 * - รับ input จาก agent tool
 * - ส่งเข้า OpenAI Responses API
 * - คืนข้อความ output_text กลับไปให้ tools / executor
 */
export async function askOpenAI(
  input: string,
  instructions?: string
): Promise<string> {
  try {
    const response = await client.responses.create({
      model,
      instructions:
        instructions ||
        "You are a professional Thai compliance, labour law, HR, and audit assistant. Answer clearly and structurally.",
      input,
    });

    return response.output_text || "No response text returned.";
  } catch (error: any) {
    // Error handling สำหรับ dev/debug
    if (error?.code === "insufficient_quota") {
      return "OpenAI API quota ไม่พอ หรือ billing ยังไม่พร้อมใช้งาน";
    }

    if (error?.status === 401) {
      return "OpenAI API key ไม่ถูกต้อง หรือยังไม่ได้ตั้งค่า OPENAI_API_KEY";
    }

    return `OpenAI API error: ${error?.message || "Unknown error"}`;
  }
}
```

## 6) Example Tool Integration

```ts
import { askOpenAI } from "../services/openai";

export async function searchCompliance(input: string): Promise<string> {
  return askOpenAI(
    `คำถาม: ${input}

ช่วยวิเคราะห์ในมุม labour law / HR compliance
จัดหัวข้อ:
1) ประเด็นสำคัญ
2) สิ่งที่ควรตรวจ
3) ความเสี่ยง
4) ข้อเสนอแนะ`,
    "You are a Thai labour law and HR compliance analyst."
  );
}
```

## 7) Important Executor Change

เมื่อ tool เรียก API จริง ต้องเปลี่ยน executor เป็น `async`

```ts
output = await searchCompliance(step.input);
```

และ function หลักควรเป็น

```ts
export async function executePlan(...): Promise<string[]> {
  // run steps
}
```

## 8) Run Commands

```bash
npm run build
npm start
```

หรือระหว่างพัฒนา

```bash
npm run dev
```

ตัวอย่าง `package.json`

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/app.js",
    "dev": "nodemon --exec ts-node src/app.ts"
  }
}
```

## 9) Common Errors

| Error | Meaning | Fix |
|---|---|---|
| `Missing credentials` | ไม่พบ API key | ตรวจ `.env` และ `OPENAI_API_KEY` |
| `401` | key ผิด / key ถูก revoke | สร้าง key ใหม่ |
| `429 insufficient_quota` | quota / billing ไม่พอ | เติมเครดิตหรือเปิด billing |
| `model not found` | model name ไม่ถูกต้อง | ตรวจชื่อ model ใน dashboard |
| `.env not loaded` | dotenv ไม่ทำงาน | ใส่ `import "dotenv/config";` บรรทัดบนสุด |

## 10) Developer Notes

- v0.6 ยังเหมาะกับ local prototype และ internal demo
- ห้ามวาง API key ใน frontend/browser
- ถ้าทำ Website UI จริง ควรให้ React เรียก backend API และให้ backend เป็นคนเรียก OpenAI
- ขั้นต่อไปที่เหมาะสมคือแยกเป็น `server/` และ `client/`
- สำหรับ production ควรเพิ่ม logging, rate limit, request validation, auth, และ persistent memory

## Summary

AI Agent v0.6 คือจุดเปลี่ยนจาก agent ที่ตอบด้วย rule/template ไปเป็น agent ที่มี LLM จริงอยู่ใน workflow โดยใช้ OpenAI SDK ผ่าน `services/openai.ts` แล้วให้ `tools.ts` เรียกใช้งานต่ออย่างเป็นระบบ
