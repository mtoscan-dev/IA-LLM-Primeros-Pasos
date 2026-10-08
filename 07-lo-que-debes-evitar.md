---
title: "07 — Qué debes evitar"
type: seccion-curso
---

# 07 — Qué debes evitar

## En 60 segundos

Esta sección no es teoría: son costos y fallas **documentados**. Costos ocultos que son multiplicadores (retries, tool fanout, crecimiento del contexto en el P95, deriva conversacional), dependencias que se rompen por decisión del proveedor, y alucinaciones que llegaron a tribunales y costaron plata real. La regla de oro: el riesgo no está en «usar IA», está en **no verificar** y en **no medir el costo real**.

---

## Costos ocultos: multiplicadores, no el precio por token

El precio por token es lo visible. El gasto real lo mueven **multiplicadores**. En un hilo técnico de HN, un equipo reporta:

- *«our biggest cost multiplier was 'conversational drift' — not the initial call, but what happens when you let users iterate… One 'simple' email could spiral into 15+ LLM calls.»*
- Multiplicadores que listan: *«retries/429s, tool fanout, P95 context growth, and safety passes»*.
- *«the 90th percentile request often costs 10x the median. A handful of pathological queries can dominate your bill.»*
- **Mitigaciones que reportaron** (y que aplican al curso): presupuestos **por sesión** (no por request), señales explícitas de «terminado», **cascada a modelos más baratos** y **caché semántica**. *«it cut iteration costs ~80%».*

> Fuente: https://news.ycombinator.com/item?id=46838390 · acceso: 2026-10-07. *(Reporte de hilo técnico; no es medición controlada.)*

**Facturas no lineales al escalar** (agregador secundario de un hilo de HN, mar-2026): *«We hit $4,000 in OpenAI costs in month two. At our price point, we needed 400 paying users just to break even on inference alone.»* Y: *«The moment you get 50 real users with unpredictable query patterns, the bill goes completely non-linear.»* En el mismo resumen: *«One developer reported cutting monthly costs by 60% by routing classification tasks to a fine-tuned Mistral model instead of GPT-4o.»*

> Fuente: https://agent-wars.com/news/2026-03-14-llm-inference-cloud-costs-hn-discussion · acceso: 2026-10-07. ⚠️ **No se pudo verificar (2026-10-08):** el artículo ahora devuelve **HTTP 410 (eliminado)** y **no existe** un hilo correspondiente en Hacker News (buscado en la API de Algolia). Estas cifras ($4.000, −60%) quedan como **no confirmadas con fuente primaria**; se citan solo como reporte de tercero.

**Mecanismo del costo:** *«LLM costs depend on prompt length, context window size, and model choice — all of which swing wildly depending on use case and user behavior.»*

---

## Errores de desarrolladores (documentados)

- **Beneficio neto neutro y deuda de revisión** (HN): *«They are at least about neutral these days (they used to be a productivity drain)»*; *«By the time I get done reviewing the code to make sure it hasn't done anything crazy, I've spent the same amount of time I would've taken to write the code myself.»*
- **Aceptar el primer output / no verificar hechos:** *«Beginners treat ChatGPT like a vending machine»*; *«Trusting unverified facts… ChatGPT can make mistakes or present outdated information.»*

> Fuentes: https://news.ycombinator.com/item?id=48933310 · https://www.tomsguide.com/ai/5-signs-youre-still-using-chatgpt-like-a-beginner-and-how-to-fix-them · acceso: 2026-10-07. *(Evidencia cualitativa.)*

---

## Dependencias riesgosas

- **Un solo proveedor.** En el hilo de costos, la mitigación recurrente es *«tiered model routing»* y *«fallback… rather than letting costs explode»*: los propios devs tratan el riesgo de depender de un único endpoint como un **problema de arquitectura**.
- **Features que el proveedor retira.** Caso oficial: OpenAI **retira los custom GPTs** y mueve los flujos a «Plugins» (migración objetivo **sep 2026**; retiro Enterprise planificado **11 dic 2026**). Un producto construido sobre esa feature queda superado por decisión de terceros.
- **APIs internas que se descontinúan.** OpenAI deprecó la **Assistants API** (*«It will shut down on August 26, 2026»*) y la **Realtime API Beta** (*«removed from the API on February 27, 2026»*).

> Fuentes: https://news.ycombinator.com/item?id=46838390 · https://help.openai.com/en/articles/8554407-gpts-in-chatgpt · https://platform.openai.com/docs/llms-full.txt · acceso: 2026-10-07.

---

## Alucinaciones en producción: casos reales

Estos no son hipotéticos. Tienen sentencia, resolución o cobertura periodística.

### 1. Mata v. Avianca (EE.UU., 2023) — seis casos legales inventados

*«Schwartz had used ChatGPT to research legal precedents… The AI generated six compelling cases, complete with detailed citations, procedural histories, and relevant quotations. All six were entirely fictitious.»* Resultado: el **22 de junio de 2023** el juez P. Kevin Castel impuso **sanciones de $5.000** a dos abogados.

> Fuentes: https://www.clever.legal/en/blog/ai-hallucinations-in-court-when-the-algorithm-lies-and-lawyers-pay-the-price · corroboración periodística independiente: https://www.reuters.com/technology/artificial-intelligence/ai-hallucinations-court-papers-spell-trouble-lawyers-2025-02-18/ · acceso: 2026-10-07.

### 2. Moffatt v. Air Canada (Canadá, 2024 BCCRT 149) — el chatbot inventó una política

El chatbot de Air Canada dijo que las tarifas de duelo se podían aplicar retroactivamente; la aerolínea después lo negó. El tribunal: *«While a chatbot has an interactive component, it is still just a part of Air Canada's website… It makes no difference whether the information comes from a static page or a chatbot.»* Air Canada fue responsable por **negligent misrepresentation**; daños: **CA$812,02**.

> Fuentes: https://www.law360.ca/ca/articles/1804075/court-rejects-air-canada-s-remarkable-denial-of-liability-regarding-misinformation-by-its-chatbot · https://www.mccarthy.ca/en/insights/blogs/techlex/moffatt-v-air-canada-misrepresentation-ai-chatbot · acceso: 2026-10-07.

### 3. Google AI Overviews (EE.UU., may-2024) — respuestas peligrosas a escala

El buscador sugirió usar *«non-toxic glue»* para que el queso se pegue a la pizza y recomendó *«eat one rock per day»*. Google las llamó «ejemplos aislados»; Sundar Pichai dijo que las alucinaciones eran *«an unsolved problem»* y «en cierto modo una característica inherente».

> Fuente: https://www.bbc.com/news/articles/cd11gzejgz4o · acceso: 2026-10-07. ⚠️ Fuente de 2024; el estado del producto puede haber cambiado, pero el caso es de la fecha.

### 4. Escala — alucinaciones en tribunales, contadas (fuente primaria)

La base de datos **primaria** es la de Damien Charlotin: *«While seeking to be exhaustive ( **2149** cases identified so far)»* — última actualización **5 oct 2026**. Desglose por naturaleza: Fabricated 1764 · False Quotes 579 · Misrepresented 903 · Outdated Advice 35.

> Fuente: https://www.damiencharlotin.com/hallucinations/ (licencia CC BY 4.0) · acceso: 2026-10-08. *(La cifra «206» que circula en blogs era un dato secundario y desactualizado.)*

> **Nota metodológica:** los casos 1 y 2 son los «mínimo 2 reales con fuente». El 3 y el 4 se incluyen como refuerzo.

### 5-7. Casos fuera del ámbito judicial (2025)

- **Deloitte (Australia, 2025) — informe gubernamental con citas académicas inexistentes.** Un informe «Future Made in Australia» de A$439.000 incluyó citas inventadas (incluida una de la Federal Court). Deloitte admitió haber usado **GPT-4o** y devolvió la cuota final. Fuente periodística: Guardian (vía Financial Express / NDTV) · acceso 2026-10-08.
- **Cursor (atención al cliente, 2025) — el bot «Sam» inventó una política.** El bot de soporte de Cursor afirmó una política inexistente de un solo dispositivo por suscripción; el cofundador lo reconoció como una respuesta incorrecta del bot. Fuente: Fortune / The Register · acceso 2026-10-08.
- **Salud — estudio de Mount Sinai (2025).** Publicado en *Communications Medicine* (2-ago-2025): tasas de alucinación de **50%–82%** en seis chatbots ante términos médicos fabricados. Fuente: Mount Sinai / HealthDay · DOI 10.1038/s43856-025-01021-3 · acceso 2026-10-08.

---

## Qué evitar — resumen operativo

**En prompts:**
- Prompts ambiguos, sin objetivo ni formato.
- Mezclar tareas sin separación.
- Asumir conocimiento que el modelo no tiene.
- No decir «no inventes» / no definir qué pasa si no sabe.
- Cargar contexto irrelevante.

**En herramientas:**
- No configurar límites de salida.
- No configurar seguridad de comandos.
- No segmentar conversaciones largas.
- No revisar skills/plugins/extensiones activos.
- No evaluar privacidad antes de enviar datos.
- No tener verificación para outputs críticos.
- Depender de un solo proveedor sin fallback.

---

## Checklist de «no hacer»

- [ ] No dejar conversaciones gigantes abiertas en modelos de pago por token.
- [ ] No confiar en la respuesta sin verificación (menos aún en producción).
- [ ] No enviar datos sensibles sin evaluar la política de entrenamiento.
- [ ] No usar el modelo más caro para tareas simples.
- [ ] No mezclar tareas en un solo prompt.
- [ ] No asumir que el modelo sabe algo que no tiene.
- [ ] No instalar extensiones/plugins sin ver qué datos envían.
- [ ] No depender de un solo proveedor sin plan de fallback.
- [ ] No construir sobre features que el proveedor ya anunció que retira.

---

## Optimización de tokens desde «qué evitar»

- **Contexto infinito** → tokens que no necesitás.
- **Iteraciones sin control** → el verdadero multiplicador del gasto.
- **Modelo grande para tareas chicas** → sobrecosto evitable.
- **Contexto irrelevante** → tokens sin retorno.
- **Deriva conversacional** → presupuestá por sesión, no por request.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07 (ver URLs citadas arriba).

## Pendientes (no inventar si falta)

- [x] ~~Casos de alucinación 2025-2026 fuera del ámbito judicial~~ → **resuelto** (Deloitte, Cursor, Mount Sinai; ver arriba).
- [x] ~~Base primaria de Damien Charlotin~~ → **resuelto:** https://www.damiencharlotin.com/hallucinations/ (2.149 casos al 5-oct-2026).
- [ ] Hilo original de HN detrás del agregador agent-wars → **no existe / no verificable** (ver aviso arriba). Se descarta esa fuente.
- [ ] Fuente oficial de pricing para el hilo «modelo grande vs chico».

---

*Sección 07 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*