---
title: "00 — Qué son IA, LLM, tokens y ventana de contexto"
type: seccion-curso
---

# 00 — Qué son IA, LLM, tokens y ventana de contexto

## En 60 segundos

Un LLM (Large Language Model) predice el siguiente token. No «piensa» ni «sabe»: calcula probabilidades sobre unidades de texto. Un token no es una palabra — es un fragmento definido por el algoritmo de tokenización del modelo. En inglés, ~1 token ≈ ¾ de palabra; en español el mismo texto cuesta más tokens. La **ventana de contexto** es cuánto puede «ver» el modelo a la vez, y todo lo que entra cuenta: system prompt, mensajes, resultados de herramientas, definiciones de tools, imágenes y hasta los tokens de razonamiento. Cada proveedor maneja al desbordar de forma distinta, y a más contexto lleno, menos precisión (context rot). Las alucinaciones existen, se mitigan — no se eliminan. Todo lo demás del curso es operar con esto en mente.

---

## Qué es un LLM, en serio

Cuando hablás con cualquier modelo, no accede a una base de conocimiento en tiempo real (salvo que tenga herramientas conectadas). Usa patrones aprendidos durante el entrenamiento para generar texto que parece plausible.

- **Lo que hace:** dado un contexto, calcula la probabilidad de cada posible siguiente token y selecciona uno según cierta estrategia.
- **Lo que no hace:** no sabe nada «de memoria» como una base de datos; no tiene acceso a información nueva salvo que se la pases o use herramientas; no recuerda entre sesiones salvo que el proveedor lo implemente explícitamente.

---

## Qué es un token

> «Tokens are the units that OpenAI models use to process text. A token can represent a character, part of a word, a whole word, or punctuation. Spaces also affect how text is divided into tokens.»

Y la advertencia central: **el conteo de tokens no es el conteo de palabras**. El mismo texto produce conteos distintos según el modelo, su *encoding* y el idioma.

### De dónde sale la tokenización (subword)

| Algoritmo | Qué hace | Origen verificado |
|-----------|----------|-------------------|
| **BPE** (Byte-Pair Encoding) | Fusiona iterativamente el par de caracteres/símbolos más frecuente. El **número de merges es el único hiperparámetro**. | Sennrich, Haddow & Birch (2015) — https://arxiv.org/abs/1508.07909 |
| **SentencePiece / unigram** | Entrena subword models directamente desde oraciones crudas, sin pre-tokenización: sistema *end-to-end y language-independent*. Licencia Apache 2. | Kudo & Richardson (2018) — https://arxiv.org/abs/1808.06226 |

No necesitás memorizar los algoritmos. Necesitás saber dos cosas: **tu texto se parte en tokens antes de llegar al modelo**, y **ese particionado depende del tokenizer del modelo y del idioma**.

### Cuántos tokens ≈ una palabra (inglés vs español)

- **Regla oficial de OpenAI:** *«one token generally corresponds to ~4 characters of text for common English text. This translates to roughly ¾ of a word (so 100 tokens ~= 75 words).»* — https://platform.openai.com/tokenizer
- **Medición con tokenizers oficiales (2026):** en el encoding **o200k de GPT-5**, el **inglés** promedia **~1.17 tokens/palabra** y el **español ~1.34**; francés 1.40; alemán 1.71. En **Claude Opus 4.8 ~1.88** (por el tokenizer nuevo). Ejemplo crudo: Inglés 94 palabras → 110 tokens; Español 107 palabras → 143 tokens (GPT-5). — https://textkit.tech/blog/tokens-per-word-tokenizer-comparison-2026 *(fuente third-party que declara medir con `tiktoken` y el endpoint `count_tokens` de Anthropic; tabla reproducible)*.
- **Tabla de referencia por tipo de contenido:** inglés común 1 token; inglés técnico 1.5–2; código Python/JS 1.5–3; JSON/YAML 2–4; español/francés/alemán 1.2–1.5; japonés/chino/coreano 2–4; árabe/hindi/tailandés 3–6. *«English prose is roughly 1.3 tokens per word.»* — https://myengineeringpath.dev/genai-engineer/tokenization/ *(blog técnico, consistente con la medición anterior; no oficial)*.
- **El tokenizer cambió y el español lo paga:** *«Claude 4.7 and later models and Claude Mythos Preview use a newer tokenizer. The same input text produces approximately 30 percent more tokens than on earlier models.»* — https://platform.claude.com/docs/en/build-with-claude/token-counting

**Takeaway:** escribir en español no cuesta lo mismo que en inglés. Y un cambio de tokenizer del proveedor puede subir tu factura sin que vos hayas cambiado nada.

---

## Ventana de contexto: qué entra y cuánto cabe (octubre 2026)

La ventana de contexto es el máximo de tokens que el modelo puede ver a la vez. **No es solo tu pregunta.** Anthropic lo documenta así: *«Everything in the request counts toward the context window: the system prompt, every message in `messages` (including tool results, images, and documents), and your tool definitions. The output Claude generates for the turn, including its extended thinking, counts too.»*

### Contexto de los modelos vigentes (fuente oficial)

| Proveedor | Modelo / línea | Contexto | Salida máx. |
|-----------|----------------|----------|-------------|
| OpenAI | GPT-6 Astra | **1.05M** | 128K |
| Anthropic | Claude Fable 5.1 / Mythos 5.1 / Opus 5.5 / Sonnet 5.5 / Haiku 5.5 | **1M** | 128K |
| Anthropic | modelos anteriores (incl. Sonnet 4.5, deprecado) | 200K | — |
| Google | Gemini 3 (serie actual) | **1M** | 64K |
| Meta | Llama 3.1 (8B/70B/405B) | 128K | — |
| Alibaba | Qwen 3.8 Max/Flash, Qwen 3.7 Plus/Flash | 1M | — |
| Alibaba | qwen-long (documentos muy largos) | **10M** | — |
| DeepSeek | V4 (flash / pro) | **1M** | 384K |
| Mistral | Large 3 / Medium 3.5 / Small 4 / Devstral 2 | 256K | — |
| Mistral | Codestral, Magistral Medium 1.2, Nemo 12B | 128K | — |

> Fuentes: https://developers.openai.com/api/docs/models · https://docs.anthropic.com/en/docs/build-with-claude/context-windows · https://ai.google.dev/gemini-api/docs/gemini-3 · https://ai.meta.com/blog/meta-llama-3-1/ · https://www.alibabacloud.com/help/en/model-studio/text-generation-model · https://api-docs.deepseek.com/quick_start/pricing · https://docs.mistral.ai/resources/known-limitations · acceso: 2026-10-07.

> ⚠️ **Modelos que el brief pedía y ya no son vigentes** (verificado 2026-10-08): GPT-4o, GPT-4o mini, GPT-4.1, Claude 3.5 Sonnet/Haiku, Claude Opus 4, Claude Sonnet 4, Claude Haiku 3.5. No figuran como vigentes, pero **sí tienen context window histórica documentada**:

| Modelo deprecado | Contexto histórico | Fuente |
|------------------|--------------------|--------|
| GPT-4o | 128.000 | https://developers.openai.com/api/docs/models/gpt-4o |
| GPT-4o mini | 128.000 | https://developers.openai.com/api/docs/models/gpt-4o-mini |
| GPT-4.1 | 1.047.576 (~1M) | https://developers.openai.com/api/docs/models/gpt-4.1 |
| Claude 3.5 Sonnet / Haiku, Opus 4, Sonnet 4, Haiku 3.5 | 200.000 | https://platform.claude.com/docs/en/build-with-claude/context-windows (regla general: todo modelo no listado como 1M = 200K) |

(Sonnet 4 tuvo 1M en **beta** desde el 12-ago-2025; la beta se retiró el 30-abr-2026.) Esta es la lección del tema: el catálogo tiene vida corta.

### Cómo se consume el contexto turno a turno

- **Acumulación:** *«As the conversation advances through turns, each user message and assistant response accumulates within the context window, and previous turns are preserved completely.»*
- **Con tool use:** cada turno arrastra los resultados de herramientas anteriores. En el turno 2, la entrada incluye *«every block in the first turn and the `tool_result`»*. Los resultados de tools **no desaparecen**: suman.
- **El thinking también ocupa:** *«With thinking, all input and output tokens, including thinking tokens, count toward the context window limit.»* Y se facturan como output.
- **Tipos de tokens facturables** (OpenAI): input, output, *cached input* y *reasoning*. Los de razonamiento *«count toward output usage and are billed as output tokens»*.

### Context rot: más contexto no es gratis

> «As token count grows, accuracy and recall degrade, a phenomenon known as context rot. This makes curating what's in context just as important as how much space is available.»

Traducción: llenar la ventana no es el objetivo. **Curar qué entra** es tan importante como cuánto espacio hay.

### Qué pasa cuando te pasás de la ventana

- Si el input solo ya excede la ventana: la API devuelve un `400 invalid_request_error` («prompt is too long») en todos los modelos.
- En Claude 4.5+, si input + `max_tokens` excede la ventana, la request se acepta y corta con `stop_reason: "model_context_window_exceeded"`.

> Fuentes de «cómo se consume/context rot/desborde»: https://docs.anthropic.com/en/docs/build-with-claude/context-windows · https://help.openai.com/en/articles/4936856-understanding-and-counting-tokens · acceso: 2026-10-07.

---

## Alucinaciones: qué son y cómo se mitigan

Definición del proveedor: *«Even the most advanced language models, like Claude, can sometimes generate text that is factually incorrect or inconsistent with the given context. This phenomenon, known as 'hallucination,' can undermine the reliability of your AI-driven solutions.»*

**Sobre la clasificación «factual / de formato / de lógica» que pedía el brief original:** no encontramos una fuente oficial ni un paper que use exactamente esa tripartición. Las encuestas académicas usan otras dicotomías: **intrínseca vs extrínseca**, y **factualidad vs fidelidad** (Huang et al., 2023 — https://arxiv.org/abs/2311.05232). Usamos la taxonomía con fuente y dejamos constancia de la corrección.

### Causas y mitigación (con fuente)

- Las encuestas catalogan la mitigación en cuatro familias: **prompt-centric, retrieval-centric, reasoning-centric y model-centric**; y la detección en retrieval-, uncertainty-, embedding-, learning- y self-consistency-based (survey 2025/2026 — https://arxiv.org/html/2510.06265v3).
- Se inventariaron **más de 32 técnicas** de mitigación, con RAG y CoVe entre las destacadas (Tonmoy et al. — https://arxiv.org/html/2401.01313v1).
- **Técnicas probadas por Anthropic** (https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations):
  1. Permitir «no lo sé».
  2. Para documentos largos (>20k tokens), exigir **citas textuales** antes de analizar.
  3. Verificar con citas y **retractar claims sin cita**.
  4. Cadena de verificación (chain-of-thought verification).
  5. Best-of-N.
  6. Refinamiento iterativo.
  7. Restringir el conocimiento a los documentos provistos.

**Advertencia del proveedor:** *«while these techniques significantly reduce hallucinations, they don't eliminate them entirely. Always validate critical information, especially for high-stakes decisions.»*

---

## Takeaway de esta sección

1. Un LLM predice tokens; no consulta una base de conocimiento.
2. El conteo de tokens no es el de palabras: depende del modelo, encoding e idioma (el español cuesta más).
3. La ventana de contexto la consumen system prompt, mensajes, tool results, definiciones de tools, imágenes y el propio razonamiento.
4. Más contexto lleno ≠ mejor: existe el *context rot*.
5. Las alucinaciones se mitigan con técnicas concretas, no se eliminan.

---

## Optimización de tokens en esta sección

- **Estimá en tokens, no en palabras.** 1.000 palabras ≈ 1.300–1.400 tokens en prosa inglesa; en español, más.
- **Contá antes de enviar.** Todos los proveedores grandes tienen tokenizer o endpoint de conteo (OpenAI `tiktoken`, `count_tokens` de Anthropic).
- **Cuidado con el cambio de tokenizer:** un modelo nuevo puede generar ~30% más tokens por el mismo texto.
- **Los tool results acumulan.** En agentes, cada resultado de tool queda en la ventana para siempre hasta que se compacte.
- **Curá el contexto.** El context rot es un problema de precisión, no solo de costo.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07:
- https://help.openai.com/en/articles/4936856-understanding-and-counting-tokens · https://platform.openai.com/tokenizer
- https://docs.anthropic.com/en/docs/build-with-claude/context-windows · https://platform.claude.com/docs/en/build-with-claude/token-counting
- https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations
- https://developers.openai.com/api/docs/models · https://ai.google.dev/gemini-api/docs/gemini-3
- https://ai.meta.com/blog/meta-llama-3-1/ · https://www.alibabacloud.com/help/en/model-studio/text-generation-model
- https://api-docs.deepseek.com/quick_start/pricing · https://docs.mistral.ai/resources/known-limitations
- https://arxiv.org/abs/1508.07909 · https://arxiv.org/abs/1808.06226 · https://arxiv.org/abs/2311.05232 · https://arxiv.org/html/2510.06265v3 · https://arxiv.org/html/2401.01313v1
- https://textkit.tech/blog/tokens-per-word-tokenizer-comparison-2026 · https://myengineeringpath.dev/genai-engineer/tokenization/

## Pendientes (no inventar si falta)

- [x] ~~Context windows históricos de GPT-4o/4.1 y Claude 3.5/4~~ → **resuelto** (ver tabla arriba, verificado 2026-10-08): GPT-4o/4o mini = 128K, GPT-4.1 = 1.047.576, Claude 3.5/Opus 4/Sonnet 4/Haiku 3.5 = 200K.
- [x] ~~Equivalencia «1M ≈ 750.000 palabras»~~ → **derivada, no literal:** el ratio oficial de Anthropic es «1 token ≈ 0,75 palabras»; 0,75 × 1M = 750.000. Se cita como derivada.
- [x] ~~Tabla de Meta~~ → **no existe una tabla única en ai.meta.com**; los datos oficiales (Llama 3.1 y 3.2 = 128K) están en blogs de ai.meta.com y en el `MODEL_CARD.md` del repo `meta-llama/llama-models`.
- [ ] Fuente que use explícitamente la tripartición factual/formato/lógica (o mantener la de Huang et al.).

---

*Sección 00 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*