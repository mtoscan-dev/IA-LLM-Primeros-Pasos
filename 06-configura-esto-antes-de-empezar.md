---
title: "06 — Qué configurar antes de empezar (checklist universal)"
type: seccion-curso
---

# 06 — Qué configurar antes de empezar (checklist universal)

## En 60 segundos

Antes de usar IA en serio, hay una lista corta que te ahorra problemas, costos y sorpresas: saber qué herramienta y modelo usás, poner límites de salida, configurar seguridad de comandos, **decidir qué datos no salen de tu equipo** y tener un modo de verificación. El punto donde casi todos se caen no es técnico, es de datos: la política de entrenamiento cambia según si usás un producto de consumidor gratuito o una API/capa paga. Acá está esa línea, con fuente de los tres proveedores.

---

## 1. Sabé qué herramienta y qué modelo usás

- ¿Es ChatGPT, Claude, Codex, Claude Code, un agente, una API?
- ¿Qué modelo tiene detrás? (no lo asumas — algunos productos lo cambian por defecto).
- ¿Cómo maneja el contexto (historial, archivos, tool calls, thinking)?
- ¿Cómo se cobra (suscripción, por token, free tier)?

Sin esto no podés optimizar nada.

## 2. Elegí el modelo correcto (no el más caro ni el más barato)

El más capaz no siempre es el adecuado. Usá el más barato que haga bien **esa** tarea.

## 3. Poné límites de salida (`max_tokens`)

En la API: `max_tokens` es tu freno contra respuestas infinitas. En CLIs/agentes: revisá si hay límite configurable. En UIs: si no hay control, empezá conversaciones nuevas cuando el contexto crezca.

## 4. Configurá la seguridad de comandos

En Claude Code: definí el **permission mode** para no ejecutar comandos peligrosos sin aprobación (ver tema 04). En cualquier herramienta que ejecute código, evaluá qué puede correr y con qué permisos.

## 5. Decidí qué datos salen de tu equipo (con fuente)

Este es el punto crítico. Los tres proveedores trazan la **misma línea**: **producto de consumidor gratuito = puede entrenar (opt-out); API / capa comercial paga = no entrena por defecto.**

| Proveedor | Consumidor (gratis/típico) | API / capa comercial |
|-----------|-----------------------------|-----------------------|
| **OpenAI** | *«We may use content submitted to ChatGPT and our other services for individuals to improve model performance… depending on a user's settings.»* Opt-out en **Settings > Data controls** («Improve the model for everyone»). | *«By default, we don't use inputs or outputs from ChatGPT Business, ChatGPT Enterprise, ChatGPT Edu, or our API to improve our models.»* |
| **Anthropic** | *«We may use your Inputs and Outputs to train and improve Anthropic AI models, unless you opt out through your account settings.»* | *«Anthropic may not train models on Customer Content from Services»* / *«we will not use your inputs or outputs from our commercial products (e.g. Claude for Work, Anthropic API…) to train our models.»* |
| **Google (Gemini API)** | *«When you use Unpaid Services, including… Google AI Studio and the unpaid quota on Gemini API, Google uses the content you submit… to provide, improve, and develop Google products.»* | *«When you use Paid Services… Google doesn't use your prompts… or responses to improve our products.»* |

**Excepciones que rompen la regla (leelas):**
- **OpenAI, retención API:** *«OpenAI may securely retain API inputs and outputs for up to 30 days… After 30 days, API inputs and outputs are removed from our systems, unless we are legally required to retain them.»*
- **Anthropic, feedback explícito:** aun en comercial, si *«you explicitly report feedback or bugs to us (e.g. via our thumbs up/down feedback button)… then we may use your chats and coding sessions to train our models.»*
- **Anthropic, seguridad:** si una conversación se marca para revisión de seguridad o la reportás, se usa para mejora aunque hayas hecho opt-out.
- **Google, revisión humana:** *«Chats reviewed by human reviewers… are retained for up to three years.»*
- **Google Workspace:** *«We do not use your Workspace data to train or improve the underlying generative AI… without permission.»*

> Fuentes: https://help.openai.com/en/articles/5722486-api-data-usage-policies · https://openai.com/enterprise-privacy · https://help.openai.com/en/articles/7039943-openai-privacy-policy · https://www.anthropic.com/legal/privacy · https://www.anthropic.com/legal/commercial-terms · https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training · https://ai.google.dev/gemini-api/terms · https://support.google.com/gemini/answer/13594961 · https://support.google.com/docs/answer/14615114 · acceso: 2026-10-07.

**Advertencia del propio proveedor:** *«Please do not include any sensitive, confidential, or proprietary information in the data you share.»*

## 6. Definí qué hace el modelo si no sabe algo

¿Pide más información? ¿Dice «no sé»? ¿Inventa? Esta decisión va en tu prompt o en la config de tu agente. Nunca dejes el default ambiguo.

## 7. Definí el formato de salida

¿JSON, markdown, texto libre? Definilo **antes**, no después de recibir el output.

## 8. Revisá qué skills / plugins / extensiones tenés activos

Cada uno puede agregar contexto al modelo sin que te des cuenta. Revisalos periódicamente.

## 9. Sabé cuándo empezar una conversación nueva

Conversación larga = contexto cargado. Si las respuestas se degradan (context rot), empezá de cero con solo lo necesario.

## 10. Tené un modo de verificación

¿Cómo comprobás que la respuesta es correcta? Si el output se usa en producción o en una decisión importante, verificá. No confíes solo en el modelo (ver tema 07).

---

## Retención de datos (verificado 2026-10-08)

- **OpenAI API:** retención estándar **hasta 30 días**; después se eliminan.
- **Anthropic API:** *«we automatically delete inputs and outputs on our backend **within 30 days** of receipt or generation»* (salvo Files API, ZDR, enforcement de Usage Policy o ley). Existe **Zero Data Retention (ZDR)** — se habilita **con aprobación, por organización**; no cubre Claude Free/Pro/Max ni Console. Los «Covered Models» (Fable/Mythos) **requieren 30 días** y no están bajo ZDR. Contenido marcado por violaciones de Usage Policy: hasta **2 años**.
- **Google Gemini Apps:** por defecto **18 meses** (configurable a 3 / 36 meses / indefinido); chats temporales o con «Keep Activity» off: **72 horas**; chats revisados por humanos: **hasta 3 años**.
- **Google Workspace:** no usa tus datos para entrenar «without permission».

> Fuentes: https://openai.com/enterprise-privacy · https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data · https://platform.claude.com/docs/en/manage-claude/api-and-data-retention · https://support.google.com/gemini/answer/13594961 · acceso: 2026-10-08.

---

## Errores comunes que este checklist evita (con evidencia)

- **Tratar al LLM como buscador.** *«A lot of people use ChatGPT the same way they use Google. They type in a quick question, keep it vague, and expect a great answer.»* (blog; evidencia cualitativa)
- **Aceptar la primera respuesta.** *«Beginners treat ChatGPT like a vending machine… Experts know that the first response… is essentially just a rough first draft.»*
- **No forzar razonamiento paso a paso.** El default es responder de inmediato; en tareas complejas conviene pedir la lógica antes de la conclusión.
- **Beneficio neto neutro por deuda de revisión** (hilo técnico de HN): *«They are at least about neutral these days (they used to be a productivity drain)»*; *«By the time I get done reviewing the code… I've spent the same amount of time I would've taken to write the code myself.»*
- **Deriva conversacional:** *«our biggest cost multiplier was 'conversational drift'… One 'simple' email could spiral into 15+ LLM calls.»*

> Fuentes: https://www.markbrinker.com/using-chatgpt-wrong · https://www.tomsguide.com/ai/5-signs-youre-still-using-chatgpt-like-a-beginner-and-how-to-fix-them · https://news.ycombinator.com/item?id=48933310 · https://news.ycombinator.com/item?id=46838390 · acceso: 2026-10-07. *(Evidencia cualitativa / reportes de hilos técnicos, no mediciones controladas.)*

---

## Checklist de seguridad de datos (resumen)

| Qué no enviar | Por qué |
|---------------|---------|
| Contraseñas, claves API, secretos | No compartir secretos con un modelo externo |
| Información personal de clientes/usuarios | Privacidad y responsabilidad legal |
| Información sensible de negocio sin control | Confidencialidad |
| Documentos con datos que no deberían salir | Evitar filtraciones |

**No es «nunca enviar nada».** Es «evaluá qué enviás y si vale el riesgo». Un dato no sensible a un servicio pago suele ser un riesgo distinto al de un dato de cliente en un servicio gratuito que entrena.

---

## Optimización de tokens desde el primer día

- **Elegir el modelo correcto** → no pagar de más.
- **`max_tokens`** → no generar salida innecesaria.
- **Segmentar contexto** → no dejar conversaciones crecer sin control.
- **Revisar skills/plugins/extensiones** → ver qué contexto extra agregan.

---

## Checklist resumido (post-it)

- [ ] Sé qué herramienta y modelo uso.
- [ ] Puse límite de salida si está disponible.
- [ ] Configuré seguridad de comandos si puedo ejecutar código.
- [ ] **Sé si mi plan entrena con mis datos (consumidor vs API).**
- [ ] Definí qué pasa si el modelo no sabe.
- [ ] Definí el formato de salida.
- [ ] Revisé skills/plugins/extensiones activos.
- [ ] Sé cuándo empezar conversación nueva.
- [ ] Tengo modo de verificación para outputs críticos.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07 (ver URLs citadas arriba).

## Pendientes (no inventar si falta)

- [x] ~~Cuadro de retención de Gemini Apps~~ → **resuelto** (ver «Retención de datos» arriba: 18 meses / 72 h / 3 años).
- [x] ~~Retención de Anthropic API y ZDR~~ → **resuelto** (30 días; ZDR con aprobación por organización).
- [ ] Fuente técnica primaria (no blog) para «errores comunes al empezar».

---

*Sección 06 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*