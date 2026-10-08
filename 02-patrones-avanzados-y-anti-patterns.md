---
title: "02 — Patrones avanzados y anti-patterns"
type: seccion-curso
---

# 02 — Patrones avanzados y anti-patterns

## En 60 segundos

Los patrones avanzados no son magia: cada uno tiene un paper, un mecanismo y un costo. CoT (Wei et al., 2022) mejora razonamiento en modelos grandes; few-shot (Brown et al., 2020) enseña por ejemplos sin tocar pesos; self-consistency (Wang et al., 2022) sube la confiabilidad muestreando varios caminos; Tree of Thoughts (Yao et al., 2023) explora ramas y da saltos enormes en tareas de búsqueda; RAG (Lewis et al., 2020) ancla respuestas a documentos. El anti-pattern no es «usarlos de más» ni «de menos»: es **no medir el costo en tokens y latencia** y complicar donde un prompt simple alcanzaba.

---

## Chain-of-thought (CoT)

Pedirle al modelo que exponga razonamiento intermedio antes de la respuesta final.

> «We explore how generating a chain of thought -- a series of intermediate reasoning steps -- significantly improves the ability of large language models to perform complex reasoning.»

- **Origen:** Wei et al. (2022) — https://arxiv.org/abs/2201.11903
- **Dato concreto:** *«prompting a 540B-parameter language model with just eight chain of thought exemplars achieves state of the art accuracy on the GSM8K benchmark of math word problems, surpassing even finetuned GPT-3 with a verifier.»*
- **Cuándo mejora:** *«improves performance on a range of arithmetic, commonsense, and symbolic reasoning tasks.»*
- **Cuándo NO:** las ganancias *«emerge naturally in sufficiently large language models»* — el paper no reporta el beneficio en modelos pequeños; el beneficio aparece con escala. En tareas sin razonamiento en pasos (clasificación simple), no hay ganancia esperable.

**Prompt con CoT:**

```
Antes de responder, pensá paso a paso:
1. Qué se pregunta exactamente.
2. Qué información falta y cómo obtenerla.
3. Cómo llegar a la respuesta.
4. La respuesta final.

Pregunta: [tu pregunta]
```

> ⚠️ **Honestidad sobre «CoT puede empeorar»:** circula mucho esta afirmación, pero **no encontramos fuente primaria** que diga que CoT *empeore* resultados. El paper original dice que **no ayuda / no emerge** en modelos chicos y que depende de la tarea. No afirmamos más que eso.

---

## Few-shot prompting

> Brown et al. (2020), GPT-3: *«we show that scaling up language models greatly improves task-agnostic, few-shot performance… For all tasks, GPT-3 is applied without any gradient updates or fine-tuning, with tasks and few-shot demonstrations specified purely via text interaction with the model.»* — https://arxiv.org/abs/2005.14165

- **Cómo funciona:** *«The model learns the task from the examples in the prompt without any weight updates!»* (aprendizaje en contexto).
- **Cuántos ejemplos:** Google recomienda empezar con **3-5** ejemplos de alta calidad, variar orden y elegir ejemplos semánticamente similares a la tarea.
- **Cuándo funciona:** cuando la tarea se define mejor por demostración que por descripción (formato de salida, clasificación, transformaciones). El paper nota que few-shot compite con fine-tuning en varias tareas, pero *«struggles on some datasets»* (tareas con información distribuida).
- **Riesgo:** cada ejemplo consume contexto en **cada** consulta. Más ejemplos = más tokens.

---

## Self-consistency

Muestrear varios caminos de razonamiento en lugar de tomar el greedy, y elegir la respuesta más consistente.

> Wang et al. (2022): *«It first samples a diverse set of reasoning paths instead of only taking the greedy one, and then selects the most consistent answer by marginalizing out the sampled reasoning paths.»* — https://arxiv.org/abs/2203.11171v4

**Márgenes reportados** (sobre CoT): GSM8K **+17.9%**, SVAMP +11.0%, AQuA +12.2%, StrategyQA +6.4%, ARC-challenge +3.9%.

**Intuición del paper:** *«a complex reasoning problem typically admits multiple different ways of thinking leading to its unique correct answer.»*

**Costo:** multiplica las generaciones. *No hay un multiplicador fijo* (depende de cuántas muestras pidas) y no encontramos cifra oficial de overhead. Y ojo: si todas las generaciones comparten un mismo error sistemático, la «mayoría» estará equivocada.

---

## Tree of Thoughts (ToT)

Explorar múltiples caminos de razonamiento con autoevaluación, lookahead y backtracking.

> Yao et al. (2023): *«ToT allows LMs to perform deliberate decision making by considering multiple different reasoning paths and self-evaluating choices… as well as looking ahead or backtracking when necessary to make global choices.»* — https://arxiv.org/abs/2305.10601

**Dato concreto:** en Game of 24, *«GPT-4 with chain-of-thought prompting only solved 4% of tasks, our method achieved a success rate of 74%.»*

**Cuándo se justifica:** tareas que requieren planificación o búsqueda (Game of 24, escritura creativa, crucigramas). **Es costoso** (explora ramas y se autoevalúa) — no para tareas simples. Para un curso introductorio, basta saber qué es y para qué tipo de problemas sirve.

---

## RAG básico: qué es y cuándo pensar en ello

> Lewis et al. (2020): *«models which combine pre-trained parametric and non-parametric memory for language generation… the non-parametric memory is a dense vector index of Wikipedia, accessed with a pre-trained neural retriever.»* — https://arxiv.org/abs/2005.11401

**Cómo funciona:** recuperar pasajes de un índice vectorial y condicionar la generación sobre ellos. El paper reporta que RAG *«generate more specific, diverse and factual language than a state-of-the-art parametric-only seq2seq baseline.»*

**Cuándo conviene:** cuando el modelo necesita datos que no están en su entrenamiento o querés respuestas ancladas a documentos verificables. Anthropic: *«optimizing single LLM calls with retrieval and in-context examples is usually enough.»*

**Cuándo NO pensar en RAG todavía:** si la tarea cae dentro del conocimiento del modelo y no requiere datos frescos ni trazabilidad, la complejidad no se justifica: *«we recommend finding the simplest solution possible, and only increasing complexity when needed.»*

> **Límite de alcance del curso:** RAG acá es introductorio. **No** entramos en chunking, embeddings avanzados, reranking ni hybrid search.

---

## Anti-patterns documentados

### 1. Contexto inflado (context rot)

> «As token count grows, accuracy and recall degrade, a phenomenon known as context rot.» — https://docs.anthropic.com/en/docs/build-with-claude/context-windows

Más contexto **no** es más precisión. Curar qué entra importa tanto como cuánto espacio hay.

### 2. Sin límites de formato

OpenAI: *«If you dislike the format, demonstrate the format you'd like to see… Specify the desired length of the output.»* Sin formato, output variable y difícil de parsear.

### 3. Asumir conocimiento que el modelo no tiene

OpenAI: *«These models can't read your mind… The less the model has to guess at what you want, the more likely you'll get it.»* Google: no asumas que el modelo tiene toda la información; pasásela.

### 4. Mezclar tareas sin separación

OpenAI: *«Split complex tasks into simpler subtasks… complex tasks tend to have higher error rates than simpler tasks.»* Cada subtarea debería tener **un objetivo único y claro**.

### 5. Mezclar instrucciones con datos

*«This prevents Claude from mixing up instructions with examples or context»* — para eso están los delimitadores/XML tags.

### 6. Confiar en JSON mode para un esquema

*«JSON mode will not guarantee the output matches any specific schema, only that it is valid and parses without errors.»* Si necesitás esquema estricto, usá Structured Outputs.

---

## Corrección de un claim del brief original

El brief original afirmaba que **«un modelo chico evita alucinaciones en tareas simples»**. **No pudimos verificar ese claim con ninguna fuente.** Lo que sí está documentado es:

- Que el **contexto lleno degrada** la precisión (context rot).
- Que conviene **elegir el modelo según la tarea** y usar el más barato que la haga bien.

Por eso **no lo afirmamos**. Si querés sostenerlo, hay que probarlo: la frase del curso es «No me creas. Probalo.»

---

## Checklist de patrones avanzados

- [ ] ¿La tarea requiere razonamiento paso a paso? → CoT puede ayudar (en modelos grandes).
- [ ] ¿Es un formato difícil de describir? → few-shot con 3-5 ejemplos.
- [ ] ¿La confiabilidad es crítica y podés pagar varias generaciones? → self-consistency.
- [ ] ¿La tarea requiere búsqueda/planificación? → ToT, asumiendo su costo.
- [ ] ¿Necesitás datos externos y trazabilidad? → RAG básico.
- [ ] ¿La tarea es simple? → no la compliques con patrones avanzados.

---

## Optimización de tokens en patrones avanzados

- **CoT agrega tokens** (y en modelos con thinking, se facturan como salida). Usalo cuando la mejora justifique el costo.
- **Few-shot multiplica el costo por consulta.** Cada ejemplo viaja en cada request.
- **Self-consistency multiplica por el número de generaciones.** Sin número fijo; medilo en tu caso.
- **ToT explora ramas**: el más caro de todos. Reservalo a problemas de búsqueda.
- **RAG agrega tokens de contexto externo.** Sé selectivo con lo que inyectás.
- **El output estructurado puede reducir relleno** — si está bien definido.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07:
- https://arxiv.org/abs/2201.11903 (CoT) · https://arxiv.org/abs/2005.14165 (few-shot/GPT-3)
- https://arxiv.org/abs/2203.11171v4 (self-consistency) · https://arxiv.org/abs/2305.10601 (ToT)
- https://arxiv.org/abs/2005.11401 (RAG) · https://arxiv.org/html/2510.06265v3 (survey hallucination)
- https://www.anthropic.com/engineering/building-effective-agents
- https://docs.anthropic.com/en/docs/build-with-claude/context-windows · https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags
- https://platform.openai.com/docs/guides/prompt-engineering/six-strategies-for-getting-better-results · https://platform.openai.com/docs/guides/structured-outputs
- https://services.google.com/fh/files/misc/1_vertex_ai_gemini_prompting_strategies.pdf

## Pendientes (no inventar si falta)

- [ ] Evidencia primaria de que CoT puede **empeorar** resultados (no solo «no mejora»).
- [ ] Umbral de tamaño a partir del cual CoT rinde (el abstract no lo da).
- [ ] Overhead cuantificado de self-consistency y ToT (tokens y latencia).
- [ ] Confirmar o descartar el claim «modelo chico evita alucinaciones».

---

*Sección 02 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*