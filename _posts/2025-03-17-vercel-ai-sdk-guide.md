---
title: "Vercel AI SDK: Membangun Antarmuka AI Modern Secara Cepat dengan React dan Next.js"
author: Ilham
description: Pelajari cara menggunakan Vercel AI SDK untuk menangani streaming response, UI chat yang interaktif, dan integrasi berbagai model AI hanya dalam hitungan menit.
date: 2025-03-17 10:00:00 +0800
categories: [Programing, AI, Frontend]
tags: [vercel, nextjs, react, ai-sdk, frontend, generative-ai]
---

Membangun antarmuka AI yang responsif seperti ChatGPT (dengan teks yang muncul satu per satu atau *streaming*) dulunya cukup rumit. Anda harus berurusan dengan WebSockets atau Server-Sent Events (SSE). 

Namun, Vercel merilis **AI SDK**, sebuah library yang dirancang khusus untuk membuat integrasi AI ke aplikasi React dan Next.js menjadi pengalaman yang luar biasa mudah dan menyenangkan.

---

### 1. Apa itu Vercel AI SDK?

Vercel AI SDK adalah library "wrapper" yang menghubungkan aplikasi frontend Anda dengan berbagai model AI (OpenAI, Anthropic, Google Gemini, dll). SDK ini menangani hal-hal berat seperti:
- **Streaming Response:** Menampilkan kata demi kata saat AI sedang berpikir.
- **Hooks Management:** Menyediakan fungsi seperti `useChat` atau `useCompletion` yang sangat simpel.
- **Satu Interface untuk Semua Model:** Anda bisa mengganti dari OpenAI ke Gemini tanpa mengubah kode frontend Anda.

### 2. Memulai Integrasi

Instalasi paket yang dibutuhkan:
```bash
npm install ai openai lucide-react
```

### 3. Setup di Sisi Server (Next.js App Router)

Buatlah sebuah rute API di `app/api/chat/route.ts`:

```typescript
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = await streamText({
    model: openai('gpt-4o'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

### 4. Menampilkan Chat di Frontend (Magic!)

Berkat hook `useChat`, Anda tidak perlu lagi mengurusi state input atau riwayat pesan secara manual.

```tsx
'use client';

import { useChat } from 'ai/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <div>
      {messages.map(m => (
        <div key={m.id} className={m.role === 'user' ? 'text-blue-500' : 'text-green-500'}>
          {m.content}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} placeholder="Ketik pesan..." />
        <button type="submit">Kirim</button>
      </form>
    </div>
  );
}
```

### 5. Fitur "Generative UI"

Salah satu fitur paling canggih dari SDK ini adalah kemampuan AI untuk merender komponen UI secara dinamis. Misal, jika user bertanya tentang cuaca, AI bukan hanya menjawab teks, tapi bisa merender **Komponen Cuaca** yang asli dan interaktif.

### Kesimpulan

Vercel AI SDK adalah standar baru bagi siapa pun yang ingin membangun produk AI di ekosistem web modern. Ia memangkas waktu pengembangan dari harian menjadi hitungan menit. Jika Anda menggunakan Next.js, ini adalah alat pertama yang harus Anda ambil saat ingin menambahkan fitur AI.

---

**Artikel Sebelumnya:** [Mengenal RAG: Memberi 'Otak' Tambahan pada AI Anda dengan Data Kustom](/posts/introducing-rag-custom-ai-data/)
**Artikel Selanjutnya:** [Etika Penggunaan AI dalam Coding: Apa yang Boleh dan Tidak bagi Developer Profesional?](/posts/ethics-of-ai-coding-guide/)
