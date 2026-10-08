---
title: "04 — Configuraciones clave en Claude y Claude Code"
type: seccion-curso
---

# 04 — Configuraciones clave en Claude y Claude Code

## En 60 segundos

Claude es el modelo/chat; Claude Code es el agente de programación que corre en tu terminal. La palanca central del ecosistema Claude es el **manejo explícito del contexto**: el *extended thinking* gasta tokens de salida aunque no te muestre el razonamiento, los **subagents** existen para no contaminar el contexto principal, y el **prompt caching** abarata reenviar historial largo. Si entendés esas tres cosas, entendés Claude. Y como en el tema anterior: los modelos que pedía el brief (Claude 3.5 Sonnet/Haiku, Claude 4 Opus/Sonnet) **ya no son los vigentes** a octubre de 2026.

---

## Nota de actualización

El brief original pedía context windows y precios de **Claude 3.5 Sonnet, 3.5 Haiku, 4 Opus, 4 Sonnet, Haiku 3.5**. A la fecha de corte (2026-10-07), la página oficial de modelos muestra como vigentes a **Claude Fable 5.1, Opus 5.5, Sonnet 5.5 y Haiku 5.5** (todos con 1M de contexto). Los 3.5 no figuran como vigentes.

- Igual que en el tema 03: usamos el catálogo vigente y marcamos lo viejo.
- Un dato clave para el hilo de tokens: **los modelos Claude 4.7 y posteriores usan un tokenizer nuevo**; el mismo texto produce **≈30% más tokens** que en modelos anteriores. Mismo texto, más costo.

---

## Modelos y precios de Claude (octubre 2026, fuente oficial)

Precios por 1M de tokens (MTok, standard). Fuente: https://platform.claude.com/docs/en/about-claude/pricing · info: 2026-10-07 · **reconfirmado 2026-10-08 (sin cambios)**.

| Modelo | Contexto | Entrada /1M | Salida /1M |
|--------|----------|-------------|------------|
| Claude Fable 5.1 | 1M | $10 | $50 |
| Claude Opus 5.5 | 1M | $4 | $20 |
| Claude Sonnet 5.5 | 1M | $2 | $10 |
| Claude Haiku 5.5 | 1M | $0.10 (≤100K) / $0.50 (>100K) | $0.50 |
| Claude Opus 5 / Opus 4.5–4.8 | n/d | $5 | $25 |
| Claude Opus 4 / Opus 4.1 | n/d | $15 | $75 |
| Claude Sonnet 4.6 / 4.5 / 4 | n/d | $3 | $15 |
| Claude Haiku 4.5 | n/d | $1 | $5 |
| Claude Haiku 3.5 | n/d | $0.80 | $4 |
| ~~Claude Sonnet 3.5~~ | no listado | no listado | retirado |

**Salida máxima:** 128K tokens en los modelos 5.5 (Fable/Opus/Sonnet/Haiku). Fuente: https://platform.claude.com/docs/en/models/overview.

**Notas de pricing:**

- **Prompt caching:** escribir caché 5m = 1.25x, 1h = 2x; **leer caché (hit) = 0.1x del input estándar** (0.025x en Fable 5.1/Mythos 5.1; 0.05x en Opus 5.5/Sonnet 5.5). Un *cache hit* cuesta **el 10% del precio de input estándar**.
- **Batch API:** descuento adicional, combinable con caching.
- **Data residency** (`inference_geo: "us"`, modelos 4.6+): multiplicador 1.1x.
- La página de pricing no lleva fecha de publicación explícita; se toma como vigente a la fecha de acceso.

---

## Extended thinking («think mode»)

> «When thinking is active, Claude works through the problem in its own words before answering… Thinking has a cost: the tokens Claude spends reasoning are billed as output tokens… and they count toward `max_tokens` alongside the response text.»

Esto es clave y suele sorprender: **el razonamiento se factura como tokens de salida aunque no lo veas**. En modelos 4.6+ el thinking es **adaptativo** (Claude decide cuándo y cuánto pensar) y se controla con `thinking: {type: "adaptive" | "disabled" | "between_tools"}` y con el parámetro `effort`.

- **Display:** puede ser `"summarized"` (devuelve un resumen) u `"omitted"` (default en varios modelos). *«You don't always see this text, and what you see is never the raw chain of thought.»*
- **Con herramientas:** el thinking también aparece **entre** llamadas a tools; cada bloque lleva un `signature` (copia cifrada del razonamiento que se reenvía sin cambios en conversaciones multi-turno y con tools).

> Fuente: https://platform.claude.com/docs/en/build-with-claude/thinking · acceso: 2026-10-07.

---

## Subagents (Claude Code)

> «Subagents are specialized AI assistants that handle specific types of tasks… Each subagent runs in its own context window with a custom system prompt, specific tool access, and independent permissions. It also sends its own requests, which count toward the same usage limits as your main conversation.»

Los subagents se definen como archivos Markdown con frontmatter YAML en `~/.claude/agents/` o `.claude/agents/`. Frontmatter soportado: `name`, `description`, `tools`, `model`, `hooks`, `omitClaudeMd`.

- **Built-in:** Explore, Plan, general-purpose (y helpers: `claude`, `statusline-setup`, `claude-code-guide`). Explore y Plan son **read-only** (Write y Edit denegados).
- **Advertencia de contexto:** cuando la suma de las descripciones de tus subagents supera **15.000 tokens**, Claude Code muestra un warning al arrancar. Las descripciones largas **consumen contexto**.

**Por qué importan para tokens:** el subagent hace el trabajo en su propio contexto y devuelve solo el resumen. Sirven para **preservar contexto** del hilo principal — el patrón correcto cuando una tarea generaría cientos de tokens de resultados que no necesitás ver.

> Fuente: https://code.claude.com/docs/en/sub-agents · acceso: 2026-10-07.

---

## Claude Code: qué es y cómo se instala

> «an AI-powered coding assistant that helps you build features, fix bugs, and automate development tasks. It understands your entire codebase and can work across multiple files and tools.»

Corre en terminal, extensiones de IDE, app de escritorio y web.

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash

# Windows (PowerShell)
irm https://claude.ai/install.ps1 | iex

# vía Homebrew / winget
brew install --cask claude-code
winget install Anthropic.ClaudeCode
```

> Fuente: https://code.claude.com/docs/en/overview · acceso: 2026-10-07.

### Archivos de configuración (settings)

Claude Code lee cuatro archivos JSON, en este orden de precedencia:

| Archivo | Alcance |
|---------|---------|
| `~/.claude/settings.json` | Usuario |
| `.claude/settings.json` | Proyecto (compartido) |
| `.claude/settings.local.json` | Proyecto (local, no versionado) |
| `managed-settings.json` | Gestionado |

Más `~/.claude.json` (estado propio: sesión de login, MCP servers, estado por proyecto). Se puede reubicar con `CLAUDE_CONFIG_DIR`.

> Fuente: https://code.claude.com/docs/en/settings · acceso: 2026-10-07.

### Comandos esenciales

`claude`, `claude "query"`, `claude -p "query"`, `claude -c`, `claude -r "<session>"`, `claude update`, `claude auth login|logout|status`, `claude mcp`, `claude plugin`, `claude doctor`, `claude agents`, `claude setup-token`.

> Fuente: https://code.claude.com/docs/en/cli-reference · acceso: 2026-10-07.

### Hooks

> «Hooks are user-defined shell commands, HTTP endpoints, MCP tool calls, LLM prompts, or subagents that execute automatically at specific points in Claude Code's lifecycle.»

Eventos: `SessionStart`, `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `SubagentStart/Stop`, `PreCompact`, `Stop`, `SessionEnd`, etc. Los hooks `PreToolUse` pueden **bloquear** una llamada (`permissionDecision: "deny"`).

> Fuente: https://code.claude.com/docs/en/hooks · acceso: 2026-10-07.

### Permission modes

| Modo | Comportamiento |
|------|----------------|
| `default` | Manual: pregunta en el primer uso de cada tool. |
| `acceptEdits` | Acepta ediciones de archivo y comandos de FS comunes. |
| `plan` | Solo lectura/exploración. |
| `auto` | Corre sin prompts rutinarios; un clasificador revisa acciones. |

Se cambia con `defaultMode` en settings o `/permissions`. Las reglas allow/ask/deny se evalúan en orden: **deny → ask → allow**.

> Fuente: https://code.claude.com/docs/en/permissions · acceso: 2026-10-07.

### MCP (Model Context Protocol)

> «The Model Context Protocol (MCP) is an open standard for connecting AI tools to external data sources. With MCP, Claude Code can read your design docs in Google Drive, update tickets in Jira, pull data from Slack, or use your own custom tooling.»

> Fuente: https://code.claude.com/docs/en/overview · acceso: 2026-10-07.

---

## Cómo se maneja el contexto en Claude

El **context window incluye todo**: historial de conversación + resultados de tool calls + system prompt + thinking. Cuando se llena, Claude Code **compacta** (eventos `PreCompact`/`PostCompact`).

- **El umbral numérico SÍ está publicado** (verificado 2026-10-08): en modelos con ventana nativa de **1M**, Claude Code autocompacta **antes de llenar la ventana, a ~967K tokens por defecto**; en modelos de **200K**, compacta en el borde de 200K. El umbral es configurable (`/autocompact <valor>`, rango 100K–1M; setting `autoCompactWindow`; variable `CLAUDE_CODE_AUTO_COMPACT_WINDOW`). Aparte, la compaction **del lado API** (Messages API) exige un trigger mínimo de **50.000 tokens**.
- Como referencia de tamaño oficial: la doc usa `max_tokens=16000` para razonamiento + respuesta en un ejemplo, y advierte a los **15.000 tokens** de descripciones de subagents.

> Fuentes: https://code.claude.com/docs/en/model-config · /context-window · https://platform.claude.com/docs/en/build-with-claude/compaction-threshold · acceso: 2026-10-08.

**Diferencia verificable vs ChatGPT:** Anthropic documenta **prompt caching** explícito (cache hit = 10% del input); OpenAI documenta «cached input» en su tabla (p. ej. gpt-6-astra cached input $1.00 vs $10.00). Ambos abaratan el contexto repetido, con estructuras de precio distintas. Esto es factual, no un juicio de «cuál es mejor».

> Fuentes: https://platform.claude.com/docs/en/about-claude/pricing y https://developers.openai.com/api/docs/pricing · acceso: 2026-10-07.

---

## Checklist de configuración

- [ ] ¿Sé si el thinking está adaptativo, apagado o entre tools?
- [ ] ¿Tengo `effort` acorde a la tarea (no todo al máximo)?
- [ ] ¿Uso subagents para tareas que generarían mucho contexto en el hilo principal?
- [ ] ¿Revisé mis descripciones de subagents (no pasarme de ~15k tokens)?
- [ ] ¿Tengo un permission mode consciente (no `auto` por default sin pensarlo)?
- [ ] ¿Aprovecho prompt caching para historial largo?

---

## Optimización de tokens en Claude / Claude Code

- **El thinking cuesta aunque no lo veas.** Se factura como salida y cuenta contra `max_tokens`. No lo dejes al máximo «por las dudas».
- **Un tokenizer nuevo significa ~30% más tokens** en modelos 4.7+. Lo mismo que escribías antes ahora cuesta más.
- **Subagents = contexto aislado.** Delegá lo ruidoso; que vuelva solo el resumen.
- **Descripciones de subagents consumen contexto** desde el arranque (warning a los 15k tokens).
- **Prompt caching: 10% del input en un hit.** Reenviar un historial largo sin caching es carísimo al lado de cachearlo.
- **Haiku 5.5 cambia de precio por umbral** ($0.10 ≤100K, $0.50 >100K): vigilar el tamaño del prompt.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07:
- https://www.anthropic.com/pricing · https://platform.claude.com/docs/en/about-claude/pricing
- https://platform.claude.com/docs/en/models/overview
- https://platform.claude.com/docs/en/build-with-claude/thinking
- https://code.claude.com/docs/en/overview · /settings · /cli-reference · /hooks · /permissions · /sub-agents
- https://developers.openai.com/api/docs/pricing

## Pendientes (no inventar si falta)

- [x] ~~Context windows históricos de Claude 3.5 / Claude 4~~ → **resuelto** (200K; Sonnet 4 con 1M en beta retirada el 30-abr-2026).
- [x] ~~Umbral numérico de compactación de Claude Code~~ → **resuelto:** ~967K por defecto en 1M; borde de 200K en 200K; configurable.
- [ ] Confirmar si «subagents» existe como feature del chat de Claude (solo verificado en Claude Code).
- [ ] Reconfirmar pricing de Anthropic antes de publicar (dato volátil).

---

*Sección 04 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*