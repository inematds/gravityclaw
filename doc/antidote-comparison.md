# Análise Comparativa — Antidote vs Gravity Claw Feature Stores

> Comparação do repositório https://github.com/inematds/antidote com os 4 HTMLs do Gravity Claw,
> do ponto de vista de estrutura, output gerado e portabilidade para diferentes ferramentas de IA.

---

## Estrutura dos Repositórios

### Antidote

```
index.html                          ← landing page (2 steps: Compare → Build)
doc/framework-comparison-pt.html   ← "grocery store" dos 9 frameworks
doc/como-construir.html            ← guia passo-a-passo completo
```

**Fluxo:** Comprar features → Baixar `.md` → Dar ao Claude Code → `docker compose up`

### Gravity Claw

```
index.html                          ← hub de recursos / landing page
doc/gravity-claw.html              ← feature store original (roxo)
doc/gravity-claw-store.html        ← clone do Vercel (teal, 11 cats)
doc/gravity-claw-antigravity.html  ← store para AntiGravity (CLAWS)
doc/gravity-claw-claudecode.html   ← store para Claude Code (CLAUDE.md)
```

**Fluxo:** Selecionar features → Copiar prompt → Colar na ferramenta escolhida

---

## O que cada um gera

| | Antidote | Gravity Claw (store) | Gravity Claw (antigravity) | Gravity Claw (claudecode) |
|---|---|---|---|---|
| **Formato do output** | `.md` (download de arquivo) | Clipboard (prompt) | Clipboard (prompt CLAWS) | Clipboard (CLAUDE.md) |
| **Conteúdo** | Spec + referências de código por repo | Stack + lista de features | CLAWS steps + Opus 4.5 | CLAUDE.md com file paths |
| **Referência de implementação** | ✅ aponta repo + pasta exata | ❌ não | ❌ não | ⚠️ aponta arquivo (`src/agent.ts`) |
| **Tool-specific** | ❌ genérico | ⚠️ parcial | ✅ AntiGravity only | ✅ Claude Code only |
| **Portável para Codex/Gemini** | ✅ sim | ✅ sim | ❌ não | ❌ não |

---

## Diferença Crítica: o Output

### Antidote gera algo assim:

```markdown
## Memória

### SQLite FTS5

**What it does**: BM25 full-text search sem custo de embeddings

**Reference implementations**: NanoClaw, OpenClaw, IronClaw

**Where to find reference code**:
- NanoClaw (./nanoclaw/): check src/memory/
- OpenClaw (./openclaw/): check skills/memory.js

---

## Implementation Notes for Claude Code
1. Start with the reference code — each item lists which repos implement it
2. Prefer simplicity — if NanoClaw and OpenClaw both implement, start com NanoClaw
```

### Gravity Claw (claudecode) gera algo assim:

```markdown
## Superhuman Memory (Level 2)

- **SQLite Conversation Buffer** — histórico local com better-sqlite3 → `src/memory/db.ts`
- **FTS5 Full-Text Search** — busca eficiente no histórico → `src/memory/db.ts`
- **Supabase + pgvector** — busca semântica persistente → `src/memory/vec.ts`
```

**Diferença real:** O Antidote diz *onde no código de referência encontrar a implementação*. O Gravity Claw diz *onde no seu próprio projeto criar o arquivo*. São utilidades complementares — um é **discovery**, o outro é **scaffolding**.

---

## Comparação por Dimensão

### 1. Propósito

| | Antidote | Gravity Claw stores |
|---|---|---|
| **Para quê** | Comparar frameworks e montar spec | Selecionar features e gerar prompt/config |
| **Ponto de partida** | "Qual framework usar?" | "O que implementar no Gravity Claw?" |
| **Foco** | Ecosystem-level (9 frameworks) | Project-level (1 projeto específico) |

### 2. Feature Data

| | Antidote | Gravity Claw stores |
|---|---|---|
| **Categorias** | 9 (Identidade, Segurança, Comunicação, Memória, Automação, Integrações, Skills, Arquitetura, Design Patterns) | 11 (Core, Messaging, Memory, Voice, LLM, Tools, Proactive, Security, Architecture, Deploy, UX) |
| **Features por item** | Nome + descrição + lista de repos que implementam | Nome + subdescição + tag + caminho de arquivo |
| **Subdescrições** | ❌ não | ✅ "listens to voice notes", "blue bubbles via Mac" |
| **Referências externas** | ✅ 9 repos com pastas | ❌ apenas file paths internos |
| **Granularidade** | Média (feature-level) | Alta (sub-feature level) |

### 3. UX e Design

| | Antidote | Gravity Claw stores |
|---|---|---|
| **Tema visual** | Terminal/dark green (#00D26A), monospace, ASCII | Teal/amber/purple, Outfit font, glassmorphism |
| **Estilo** | Minimalista, hacker aesthetic | Polished, produto SaaS |
| **Landing page** | 2 cards simples + ASCII banner | Hub completo com seções, tabelas, arquitetura |
| **Guia de uso** | ✅ `como-construir.html` detalhado | ❌ não existe |
| **Sticky header** | ❌ não | ✅ sim |
| **Filtro por categoria** | ✅ sim | ✅ sim |
| **Busca de features** | ⚠️ básica | ✅ real-time search |

### 4. Output e Portabilidade

| | Antidote | Gravity Claw (original) | Gravity Claw (store) | Gravity Claw (antigravity) | Gravity Claw (claudecode) |
|---|:---:|:---:|:---:|:---:|:---:|
| Claude Code | ✅ | ⚠️ | ✅ | ⚠️ | ✅ nativo |
| AntiGravity | ✅ | ✅ | ✅ | ✅ nativo | ⚠️ |
| Codex / OpenAI | ✅ | ⚠️ | ✅ | ❌ | ❌ |
| Gemini | ✅ | ⚠️ | ✅ | ❌ | ❌ |
| Cursor | ✅ | ⚠️ | ✅ | ❌ | ⚠️ |
| **Download de arquivo** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Só clipboard** | ❌ | ✅ | ✅ | ✅ | ✅ |

**Nota:** O Antidote **baixa** o arquivo — o usuário recebe um `.md` que pode abrir no Finder/Explorer e arrastar para qualquer ferramenta. Os Gravity Claw stores copiam para clipboard — funciona mas é menos robusto para outputs longos.

---

## O que cada um faz melhor

### Antidote faz melhor:

1. **Tool-agnostic** — o `.md` gerado não menciona AntiGravity nem Claude Code; é uma spec pura que qualquer LLM entende
2. **Referências de código** — aponta repo + pasta para cada feature; Claude Code pode ir buscar o código de referência diretamente
3. **Download de arquivo** — mais robusto que clipboard para documentos longos; pode ser versionado e compartilhado
4. **Guia de build** — `como-construir.html` explica o workflow completo passo-a-passo; Gravity Claw não tem equivalente
5. **Comparação entre frameworks** — permite ver o que cada um dos 9 projetos implementa antes de escolher

### Gravity Claw faz melhor:

1. **Subdescrições por feature** — "listens to voice notes", "blue bubbles via Mac" — mais fácil de entender o que cada item faz
2. **File path hints** — `src/agent.ts`, `mcp.json` — já diz onde criar o arquivo no projeto
3. **Outputs especializados por ferramenta** — CLAUDE.md formatado corretamente para Claude Code; prompt CLAWS para AntiGravity
4. **Visual mais polido** — sticky headers, glassmorphism, animações, tags coloridas
5. **Hub central** — `index.html` como portal com arquitetura, build levels, dev workflow, tabela comparativa
6. **Build Levels explícitos** — L1→L5 com ordem de implementação clara
7. **Mais categorias** — 11 vs 9; inclui LLM & Models, Proactive Behavior, UX & Interaction

---

## O que está faltando em cada um

### Antidote precisa de:
- Subdescrições por feature (o que exatamente cada item faz)
- Build order / sequência de implementação (qual feature implementar primeiro)
- Output especializado — poder gerar CLAUDE.md ou prompt AntiGravity além do genérico
- Busca em tempo real mais robusta

### Gravity Claw precisa de:
- **Referências de implementação** — apontar qual repo open-source implementa cada feature
- **Download de arquivo** além de clipboard
- **Output genérico tool-agnostic** — um terceiro botão "Gerar Spec Genérica"
- **Guia de build** — equivalente ao `como-construir.html` do Antidote

---

## Síntese

Os dois projetos resolvem problemas complementares e **não são concorrentes — são sequenciais**:

```
Antidote                          →   Gravity Claw
(Qual framework usar? O que       →   (Como implementar no meu projeto?
 existe no ecossistema?)               Quais features, em que ordem?)
```

O fluxo ideal seria:
1. **Antidote** — compara os 9 frameworks, seleciona ingredientes, baixa a spec
2. **Gravity Claw claudecode** — seleciona features específicas do projeto, gera o CLAUDE.md
3. **Claude Code** — lê ambos os documentos e scaffolda o projeto

O Antidote é um **catálogo do ecossistema**. O Gravity Claw é um **configurador do projeto específico**.

---

*Gerado em 2026-02-22 — análise dos repositórios https://github.com/inematds/antidote e https://github.com/inematds/gravityclaw*
