---
title: "12 — Recursos útiles y biblioteca"
type: seccion-curso
---

# 12 — Recursos útiles y biblioteca

## En 60 segundos

Esta es una biblioteca **curada**: no todo lo que existe, sino lo que sirve al lector de este curso (alguien que quiere optimizar tokens, contexto y evitar alucinaciones). Cada recurso va con una explicación de 2-4 líneas y con el dato verificado que lo respalda. Regla del curso: un recurso sin fecha, sin autor y con claims exagerados sin fuente, es sospechoso. Uno con fecha, autor, fuentes y ejemplos, es confiable.

---

## Cómo usar esta biblioteca

1. **Si estás empezando:** documentación oficial + un curso hands-on.
2. **Si querés profundizar en prompts:** papers clave + Evals.
3. **Si querés mantenerte al día:** blogs de ingeniería y comunidades.
4. **Si querés comparar modelos:** leaderboards.

No necesitás leer todo. Necesitás saber dónde mirar cuando lo necesitás.

---

## Documentación oficial (empiezá por acá)

| Recurso | Qué es | Por qué te sirve |
|---------|--------|------------------|
| **OpenAI Docs — Prompt engineering** (https://developers.openai.com/api/docs/guides/prompt-engineering) | Doc oficial | Estrategias oficiales: instrucciones claras, dividir tareas, dar tiempo a «pensar». |
| **OpenAI — Structured Outputs** (https://platform.openai.com/docs/guides/structured-outputs) | Doc oficial | La diferencia entre JSON mode y schema estricto; clave para APIs confiables. |
| **OpenAI — Understanding & counting tokens** (https://help.openai.com/en/articles/4936856-understanding-and-counting-tokens) | Doc oficial | Input/output/cached/reasoning tokens y las reglas de estimación. |
| **OpenAI Tokenizer** (https://platform.openai.com/tokenizer) | Herramienta | Visualizás en vivo cómo se parten los tokens. El modo más rápido de «ver» el problema. |
| **tiktoken** (https://github.com/openai/tiktoken) | Librería | Tokenización programática: medí antes de gastar. |
| **Anthropic — Context windows** (https://docs.anthropic.com/en/docs/build-with-claude/context-windows) | Doc oficial | Qué consume la ventana y el fenómeno «context rot». |
| **Anthropic — Token counting** (https://platform.claude.com/docs/en/build-with-claude/token-counting) | Doc oficial | Endpoint para contar antes de enviar; advierte el ~30% más del tokenizer nuevo. |
| **Anthropic — Reduce hallucinations** (https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) | Doc oficial | Técnicas accionables: permitir «no sé», citas textuales, verificación. |
| **Anthropic — Use XML tags** (https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags) | Doc oficial | Delimitadores recomendados para Claude. |
| **Google — Prompt design strategies** (https://ai.google.dev/gemini-api/docs/prompting-strategies) | Doc oficial | Principios aplicables a cualquier modelo. |
| **Google — Vertex AI prompting strategies** (https://services.google.com/fh/files/misc/1_vertex_ai_gemini_prompting_strategies.pdf) | Booklet oficial | Componentes del prompt y la recomendación de 3-5 ejemplos few-shot. |

---

## Cursos gratuitos

| Recurso | Qué es | Por qué te sirve |
|---------|--------|------------------|
| **Anthropic — Prompt Engineering Interactive Tutorial** (https://github.com/anthropics/prompt-eng-interactive-tutorial) | Curso gratuito en GitHub | **9 capítulos con ejercicios**, incluido el capítulo 8 «Avoiding Hallucinations». El más completo y hands-on. |
| **DeepLearning.AI — ChatGPT Prompt Engineering for Developers** (https://www.deeplearning.ai/courses/chatgpt-prompt-eng) | Curso gratuito, **1h40m** | Intro de Andrew Ng e Isa Fulford (OpenAI), con laboratorios. Buena puerta de entrada. |
| **Anthropic — Building effective agents** (https://www.anthropic.com/engineering/building-effective-agents) | Blog de ingeniería oficial | Patrones (chaining, routing, evaluator-optimizer) y **cuándo NO complicar**. |

> ⚠️ **Nota verificada:** el repo `anthropics/courses` fue **archivado por su dueño el 2026-09-15** (quedó read-only). El tutorial interactivo sigue disponible, pero si seguís un enlace viejo a `anthropics/courses`, puede estar congelado.

---

## Papers clave (arXiv)

No hace falta leerlos completos. Conocerlos explica de dónde vienen las técnicas.

| Paper | Tema | Por qué |
|-------|------|---------|
| **2201.11903 — Chain-of-Thought** (Wei et al., 2022) | CoT | El fundacional: por qué «pensar paso a paso» mejora el razonamiento. |
| **2005.14165 — Language Models are Few-Shot Learners** (Brown et al., 2020) | Few-shot | Origen del aprendizaje en contexto (GPT-3). |
| **2203.11171 — Self-Consistency** (Wang et al., 2022) | Confiabilidad | Muestreo + voto mayoritario; sube precisión en razonamiento. |
| **2305.10601 — Tree of Thoughts** (Yao et al., 2023) | Planificación | Patrón avanzado; 4%→74% en Game of 24. |
| **2005.11401 — RAG** (Lewis et al., 2020) | Retrieval | Origen de Retrieval-Augmented Generation. |
| **2311.05232 — Survey on Hallucination** (Huang et al., 2023) | Alucinaciones | Taxonomía y factores del problema central del curso. |
| **2401.01313 — Survey of Hallucination Mitigation** (Tonmoy et al.) | Mitigación | Inventario de **más de 32 técnicas** (RAG, CoVe…). |
| **2510.06265 — Survey detección/mitigación** | Alucinaciones | Taxonomías de detección y mitigación (2025/2026). |
| **1508.07909 — BPE** (Sennrich et al., 2015) | Tokenización | Por qué el conteo no es por palabra. |
| **1808.06226 — SentencePiece** (Kudo & Richardson, 2018) | Tokenización | Tokenizer language-independent; clave para el sobrecosto en español. |

---

## Herramientas

| Recurso | Qué es | Por qué te sirve |
|---------|--------|------------------|
| **promptfoo** (https://github.com/promptfoo/promptfoo) | CLI **open source (MIT)** para testear prompts y hacer red-teaming | La forma «ingenieril» de evitar regresiones y alucinaciones. *«Promptfoo is now part of OpenAI»* y **sigue siendo open source bajo MIT**. |
| **Hugging Face Tokenizers — Quicktour** (https://huggingface.co/docs/tokenizers/en/quicktour) | Documentación | Entrenar/usar un tokenizer BPE: puente entre el paper y el uso real. |
| **OpenAI Cookbook** (https://github.com/openai/openai-cookbook) | Repositorio de guías | Recetas reproducibles (contar tokens con `tiktoken`, evals, functions). |
| **OpenRouter Free Models Router** (https://openrouter.ai/docs/guides/routing/routers/free-router) | Router | Variantes `:free` y `openrouter/free` para probar modelos sin costo. |

---

## Herramientas de evaluación de prompts (además de promptfoo)

| Herramienta | Qué es | URL |
|-------------|--------|-----|
| **LangSmith** | Evaluación/observabilidad: datasets, evaluators, LLM-as-judge, evals online | https://www.langchain.com/langsmith |
| **Braintrust** | Observability, evals, datasets versionados, scoring LLM/código/humano | https://www.braintrust.dev/ |
| **Helicone** | Gateway LLM + observabilidad (open source) | https://www.helicone.ai/ |

## Newsletters técnicas vigentes

- **Ahead of AI** (Sebastian Raschka) — arquitectura/entrenamiento de LLMs · https://magazine.sebastianraschka.com/
- **Import AI** (Jack Clark, cofundador de Anthropic) — investigación de frontera + política · https://importai.substack.com/
- **Interconnects** (Nathan Lambert) — RLHF y modelos abiertos · https://www.interconnects.ai/
- Extra verificables: **The Batch** (DeepLearning.AI) · https://www.deeplearning.ai/the-batch/ ; **Latent Space** · https://www.latent.space/

---

## Benchmarks y leaderboards

| Recurso | Qué es | Por qué te sirve |
|---------|--------|------------------|
| **LMArena** (https://lmarena.ai/) | Benchmark de **preferencia humana** | Votos ciegos A/B y rating estilo Elo (modelo estadístico Bradley-Terry). Cross-check de benchmarks estáticos. |
| **Open LLM Leaderboard (HF)** (https://huggingface.co/open-llm-leaderboard) | Leaderboard de modelos abiertos | Evalúa con el harness de EleutherAI (ARC, HellaSwag, MMLU, TruthfulQA, GSM8K…). |

> **Nota honesta (verificada 2026-10-08):** la versión vigente es **v2 (Open LLM Leaderboard 2)**, con MMLU-Pro, GPQA, MuSR, MATH Lv5, IFEval y BBH (puntuación normalizada 0–100). El Space oficial (https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard) figura como **«Archived»**: v2 sigue siendo la última versión, pero **ya no se actualiza**. No se anunció una v3 pública.
> **RULER** (benchmark de contexto largo): https://arxiv.org/abs/2404.06654 · repo https://github.com/NVIDIA/RULER.

---

## Blogs y comunidades

| Recurso | Qué es | Por qué te sirve |
|---------|--------|------------------|
| **Simon Willison's Weblog** (https://simonwillison.net/tags/prompt-engineering/) | Blog de dev | Cobertura crítica y actualizada de LLMs, tokens y prompting. |
| **r/LocalLLaMA** (https://www.reddit.com/r/LocalLLaMA/) | Comunidad (Reddit) | Discusiones prácticas de modelos abiertos, contexto, VRAM y Ollama/LM Studio. |
| **OpenRouter Discord** (https://discord.gg/openrouter) | Comunidad (Discord) | Soporte y debate de modelos/routing del agregador. |
| **Google AI Developers Forum** (https://discuss.ai.google.dev/c/gemini-api/) | Foro oficial | Dudas de la API de Gemini y prompting. |

> **Nota honesta:** **no citamos** el número de miembros de r/LocalLLaMA. Las fuentes terciarias dan cifras contradictorias (~686k, ~773k, ~842k) y no pudimos verificarlo en la página de Reddit. Por eso, sin número.

---

## Cómo evaluar un recurso antes de usarlo

| Criterio | Qué preguntar |
|----------|----------------|
| **Actualidad** | ¿Tiene fecha? ¿Es relevante hoy? |
| **Autoría** | ¿Quién escribe? ¿Tiene credenciales o experiencia? |
| **Calidad** | ¿Tiene ejemplos? ¿Es claro? |
| **Objetividad** | ¿Opina o informa? ¿Cita fuentes? ¿Exagera? |
| **Relevancia** | ¿Sirve para mi caso? |
| **Privacidad** | ¿Pide datos o accesos innecesarios? |

**Regla práctica:** sin fecha, sin autor y con claims exagerados sin fuente = sospechoso.

---

## Checklist de recursos

- [ ] ¿Conozco la documentación oficial de los proveedores que uso?
- [ ] ¿Tengo un curso hands-on por dónde empezar?
- [ ] ¿Sé dónde buscar papers cuando me interese una técnica?
- [ ] ¿Sé qué herramientas de evaluación existen (promptfoo)?
- [ ] ¿Sé evaluar actualidad y autoría de un recurso?
- [ ] ¿Tengo 1-2 fuentes que consulto periódicamente (no «todas»)?

---

## Optimización de «tokens de atención»

Esta sección no es sobre tokens de LLM, es sobre **tokens de tu atención**. No leas recursos que no vas a usar. Elegí los que resuelven un problema tuyo concreto. Un recurso «cool» que no resolvés nada es costo sin retorno.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07 (ver URLs en las tablas). Datos verificados destacados:
- promptfoo: https://github.com/promptfoo/promptfoo
- anthropics/courses archivado: https://github.com/anthropics/courses/blob/master/prompt_engineering_interactive_tutorial/README.md
- LMArena (Bradley-Terry): https://www.lmsys.org/blog/2023-07-20-dataset/
- OpenRouter free router: https://openrouter.ai/docs/guides/routing/routers/free-router
- DeepLearning.AI (1h40m): https://www.deeplearning.ai/courses/chatgpt-prompt-eng

## Pendientes (no inventar si falta)

- [x] ~~URL y estado del Open LLM Leaderboard~~ → **resuelto** (v2, Space «Archived»).
- [x] ~~Página oficial de RULER~~ → **resuelto** (arXiv 2404.06654 · github.com/NVIDIA/RULER).
- [x] ~~Herramientas de evaluación de prompts~~ → **resuelto** (LangSmith, Braintrust, Helicone; ver arriba).
- [x] ~~Newsletters técnicas vigentes~~ → **resuelto** (Ahead of AI, Import AI, Interconnects; ver arriba).

---

*Sección 12 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*