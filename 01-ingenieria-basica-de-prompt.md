---
title: "01 — Ingeniería básica de prompt"
type: seccion-curso
---

# 01 — Ingeniería básica de prompt

## En 60 segundos

La ingeniería de prompt es comunicación estructurada con un sistema que procesa lenguaje, no intención. Un prompt completo define objetivo, contexto mínimo, formato de salida y restricciones — y no asume que el modelo «adivine» lo que querés. Los proveedores convergen en lo mismo: instrucciones claras, ejemplos cuando el formato importa, delimitadores para separar datos de instrucciones, y dividir tareas complejas. No hay trucos secretos, pero sí frameworks documentados (uno de ellos peer-reviewed) y técnicas con fuente. Y todo esto también es economía de tokens: un prompt bien estructurado evita iteraciones, que es donde se va la plata.

---

## Qué es (y qué no es)

Es estructurar la instrucción que le das a un LLM para obtener un resultado útil, predecible y con el mínimo de tokens innecesarios. No es «trucos secretos»: es comunicación explícita con un sistema que responde al lenguaje literal, no a lo que «debería» saber.

**Qué NO quiero de un prompt:** definiciones vagas. Los proveedores lo dicen sin vueltas — *«These models can't read your mind… The less the model has to guess at what you want, the more likely you'll get it.»*

---

## Frameworks de prompt (con fuente)

Google documenta los componentes que un buen prompt suele incluir: *Goal (Mission, Objective), Task (Instructions, Steps), Persona (Role), Tone, Safeguards, Context, Examples, Constraints, Output Format, Prompt Triggers, Input (Query)* (booklet oficial de Google — https://services.google.com/fh/files/misc/1_vertex_ai_gemini_prompting_strategies.pdf).

Ahora, los frameworks con nombre, con honestidad sobre cuáles están bien fundados:

### CLEAR — el único peer-reviewed

**C**oncise, **L**ogical, **E**xplicit, **A**daptive, **R**eflective. Publicado por Leo S. Lo (2023) en el *Journal of Academic Librarianship* 49(4).

- **Concise:** lenguaje claro y simple; quitar palabras irrelevantes.
- **Logical:** coherente y en orden lógico; pedir pasos de un proceso.
- **Explicit:** instrucciones específicas sobre formato, contenido o alcance; asignar persona.
- **Adaptive:** modificar el prompt según los resultados; dividir en trozos más chicos.
- **Reflective:** evaluar la salida por exactitud.

> Fuentes: https://digitalrepository.unm.edu/ulls_fsp/211/ (DOI 10.1016/j.acalib.2023.102720) · definiciones en https://guides.library.tamucc.edu/prompt-engineering/clear · nota de alcance: *«CLEAR is the only peer-reviewed framework in this set… It is not a structural template; it applies horizontally across whichever framework is used to build the prompt»* (https://guides.library.cmu.edu/LLMDocumentationGuide/PromptFrameworks) · acceso: 2026-10-07.

### RTF (Role, Task, Format)

El más minimalista: **R**ol (identidad experta), **T**area (el trabajo a completar), **F**ormato (cómo estructurar la salida). Bueno para pedidos directos: reportes rápidos, resúmenes, tablas, análisis simple, borradores de email.

> ⚠️ **Sin fuente primaria:** el framework se repite en blogs técnicos, pero **no encontramos quién lo creó ni cuándo**. Lo incluimos como patrón popular, no como estándar. Fuentes revisadas: https://nirmalrabari.in/blog/prompt-engineering-frameworks-master-guide · https://talentgroglobal.com/blog/top-12-prompt-engineering-frameworks.

### CRISPE — dos definiciones en disputa

- **Definición A** (UCD Library): Context, Role, Instruction, Subject, Preset, Exception — https://libguides.ucd.ie/GENAI/Prompting
- **Definición B** (varias fuentes la atribuyen a OpenAI): Capacity/Role, Insight, Statement, Personality, Experiment.

**No pudimos determinar la versión canónica** ni confirmar la autoría de OpenAI que algunas fuentes le atribuyen. Si lo usás, sabé que hay dos versiones circulando.

### CREATE y BROKE — sin verificar

El brief los mencionaba, pero **no encontramos fuente técnica primaria** que los defina. De CREATE solo apareció una expansión parcial («Character, Request, Comprehensive content creation…») en un documento inaccesible. **BROKE: ninguna fuente lo documenta.** Quedan fuera del curso hasta tener fuente; no los inventamos.

---

## Delimitadores: la técnica más recomendada

Separar visual y lógicamente las partes del prompt evita que el modelo mezcle instrucciones con datos. Los tres proveedores lo dicen:

- **OpenAI:** *«Put instructions at the beginning of the prompt and use ### or \"\"\" to separate the instruction and context.»* Táctica: *«Use delimiters to clearly indicate distinct parts of the input.»* — https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-the-openai-api
- **Anthropic (XML tags):** *«When your prompts involve multiple components like context, instructions, and examples, XML tags can be a game-changer… Use tags like `<instructions>`, `<example>`, and `<formatting>` to clearly separate different parts of your prompt. This prevents Claude from mixing up instructions with examples or context.»* Y sobre los nombres: *«There are no canonical 'best' XML tags… we recommend that your tag names make sense with the information they surround.»* — https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags
- **Google:** *«Use consistent structure: Employ clear delimiters… XML-style tags (e.g., `<context>`, `<task>`) or Markdown headings are effective. Choose one format and use it consistently within a single prompt.»* — https://ai.google.dev/gemini-api/docs/prompting-strategies

**Ejemplo (bien vs mal delimitado):**

```
MAL:
Traducí el texto. El texto es: "El sistema falló a las 3am y no hay logs." Traducí al inglés y respondé solo la traducción.

BIEN (OpenAI):
Traducí el texto delimitado por ### al inglés. Respondé solo la traducción.
###
El sistema falló a las 3am y no hay logs.
###

BIEN (Claude):
<instruccion>Traducí al inglés. Respondé solo la traducción.</instruccion>
<texto>El sistema falló a las 3am y no hay logs.</texto>
```

En el caso «mal», instrucción y datos están en la misma línea: el modelo puede no distinguir qué es qué.

---

## Few-shot cuando el formato importa

Dar ejemplos de input → output dentro del prompt para que el modelo aprenda el patrón.

- **Cuántos:** *«Begin with a small number of high-quality examples (3-5). Experiment with different examples and orders… Choose semantically similar examples to your target task.»* — Google.
- **Buena práctica:** *«When providing examples, try to show a diverse range of possible inputs with the desired outputs.»* — OpenAI.
- **Estrategia de barrenado:** *«Start with zero-shot, then few-shot, neither of them worked, then fine-tune.»* — OpenAI.

```
Transformá cada frase a JSON con este formato:
"El sistema X hace Y" → {"sistema": "X", "funcionalidad": "Y"}

Ejemplo 1: "Ollama corre modelos LLM localmente"
→ {"sistema": "Ollama", "funcionalidad": "correr modelos LLM localmente"}

Ejemplo 2: "OpenRouter es un agregador de APIs de modelos"
→ {"sistema": "OpenRouter", "funcionalidad": "agregar APIs de modelos"}

Ahora transformá: "Claude Code ejecuta comandos en la terminal"
```

Menos ejemplos = menos tokens. Sumá solo si mejora el resultado.

---

## Output estructurado: JSON mode vs Structured Outputs

Distinción oficial de OpenAI, importante para no comerse alucinaciones de formato:

- **JSON mode:** *«ensures that model output is valid JSON»* — pero **no** garantiza que respete tu esquema.
- **Structured Outputs:** *«reliably matches the model's output to the schema you specify»* — adherencia al schema: **sí**.

> «We recommend always using Structured Outputs instead of JSON mode when possible.»
> *«When using JSON mode, you must always instruct the model to produce JSON via some message in the conversation.»*

Habilitación: `text: { format: { type: "json_schema", strict: true, schema: ... } }` (Structured Outputs) vs `text: { format: { type: "json_object" } }` (JSON mode).

**Cuándo usar cada formato** (síntesis de las fuentes, no un dato oficial único):
- **JSON / Structured Outputs:** cuando el consumidor es **código** y necesitás schema estricto.
- **XML tags:** cuando separás secciones/estructuras de un prompt largo (especialmente con Claude).
- **Markdown:** cuando el consumidor es un **humano** que va a leer el resultado.

> Fuentes: https://platform.openai.com/docs/guides/structured-outputs · https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags · https://docs.prompts.ag/guidelines · acceso: 2026-10-07.

---

## Prompt chaining: cuándo conviene y cuándo empeora

> «Prompt chaining decomposes a task into a sequence of steps, where each LLM call processes the output of the previous one. You can add programmatic checks (see 'gate') on any intermediate steps…»

- **Cuándo conviene:** *«when the task can be easily and cleanly decomposed into fixed subtasks. The main goal is to trade off latency for higher accuracy.»* Ejemplos: copy de marketing y luego traducirlo; escribir un outline, chequearlo contra criterios, y recién después escribir el documento.
- **Cuándo empeora:** cada salto agrega latencia y tokens. La advertencia oficial: *«Agentic systems often trade latency and cost for better task performance… we recommend finding the simplest solution possible, and only increasing complexity when needed.»* Donde importa la latencia en tiempo real o los pasos son muy interdependientes, no conviene.
- **Buena práctica:** identificar subtareas, estructurar con XML para handoffs limpios, un objetivo único por subtarea, e iterar. *«Self-correction chains: You can chain prompts to have Claude review its own work!»*

> Fuentes: https://www.anthropic.com/engineering/building-effective-agents · https://anthropic.mintlify.app/en/docs/build-with-claude/prompt-engineering/chain-prompts · https://platform.openai.com/docs/guides/prompt-engineering/six-strategies-for-getting-better-results · acceso: 2026-10-07.

---

## Prompts malos → cómo se corrigen (oficial)

| Mal | Bien |
|-----|------|
| Vago: «hacé un lindo resumen» | *«Be specific, descriptive and as detailed as possible about the desired context, outcome, length, format, style, etc.»* |
| Pedís razonamiento sin espacio para pensar | *«Give the model time to 'think'… Instruct the model to work out its own solution before rushing to a conclusion.»* |
| Mezclás instrucción y datos | Delimitadores (`###`, `"""`, XML tags). |
| Tareas complejas en una sola llamada | Dividir en subtareas / chaining. |
| Confiás en JSON mode para un esquema | Usar Structured Outputs. |

**Refuerzo contra alucinación:** pedir permiso de «no sé», exigir citas, restringir el modelo a los documentos provistos.

> Fuentes: https://help.openai.com/en/articles/6654000-… · https://platform.openai.com/docs/guides/prompt-engineering/six-strategies-for-getting-better-results · https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations · acceso: 2026-10-07.

---

## Checklist antes de enviar un prompt

- [ ] ¿El objetivo es claro y específico?
- [ ] ¿El formato de salida está definido (JSON/XML/markdown)?
- [ ] ¿Los datos de entrada están delimitados si son grandes?
- [ ] ¿Hay restricciones explícitas (no inventar, longitud máxima, fuentes)?
- [ ] ¿Está claro qué hacer si el modelo no sabe algo?
- [ ] ¿Es el nivel de detalle correcto (ni vago ni inflado)?

---

## Optimización de tokens en prompts

- **Eliminá rodeos:** «Quisiera preguntarte si podrías…» → «Hacé X».
- **No repitas contexto** que el modelo ya tiene.
- **Formato compacto:** una tabla de 5 filas ocupa menos que 5 párrafos.
- **Poné máximo de palabras** cuando el output tiende a ser verboso.
- **Few-shot con el mínimo de ejemplos** que funcione: cada ejemplo consume tokens en **cada** consulta.
- **Un prompt bien estructurado evita iteraciones** — y las iteraciones son el verdadero multiplicador de costo.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07: ver URLs citadas arriba (OpenAI, Anthropic, Google, UNM/TAMUCC/CMU libraries, arXiv).
- Framework: https://digitalrepository.unm.edu/ulls_fsp/211/ · https://guides.library.cmu.edu/LLMDocumentationGuide/PromptFrameworks
- Componentes: https://services.google.com/fh/files/misc/1_vertex_ai_gemini_prompting_strategies.pdf
- Delimitadores: https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags
- Chaining: https://www.anthropic.com/engineering/building-effective-agents
- Structured Outputs: https://platform.openai.com/docs/guides/structured-outputs

## Pendientes (no inventar si falta)

- [ ] Fuente primaria de **RTF** y **CREATE** (expansión completa y autoría).
- [ ] Resolver la expansión canónica de **CRISPE**.
- [ ] Confirmar si **BROKE** existe como framework documentado (o es desinformación del brief).
- [ ] Umbral oficial de «prompt demasiado largo» (hoy solo tenemos *context rot*).

---

*Sección 01 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*