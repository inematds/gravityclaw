# Análise Comparativa — Intelecto vs Antidote vs Gravity Claw

> Comparação do repositório https://github.com/inematds/intelecto com os anteriores.
> Gerado em 2026-02-22.

---

## Os Três Projetos em Uma Linha

| Projeto | O que é | Stack | Deploy |
|---|---|---|---|
| **Intelecto** | Documentação + implementação Python de agente pessoal | Python 3.11, LiteLLM, SQLite FTS5 | macOS local (launchd) |
| **Antidote** | Mesmo conceito, mesma base de docs, mesmo grocery store | Python 3.11, LiteLLM, SQLite FTS5 | macOS local (launchd) |
| **Gravity Claw** | Agente pessoal TypeScript, deploy em cloud | TypeScript, grammy, @anthropic-ai/sdk | Railway + Docker |

**Conclusão imediata:** Intelecto e Antidote são o **mesmo projeto** em fases diferentes — compartilham o mesmo framework-comparison, o mesmo como-construir, o mesmo stack Python, e o mesmo modelo mental. Gravity Claw é uma implementação diferente do mesmo conceito, mas com escolhas técnicas opostas.

---

## Estrutura de Páginas HTML

### Intelecto (5 páginas)

```
index.html                          ← landing com 3 cards (Compare, Construir, Guia)
doc/framework-comparison-pt.html   ← grocery store PT (9 frameworks, 130+ features)
doc/framework-comparison.html      ← grocery store EN (versão inglês)
doc/como-construir.html            ← guia passo-a-passo (6 steps, FAQ)
doc/guia-funcionalidades.html      ← referência completa (8 corredores + 6 arquiteturas)
```

### Antidote (3 páginas)

```
index.html                          ← landing com 2 cards (Compare, Construir)
doc/framework-comparison-pt.html   ← mesmo grocery store PT
doc/como-construir.html            ← mesmo guia de build
```

### Gravity Claw (5 páginas)

```
index.html                          ← hub de recursos (arquitetura, dev workflow, tabela)
doc/gravity-claw.html              ← feature store original (roxo)
doc/gravity-claw-store.html        ← clone do Vercel (teal, 11 cats)
doc/gravity-claw-antigravity.html  ← store para AntiGravity (CLAWS)
doc/gravity-claw-claudecode.html   ← store para Claude Code (CLAUDE.md)
```

**O que o Intelecto tem que nenhum dos outros tem:** `guia-funcionalidades.html` — uma referência completa com 8 corredores funcionais e 6 padrões de arquitetura documentados com diagramas ASCII, file trees e cards de comparação. É o documento mais rico dos três projetos.

---

## Comparação das Páginas de Feature Selection

### Framework Comparison (Intelecto/Antidote) vs Feature Stores (Gravity Claw)

| Dimensão | Intelecto/Antidote | Gravity Claw stores |
|---|---|---|
| **Escopo** | 9 frameworks externos (OpenClaw, ZeroClaw, NanoClaw…) | 1 projeto específico (Gravity Claw) |
| **Categorias** | 8 corredores (Identidade, Segurança, Comunicação, Memória, Automação, Integrações, Skills, Arquitetura) | 11 categorias (Core, Messaging, Memory, Voice, LLM, Tools, Proactive, Security, Architecture, Deploy, UX) |
| **Features** | 130+ com referências de implementação por repo | 70–94 com subdescrições e file paths internos |
| **Referência de código** | ✅ "encontra em `./nanoclaw/src/memory/`" | ⚠️ só file path interno (`src/agent.ts`) |
| **Subdescrições** | ❌ descrição longa por parágrafo | ✅ "listens to voice notes", "blue bubbles via Mac" |
| **Persistência do carrinho** | ✅ localStorage persiste entre sessões | ❌ perde ao fechar |
| **Versão em inglês** | ✅ arquivo separado | ✅ tudo em inglês já |
| **Busca** | ✅ filterItems() em tempo real | ✅ searchInput em tempo real |
| **Filtro por categoria** | ✅ filterCategory() | ✅ cat-btn com data-cat |

---

## Comparação do Output Gerado

### O que cada um baixa/copia

| | Intelecto / Antidote | GC Original | GC Store | GC AntiGravity | GC Claude Code |
|---|---|---|---|---|---|
| **Ação** | Download de arquivo | Clipboard | Clipboard | Clipboard | Clipboard |
| **Formato** | `.md` (briefing spec) | Prompt MD | Prompt MD | Prompt CLAWS | `CLAUDE.md` |
| **Referências de repo** | ✅ sim | ❌ | ❌ | ❌ | ❌ |
| **File paths do projeto** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Portável para qualquer LLM** | ✅ | ⚠️ parcial | ✅ | ❌ AntiGravity only | ❌ Claude Code only |
| **Stack hardcoded no output** | ❌ genérico | ✅ TypeScript | ✅ TypeScript | ✅ TypeScript + OpenRouter | ✅ TypeScript + Railway |

### Exemplo real do que cada um gera para "SQLite Memory"

**Intelecto:**
```markdown
### SQLite FTS5 com BM25

**O que faz**: Busca textual com ranking de relevância, zero custo de API
**Referência**: NanoClaw (./nanoclaw/src/memory/), OpenClaw (./openclaw/src/memory/)
**Como implementar**: Configure FTS5 virtual table com content='messages'
```

**Gravity Claw Claude Code:**
```markdown
- **SQLite Conversation Buffer** — histórico local com better-sqlite3 → `src/memory/db.ts`
- **FTS5 Full-Text Search** — busca eficiente no histórico → `src/memory/db.ts`
```

**Gravity Claw AntiGravity:**
```markdown
- **Conversational Buffer Cache** — histórico SQLite da sessão
- **FTS5 Full-Text Search** — busca textual sem custo de embedding
```

**Diferença:** Intelecto aponta onde no ecossistema achar o código. Gravity Claw aponta onde no seu projeto criar o arquivo.

---

## O que o Intelecto tem de único

### 1. `guia-funcionalidades.html` — Sem equivalente nos outros

Uma referência de 65KB com:
- **8 corredores funcionais** com diagramas, file trees e descrições detalhadas
- **6 padrões de arquitetura** (OpenClaw/Hub Gateway, ZeroClaw/Trait Contracts, NanoClaw/Container Isolation, TinyClaw/Agent Teams, IronClaw/Zero-Trust, Agent Zero/Learning Loop)
- Para cada padrão: badge colorido, diagrama ASCII, 3 cards (força, complexidade, ideal para)
- Navegação por table of contents com smooth scroll para 14 seções

Isto é o que um dev precisa antes de escolher qual arquitetura adotar. **Não existe equivalente no Gravity Claw.**

### 2. Versão inglês do framework comparison

`framework-comparison.html` é a mesma ferramenta em inglês — permite usar com LLMs que trabalham melhor em inglês sem perder a qualidade dos dados.

### 3. `antidote.md` — Documento de spec completo (42KB)

Dentro de `doc/antidote.md` está o briefing completo de implementação com:
- Decisões técnicas documentadas com justificativa
- 6 workstreams paralelos para build
- Schema SQL completo da memória
- O que NÃO construir e por quê
- Roadmap de Phase 2 e Phase 3

### 4. Stack Python-first, macOS-only

Escolhas opostas ao Gravity Claw:

| | Intelecto | Gravity Claw |
|---|---|---|
| **Linguagem** | Python 3.11 + LiteLLM | TypeScript + @anthropic-ai/sdk |
| **Deploy** | macOS local (launchd) | Railway + Docker |
| **Vector DB** | ❌ SQLite FTS5 apenas | ✅ Supabase + pgvector / Pinecone |
| **Secrets** | Fernet (hardware UUID) | `.env` (Railway variables) |
| **Auto-restart** | launchd plist | Railway health check |
| **Providers** | OpenRouter + Ollama | Anthropic direto + OpenRouter |
| **Custo** | Ollama free local | Claude Max $200/mês |

---

## O que Gravity Claw faz que o Intelecto não faz

### 1. Outputs especializados por ferramenta

Intelecto gera um único formato genérico. Gravity Claw gera:
- CLAUDE.md formatado para Claude Code (com file paths, troubleshooting, Railway commands)
- Prompt CLAWS para AntiGravity (com Opus 4.5 planning mode, CLAWS steps)

### 2. Hub central de recursos

O `index.html` do Gravity Claw tem arquitetura documentada, tabela OpenClaw vs Gravity Claw, dev workflow com terminal block, build levels visuais. O do Intelecto tem 3 cards simples.

### 3. UX mais polida

Gravity Claw usa Outfit + JetBrains Mono, glassmorphism, animações, sticky header, tags coloridas, subdescrições por feature. Intelecto usa Inter, design mais limpo e minimalista, sem animações elaboradas.

### 4. Multi-cloud / Railway deploy

Railway + Docker permite deploy em cloud e acesso 24/7 sem manter o Mac ligado. Intelecto é macOS-only por design.

---

## Matriz de Portabilidade Completa (todos os projetos)

| Feature Store | Claude Code | AntiGravity | Codex/OpenAI | Gemini | Cursor | Download |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **Intelecto framework-comparison** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ .md |
| **Antidote framework-comparison** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ .md |
| **GC gravity-claw.html** | ⚠️ | ✅ | ⚠️ | ⚠️ | ⚠️ | ❌ |
| **GC gravity-claw-store.html** | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **GC gravity-claw-antigravity.html** | ⚠️ | ✅ nativo | ❌ | ❌ | ❌ | ❌ |
| **GC gravity-claw-claudecode.html** | ✅ nativo | ⚠️ | ❌ | ❌ | ⚠️ | ❌ |

---

## Os Três Projetos São Sequenciais

A relação entre os três não é de concorrência — é de **pipeline**:

```
┌─────────────────────────────────────────────────────┐
│  INTELECTO / ANTIDOTE                               │
│  "O que existe no ecossistema? Qual arquitetura?"   │
│  → framework-comparison (9 frameworks, 130+ items)  │
│  → guia-funcionalidades (8 corredores, 6 patterns)  │
│  → Output: .md genérico com referências de código   │
└───────────────────────┬─────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────┐
│  GRAVITY CLAW STORES                                │
│  "O que implementar no meu projeto específico?"     │
│  → feature store (70–94 items com file paths)       │
│  → Output: CLAUDE.md ou prompt CLAWS                │
└───────────────────────┬─────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────┐
│  CLAUDE CODE / ANTIGRAVITY                          │
│  "Scaffolda o projeto seguindo os dois docs"        │
│  → Lê o .md do Intelecto (referências)              │
│  → Lê o CLAUDE.md do Gravity Claw (file paths)      │
│  → Implementa                                       │
└─────────────────────────────────────────────────────┘
```

O fluxo ideal de uso:
1. **Intelecto** — descoberta e decisão de arquitetura
2. **Gravity Claw Claude Code store** — configuração específica do projeto
3. **Claude Code** — implementação com ambos os documentos como contexto

---

## Gaps que os três têm em comum

1. **Nenhum tem testes automatizados** das features selecionáveis
2. **Nenhum persiste seleções entre sessões** exceto o Intelecto (localStorage)
3. **Nenhum permite combinar outputs** (ex: gerar spec do Intelecto + CLAUDE.md do GC num único fluxo)
4. **Nenhum tem versão mobile otimizada** (todos assumem desktop)
5. **Nenhum tem modo dark/light** — todos são dark-only

---

## Resumo Final

| | Melhor em |
|---|---|
| **Intelecto** | Documentação de ecossistema, guia de arquiteturas, referências de código, portabilidade de output |
| **Antidote** | Subconjunto mais focado do Intelecto, mesma portabilidade |
| **Gravity Claw** | Outputs especializados por ferramenta (CLAUDE.md, prompt CLAWS), UX polida, hub central, deploy cloud |
