# Análise de Portabilidade — Feature Stores

> Comparação dos 4 HTMLs do ponto de vista do output gerado: são tool-specific ou podem ser usados com qualquer agente de IA?

---

## O que cada um gera (output real)

| Arquivo | Output | Primeira linha gerada |
|---|---|---|
| `gravity-claw.html` | Prompt markdown | `"Build these features brick-by-brick inside AntiGravity"` |
| `gravity-claw-store.html` | Prompt markdown | Stack + features sem mencionar ferramenta |
| `gravity-claw-antigravity.html` | Prompt AntiGravity | `"usando o ambiente AntiGravity com CLAWS Framework e Opus 4.5 em planning mode"` |
| `gravity-claw-claudecode.html` | Arquivo `CLAUDE.md` | `"guidance to Claude Code when working on this Gravity Claw implementation"` |

---

## São portáteis?

### `gravity-claw.html` — Parcialmente preso ao AntiGravity

O output tem hardcoded: *"using the AntiGravity desktop environment and the CLAWS framework"*. Se colar no Claude Code ou Codex vai funcionar — o texto é legível por qualquer LLM — mas o framing está errado. É um prompt genérico com o nome errado.

### `gravity-claw-store.html` — O mais portátil dos 4

Gera um prompt limpo com stack, segurança e features. Não menciona AntiGravity nem Claude Code nas instruções. Funciona razoavelmente bem colado em qualquer ferramenta: Claude Code, Codex, Gemini, Cursor. O stack (`grammy`, `@anthropic-ai/sdk`, Railway) é do **projeto** — não da ferramenta de IA.

### `gravity-claw-antigravity.html` — Preso ao AntiGravity

Gera um prompt que assume que você está **dentro** do AntiGravity com Opus 4.5 em planning mode. A metodologia CLAWS (C→L→A→W→S) é a sequência de conversas no AntiGravity. Se colar no Claude Code funciona parcialmente — ele entende o markdown — mas o workflow CLAWS não faz sentido no contexto de CLI.

### `gravity-claw-claudecode.html` — Preso ao Claude Code

Gera um `CLAUDE.md` — um formato **exclusivo** do Claude Code CLI. Inclui caminhos de arquivo (`src/agent.ts`, `mcp.json`), comandos Railway, troubleshooting table. Jogar isso no AntiGravity ou no Codex é jogar contexto de projeto como se fosse um prompt de usuário — funciona como contexto, mas desperdiça o formato.

---

## Onde o acoplamento acontece de verdade

O **dado** (categorias, features, descrições) é neutro e reutilizável. O acoplamento só aparece no **gerador de output**:

```
gravity-claw.html        → "inside AntiGravity"          ← string hardcoded
gravity-claw-antigravity → CLAWS steps + Opus 4.5        ← workflow hardcoded
gravity-claw-claudecode  → formato CLAUDE.md + file paths ← formato hardcoded
gravity-claw-store       → prompt genérico                ← mais neutro
```

---

## Diagnóstico

**Nenhum dos 4 serve bem para Codex (OpenAI) ou Gemini** porque todos assumem o stack Anthropic (`@anthropic-ai/sdk`) como parte do projeto — mas isso é uma escolha do *Gravity Claw*, não da ferramenta de IA que vai construí-lo. Um dev usando Codex para escrever o Gravity Claw faria sentido — o Codex seria o *builder*, o `@anthropic-ai/sdk` é o *runtime* do bot.

---

## Matriz de Compatibilidade

| Feature Store | Claude Code | AntiGravity | Codex / OpenAI | Gemini | Cursor |
|---|:---:|:---:|:---:|:---:|:---:|
| `gravity-claw.html` | ⚠️ parcial | ✅ sim | ⚠️ parcial | ⚠️ parcial | ⚠️ parcial |
| `gravity-claw-store.html` | ✅ sim | ✅ sim | ✅ sim | ✅ sim | ✅ sim |
| `gravity-claw-antigravity.html` | ⚠️ parcial | ✅ nativo | ❌ não | ❌ não | ❌ não |
| `gravity-claw-claudecode.html` | ✅ nativo | ⚠️ parcial | ❌ não | ❌ não | ⚠️ parcial |

**Legenda:**
- ✅ nativo — formato ideal para esta ferramenta
- ✅ sim — funciona sem adaptação
- ⚠️ parcial — funciona mas o framing está errado ou o formato não é ideal
- ❌ não — o output assume workflow incompatível

---

## O que seria o ideal

Um único arquivo com **seletor de destino no momento de gerar**:

```
[ Claude Code ]  [ AntiGravity ]  [ Codex / Generic ]  [ Cursor ]
```

| Destino | Output gerado |
|---|---|
| **Claude Code** | `CLAUDE.md` com stack table, file paths, Railway commands, troubleshooting |
| **AntiGravity** | Prompt CLAWS (C→L→A→W→S), Opus 4.5 planning mode |
| **Codex / Generic** | Prompt markdown limpo, sem menção a ferramenta, só stack e features |
| **Cursor** | `.cursorrules` ou prompt de contexto de projeto |

O **dado** (features, categorias, segurança, stack do projeto) ficaria centralizado. Apenas o **template de output** seria trocado conforme o destino selecionado.

---

## Conclusão

A feature store atual funciona bem como **seletor de features** mas está fragmentada como **gerador de output**. Cada arquivo é otimizado para um destino diferente e não há caminho claro para quem quer usar uma ferramenta diferente das duas previstas. O `gravity-claw-store.html` é o mais seguro para uso genérico; os outros dois devem ser usados apenas com a ferramenta para a qual foram projetados.
