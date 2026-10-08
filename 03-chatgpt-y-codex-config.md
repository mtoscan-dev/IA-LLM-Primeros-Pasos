---
title: "03 — Configuraciones clave en ChatGPT y Codex"
type: seccion-curso
---

# 03 — Configuraciones clave en ChatGPT y Codex

## En 60 segundos

ChatGPT, ChatGPT Work, Codex CLI y la API de OpenAI son cuatro puertas al mismo backend de modelos, pero con distinto control sobre el contexto y los parámetros. ChatGPT es la interfaz (el sistema elige el modelo); Codex CLI es el agente de terminal (elegís modelo y esfuerzo de razonamiento); la API es control total (elegís modelo, `temperature`, `max_tokens` y ves el `usage` de tokens de cada respuesta). Confundir «Codex de OpenAI» con «Codex de GitHub Copilot» sigue siendo el error más común. Y ojo: los modelos que pedía el brief original (GPT-4o, GPT-4o mini, GPT-4.1) **ya no figuran como vigentes** en las fuentes oficiales a octubre de 2026. Esta sección usa el catálogo vigente.

---

## Nota de actualización (leé esto primero)

El brief original del curso pedía precios y context windows de **GPT-4o, GPT-4o mini y GPT-4.1**. A la fecha de corte de la investigación (2026-10-07) esas familias **no aparecen en la página oficial de pricing ni en el catálogo de modelos de OpenAI**. Fueron reemplazadas por generaciones posteriores.

- **Regla editorial del curso:** usamos el catálogo vigente y marcamos lo deprecado explícitamente. No enseñamos con modelos que ya no están disponibles.
- Si estabas buscando específicamente GPT-4o o GPT-4.1, la lección es esa: en este rubro, «el modelo que conocías» puede dejar de existir en meses. Ese es el hilo conductor del curso.

---

## ChatGPT hoy: tres modos

La documentación oficial de ChatGPT organiza el producto en tres modos:

| Modo | Para qué sirve |
|------|----------------|
| **Chat** | Pregunta y respuesta, ida y vuelta. El uso conversacional clásico. |
| **ChatGPT Work** | Lleva una tarea grande hasta un **resultado revisable**. Genera archivos terminados: documentos, presentaciones, planillas y PDF. |
| **Codex** | Vista de desarrollador, con herramientas técnicas. |

> Fuente: https://learn.chatgpt.com/docs/use-chatgpt · acceso: 2026-10-07.

**Lo que cambió respecto de la versión vieja del curso:** «Canvas» no tiene una página de documentación vigente a 2026-10-07; la generación de archivos aparece bajo **ChatGPT Work**. Del mismo modo, los «GPTs» como página vigente no aparecen: la personalización de workflows se documenta hoy como **Skills & Plugins**. Si venías de tutoriales viejos que hablaban de Canvas y GPTs, ese vocabulario quedó atrás.

### Projects

> «Projects help you organize ChatGPT around a topic, goal, or ongoing body of work. Keep related chats, files, and instructions in one project when the work will continue over time or depend on the same context.»

Un Project agrupa chats, archivos e instrucciones bajo un mismo contexto. **Implicación en tokens:** un Project bien armado evita que re-explainás el contexto en cada chat nuevo — pero también significa que las instrucciones del proyecto se cargan en cada consulta. Contexto que no controlás es contexto que pagás.

### Custom Instructions y Memory

- **Custom Instructions:** preferencias que ChatGPT sigue **a través de todos los chats** (por ejemplo, tu estilo de respuesta preferido). En Codex, esas instrucciones personales se guardan en tu `AGENTS.md` global.
- **Memory:** ChatGPT arrastra contexto útil de chats anteriores (preferencias estables, flujos recurrentes, convenciones de proyecto).

Ambas son palancas de «contexto persistente»: te ahorran repetir, pero no son gratis en tokens ni en privacidad. Revisá qué tienen guardado.

> Fuente: https://learn.chatgpt.com/docs/personalize · acceso: 2026-10-07.

---

## Codex CLI: el Codex de OpenAI (no el de Copilot)

**Aclaración que sigue vigente:** «Codex» puede referirse a dos cosas distintas y hay que separarlas.

- **Codex de OpenAI (CLI):** el agente de programación que corre en tu terminal. Repositorio `openai/codex`, descrito como *«Lightweight coding agent that runs in your terminal»*. **Es el que usamos en este curso.**
- **Codex de GitHub Copilot:** un *modelo* que aparece dentro de GitHub Copilot. Es otro producto.

### Instalación

```bash
# macOS / Linux
curl -fsSL https://chatgpt.com/codex/install.sh | sh

# vía npm
npm install -g @openai/codex

# vía Homebrew
brew install --cask codex
```

> Fuente: https://github.com/openai/codex · acceso: 2026-10-07.

### Modelo por defecto y cómo cambiarlo

La cabecera de arranque del CLI muestra el modelo activo: `model: gpt-6.1-sol medium`. Es decir, el modelo por defecto es **`gpt-6.1-sol` con esfuerzo de razonamiento `medium`**.

- Dentro de una sesión interactiva: `/model` para cambiar de modelo o ajustar el esfuerzo de razonamiento.
- Al lanzar: `codex --model gpt-6.1-sol` (o el alias `-m`).

### Archivo de configuración

La app de escritorio de ChatGPT, Codex CLI y la extensión de IDE **comparten el mismo `config.toml`**. Para fijar un modelo:

```toml
model = "gpt-6.1-sol"
```

> Fuente: https://learn.chatgpt.com/docs/models · acceso: 2026-10-07.

### Comandos esenciales

| Comando | Qué hace |
|---------|----------|
| `/init` | Crea el `AGENTS.md` del proyecto con las instrucciones del repo. |
| `/status` | Estado de la sesión. |
| `/permissions` | Permisos de la sesión. |
| `/model` | Cambiar modelo / esfuerzo de razonamiento. |
| `/review` | Revisión de código. |
| `codex resume` | Retomar una sesión anterior. |
| `codex exec` | Ejecución no interactiva (para scripting). |
| `codex mcp` | Gestión de servidores MCP. |
| `codex --search` | Habilita búsqueda. |

> Fuente: https://learn.chatgpt.com/docs/codex/cli · acceso: 2026-10-07.

### Sobre `temperature` en ChatGPT/Codex

No encontré en la documentación de ChatGPT/Codex un control de `temperature`. El control equivalente y documentado es el **esfuerzo de razonamiento** (*reasoning effort*, de Light a Ultra):

> «Higher reasoning effort can improve results for complex tasks, but it takes longer and uses more tokens.»

Traducción práctica: en ChatGPT/Codex no bajás la «temperatura», subís o bajás el **esfuerzo**. Y subirlo cuesta tokens. Queda como pendiente confirmar si `config.toml` expone `temperature` a nivel de config.

---

## GitHub Copilot: qué es y en qué se diferencia

Copilot es un producto separado, integrado en editores (VS Code, JetBrains, Copilot CLI), no en la terminal como Codex CLI. **Catálogo de modelos soportados a oct/2026** (extracto): GPT-6 Astra/Sol/Luna, GPT-6.1 Sol, GPT-5.6 Sol/Terra/Luna, Claude Opus 5.5, Claude Sonnet 5.5, Claude Fable 5, Gemini 3.7/3.8 Flash, Grok 4.7, Kimi K3, entre otros.

- **Selección de modelo:** podés elegir el modelo, o dejar que la **auto-selección** rutee la consulta «según la complejidad de la tarea y la disponibilidad en tiempo real». También admite *bring your own key* (BYOK) y modelos base/LTS.
- **Contexto extendido:** la ventana de **1 millón de tokens** está disponible **solo en VS Code y Copilot CLI**, y elegir un contexto más grande o mayor razonamiento **impacta el consumo de créditos de IA**.

> Fuentes: https://docs.github.com/en/copilot/reference/ai-models/supported-models y https://docs.github.com/en/copilot/concepts/models/overview · acceso: 2026-10-07.

**Pendiente honesto:** la doc describe la ventana de contexto pero **no** el detalle exacto del payload que se envía por request (cuántos tokens de repo, archivos abiertos, etc.). Eso sigue sin verificar.

---

## Parámetros clave de la API de OpenAI

Si usás la API, tenés control real. La API de Chat Completions expone, entre otros: `temperature`, `top_p`, `frequency_penalty`, `presence_penalty`, `max_tokens`, `stop`, `logit_bias`, `logprobs`, `seed`, `response_format`, `tools`, `tool_choice`.

- **`temperature`** — float 0.0–2.0, default 1.0. *«Lower values lead to more predictable and typical responses… At 0, the model always gives the same response for a given input.»*
- **`top_p`** — float 0.0–1.0, default 1.0. Considera solo los tokens cuya probabilidad acumulada llega a P.
- **`frequency_penalty`** — float −2.0 a 2.0, default 0.0. Penaliza tokens según **cuántas veces** aparecen.
- **`presence_penalty`** — float −2.0 a 2.0, default 0.0. Penaliza un token si **ya apareció**, sin escalar por cantidad.
- **`max_tokens`** — entero ≥1. *«The maximum value is the context length minus the prompt length.»*
- **`stop`** — array de strings. Detiene la generación al encontrar cualquiera de ellos.

> Fuentes: https://platform.openai.com/docs/api-reference/chat/create y https://openrouter.ai/docs/api-reference/parameters (espejo documentado) · acceso: 2026-10-07.

### `reasoning_effort` y la Responses API

Los modelos de razonamiento agregan el parámetro **`reasoning_effort`** (enum: `xhigh / high / medium / low / minimal / none`). Además, la recomendación oficial hoy es:

> «If you're building any text generation app, we recommend using the Responses API over the older Chat Completions API.»

> Fuente: https://developers.openai.com/api/docs/guides/text-generation · acceso: 2026-10-07.

---

## Modelos y precios de OpenAI (octubre 2026, fuente oficial)

Precios por 1M de tokens (Standard). Fuente: https://developers.openai.com/api/docs/pricing · info: 2026-10-07 · **reconfirmado 2026-10-08 (sin cambios)**.

| Modelo | Contexto | Entrada /1M | Salida /1M |
|--------|----------|-------------|------------|
| gpt-6-astra | 1.05M | $10.00 | $50.00 |
| gpt-6.1-sol | 1.05M | $2.00 | $10.00 |
| gpt-6-luna | 1.05M | $0.10 | $0.50 |
| gpt-5.6-sol (Daybreak/Cyber) | n/d | $4.00 | $20.00 |
| gpt-5.3-codex (Codex) | n/d | $1.75 | $14.00 |
| ~~GPT-4o / GPT-4o mini / GPT-4.1~~ | — | **no listado** | **no listado** |

**Notas de pricing (mismas fuentes):**

- **Contexto largo:** gpt-6-astra pasa a $20/$75, gpt-6.1-sol a $4/$15, gpt-6-luna a $0.20/$0.75.
- **Batch:** −50% en input y output.
- **Fast mode / Ultrafast:** recargos (p. ej. Astra Fast $20/$100; Ultrafast $60/$300).
- **Búsqueda web (tool):** $10.00 / 1.000 llamadas.
- Las modelos GPT-4o, GPT-4o mini y GPT-4.1 aparecen como **reemplazadas/deprecadas**: no tienen fila en la tabla de precios vigente.
- **No se pudo confirmar un cambio de precios dentro de los últimos 30 días.** Las únicas fechas explícitas de la página son viejas (p. ej. «GPT-5.6 Sol's promotional pricing is available at least through November 21, 2026»; «Priority processing was renamed Fast mode on July 30, 2026»). Es decir: la fuente no permite probar un cambio <30 días.

---

## Diferencias prácticas: ChatGPT vs Codex vs API

| Aspecto | ChatGPT (UI) | Codex CLI | API de OpenAI |
|---------|--------------|-----------|----------------|
| Elección de modelo | El sistema decide | Configurable (`/model`, `--model`) | Total |
| «Temperatura» | No; se usa esfuerzo de razonamiento | Esfuerzo de razonamiento | `temperature` / `top_p` |
| `max_tokens` | No | Según config | Sí |
| Tokens visibles | No | Según sesión | Sí (`usage` en la respuesta) |
| Uso de herramientas | Según modo | Sí (MCP, etc.) | Vos implementás las tuyas |
| Costo | Suscripción / según uso | Según uso de API | Pago por token (input + output) |

---

## Checklist de configuración

- [ ] ¿Sé qué modelo estoy usando en cada herramienta?
- [ ] ¿Entiendo qué contexto se envía (instrucciones, Memory, archivos)?
- [ ] En la API: ¿seté `max_tokens` para evitar respuestas infinitas?
- [ ] ¿Ajusto `temperature` según la tarea (consistencia vs creatividad)?
- [ ] ¿Sé cuándo conviene empezar una conversación nueva?
- [ ] ¿Distingo Codex (OpenAI CLI) de Codex (modelo de Copilot)?

---

## Optimización de tokens en ChatGPT/Codex

- **Elegí el modelo por costo real.** gpt-6-luna a $0.10/$0.50 hace tareas simples; no pagues gpt-6-astra a $10/$50 para clasificar texto.
- **Subir el esfuerzo de razonamiento cuesta tokens.** El propio doc lo dice: mejorar resultados complejos tarda más y consume más.
- **Revisá Projects, Custom Instructions y Memory.** Todo lo persistente se carga en cada consulta.
- **En Codex, `AGENTS.md` bien acotado.** Instrucciones largas = contexto consumido en cada turno.
- **En la API, mirá `usage`.** Es la única forma de ver el costo real de lo que acabás de hacer.
- **Contexto largo = precio con recargo.** Pasado el umbral, gpt-6-astra salta de $10 a $20 de entrada.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07:
- https://learn.chatgpt.com/docs/use-chatgpt · https://learn.chatgpt.com/docs/personalize
- https://learn.chatgpt.com/docs/models · https://learn.chatgpt.com/docs/codex/cli
- https://github.com/openai/codex
- https://platform.openai.com/docs/api-reference/chat/create
- https://developers.openai.com/api/docs/pricing · https://developers.openai.com/api/docs/guides/text-generation
- https://openrouter.ai/docs/api-reference/parameters
- https://docs.github.com/en/copilot/reference/ai-models/supported-models · https://docs.github.com/en/copilot/concepts/models/overview

## Pendientes (no inventar si falta)

- [ ] Confirmar si `config.toml` de Codex expone `temperature`.
- [ ] Localizar la página vigente de Canvas / GPTs o confirmar su renombre a Work / Skills & Plugins.
- [ ] Detalle del payload de contexto de GitHub Copilot.
- [ ] Reconfirmar pricing antes de publicar (dato volátil).

---

*Sección 03 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*