# 🦞 Gravity Claw

> Um agente de IA pessoal lean, seguro e totalmente compreendido — construído do zero em TypeScript.

**[→ Acesse o hub de recursos](https://inematds.github.io/gravityclaw/)** &nbsp;·&nbsp; **[→ Feature Store (live)](https://gravity-claw.vercel.app/)**

---

## O que é

Gravity Claw é um **bot Telegram com loop agêntico**, inspirado no projeto OpenClaw mas construído do zero — para que cada linha seja entendida, auditada e sob controle do dono.

Não é um fork. É uma reimplementação deliberada que resolve os problemas estruturais do original:

| Problema no OpenClaw | Solução no Gravity Claw |
|---|---|
| 42.665 instâncias expostas publicamente | Telegram long-polling — sem portas abertas |
| 341 community skills maliciosas encontradas | MCP apenas — processos separados e auditáveis |
| Custos de $500–$5K/mês por token | Claude Max ($200 fixo) ou modelos locais |
| 100K+ linhas que ninguém leu | Construído brick-by-brick, cada linha revisada |

---

## Princípios Não-Negociáveis

- **Sem web server / sem portas abertas** — Telegram long-polling apenas
- **Whitelist de Telegram ID** — silenciosamente ignora qualquer outro usuário
- **Secrets no `.env` apenas** — nunca em código, logs ou arquivos de memória
- **MCP para integrações** — nenhum skill file externo não-auditado
- **Limite de iterações** no agent loop + confirmação para shell commands perigosos

---

## Tech Stack

| Pacote | Função |
|---|---|
| `grammy` | Telegram bot framework |
| `@anthropic-ai/sdk` | LLM primário (Claude Sonnet 4.6) |
| `better-sqlite3` + FTS5 | Memória de curto prazo / conversation buffer |
| `openai` SDK | Whisper transcription (STT) |
| ElevenLabs API | Text-to-speech |
| `tsx` | Dev runner com hot-reload |
| `node-cron` | Heartbeat / proactive check-ins |

---

## Build Levels

| Level | Foco | Features |
|---|---|---|
| **L1 — Foundation** | Telegram + Claude + agent loop | Um tool: `get_current_time` |
| **L2 — Memory** | SQLite + FTS5 + memory tools | `better-sqlite3`, `soul.md` |
| **L3 — Voice** | Whisper in, ElevenLabs out | Notas de voz bidirecionais |
| **L4 — Tools** | MCP bridge | Shell, files, Gmail, Notion, GitHub |
| **L5 — Heartbeat** | `node-cron` | Briefings diários, accountability |

---

## Recursos

### Feature Stores Interativas

| Arquivo | Descrição |
|---|---|
| [Feature Store — AntiGravity Edition](doc/gravity-claw-antigravity.html) | Seleciona features → gera prompt para o AntiGravity (CLAWS Framework) |
| [Feature Store — Claude Code Edition](doc/gravity-claw-claudecode.html) | Seleciona features → gera `CLAUDE.md` para o Claude Code CLI |
| [Feature Store — Original](doc/gravity-claw.html) | Versão original (7 categorias, tema roxo) |
| [Feature Store — Vercel Clone](doc/gravity-claw-store.html) | Clone completo do site vercel (11 categorias, tema teal) |
| [Feature Store (live)](https://gravity-claw.vercel.app/) | Versão publicada no Vercel |

### Documentos

| Arquivo | Descrição |
|---|---|
| [CLAUDE.md](CLAUDE.md) | Instruções para o Claude Code ao trabalhar neste repo |
| [Prompt de Inicialização](doc/gravity-claw-initialisation-prompt.md) | Prompt para colar no AntiGravity e bootstrapar o projeto do zero |
| [Railway SOP](doc/update-gravity-claw-railway-sop.md) | Procedimento completo de deploy e gestão no Railway |

### Referência Externa

| Link | Descrição |
|---|---|
| [Framework Comparison](https://inematds.github.io/intelecto/doc/framework-comparison-pt.html) | Comparação dos 9 frameworks do ecossistema OpenClaw |

---

## Dev Workflow

```bash
# Antes de testar localmente — evita conflito de instâncias
railway down

# Dev com hot-reload
npm run dev   # tsx watch src/index.ts

# Type-check antes de deployar
npx tsc --noEmit

# Deploy
railway up --detach
railway logs --lines 40
```

**Startup saudável:**
```
✅ Soul loaded (soul.md)
✅ Connected as @your_bot_name
✅ Heartbeat scheduled
```

---

## Memória em 3 Camadas

```
┌─────────────────────────────────┐
│  Tier 1 — Core Memory           │  soul.md → injetado no system prompt
├─────────────────────────────────┤
│  Tier 2 — Conversation Buffer   │  SQLite + FTS5 (reseta no Railway deploy)
├─────────────────────────────────┤
│  Tier 3 — Semantic Long-Term    │  Supabase + pgvector (persiste sempre)
└─────────────────────────────────┘
```

---

*Parte do ecossistema [OpenClaw](https://openclaw.ai) — construído com o [CLAWS Framework](https://gravity-claw.vercel.app/)*
