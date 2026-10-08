---
title: "05 — Skills, comandos y plugins indispensables"
type: seccion-curso
---

# 05 — Skills, comandos y plugins indispensables

## En 60 segundos

Acá se mezclan tres cosas que **no son lo mismo** y conviene separar desde el arranque:

- **Skill de un agente:** conocimiento o procedimiento empaquetado que el agente carga **bajo demanda**.
- **Extensión de navegador (Chrome):** agrega funcionalidad al **navegador**.
- **Plugin / GPT / Project de un modelo:** extiende lo que hace **el modelo** dentro de su propio producto.

Cada uno vive en una capa distinta del stack y tiene su propio costo en contexto. Mezclarlos es el error más común de esta sección.

---

## A) Skills en agentes (con documentación de Hermes)

Un skill es un **documento de conocimiento que el agente carga cuando lo necesita**:

> «Skills are on-demand knowledge documents the agent can load when needed. They follow a **progressive disclosure** pattern to minimize token usage and are compatible with the agentskills.io open standard.»

Ese «progressive disclosure» es la clave del hilo de tokens: el agente **no** carga todo siempre. Carga en niveles:

| Nivel | Qué carga | Costo aproximado |
|-------|-----------|------------------|
| 0 | Lista de skills: `{name, description, category}` | ~3k tokens |
| 1 | El contenido completo del skill elegido | según el skill |
| 2 | Un archivo de referencia puntual del skill | según el archivo |

> «The agent only loads the full skill content when it actually needs it.»

**Dónde viven:** *«All skills live in `~/.hermes/skills/` — the primary directory and source of truth.»* El agente puede modificarlos o borrarlos.

**Formato:** un `SKILL.md` con frontmatter YAML (`name`, `description`, `version`…) y secciones como `## When to Use`, `## Procedure`, `## Pitfalls`, `## Verification`.

**Se invocan como slash commands:** *«Every installed skill is automatically available as a slash command»* (p. ej. `/gif-search funny cats`). Se pueden encadenar hasta 5 skills al inicio de un mensaje.

**Creación:** el agente puede crear/actualizar/borrar skills con la tool `skill_manage` (su «memoria procedimental»). Y con `/learn` convertís material propio (una carpeta, una URL, notas, un PDF) en un skill reutilizable.

### Cómo conseguir skills: el Skills Hub de Hermes

- **Buscar/instalar:** `/skills search <tema>`, `/skills inspect <id>`, `/skills install <id>`. Las skills de Hermes siguen el **estándar abierto agentskills.io**.
- **Escaneo de seguridad:** toda skill instalada desde el Hub pasa por un **security scanner** antes de escribir a disco (busca exfiltración de datos, prompt injection, comandos destructivos y señales de supply-chain).
- **Niveles de confianza:** `builtin` (incluida), `official` (repo oficial), `trusted` (registros de confianza), `community` (todo lo demás). Los hallazgos no peligrosos se pueden forzar con `--force`, pero un veredicto **`dangerous` nunca se fuerza**.
- **Especificación agentskills.io:** una skill es un directorio con un `SKILL.md` (frontmatter YAML: `name` ≤64 caracteres que coincide con el directorio; `description` ≤1024; opcionales `license`, `compatibility`, `metadata`, `allowed-tools`). **Progressive disclosure en 3 fases:** discovery (solo `name`+`description`, ~50-100 tokens), activation (cuerpo completo, <5.000 tokens recomendado) y execution (archivos de apoyo a demanda).

> Fuentes: https://hermes-agent.nousresearch.com/docs/user-guide/features/skills · https://agentskills.io/specification · acceso: 2026-10-08.

**Criterio Skill vs Tool** (la propia doc lo define):
- **Skill** cuando la capacidad es «instrucciones + comandos de shell + tools existentes» y envuelve un CLI/API externo sin requerir Python propio ni gestión de claves.
- **Tool** cuando necesita integración end-to-end con API keys, auth o lógica que debe correr siempre con precisión.

> Fuentes: https://hermes-agent.nousresearch.com/docs/user-guide/features/skills · https://hermes-agent.nousresearch.com/docs/developer-guide/creating-skills/ · acceso: 2026-10-07.

---

## B) Extensiones de navegador (Chrome)

Son **extensiones de navegador**, no skills ni plugins. El listado evaluado, con URL de Chrome Web Store, valoración y descripción real, está en el **tema 09**. Acá queda el puntero: no se duplican.

---

## C) Plugins / GPTs / Projects del modelo

### ChatGPT — GPTs, Apps y Actions

- **Qué son los GPTs:** *«GPTs (also called custom GPTs) are versions of ChatGPT configured for a specific purpose.»* Pueden conectar servicios externos vía **Apps** o **Actions** (APIs definidas por el usuario). Regla: *«A GPT can use either apps or actions, but not both at the same time.»*
- **⚠️ Los GPTs personalizados se retiran.** *«We're planning to retire custom GPTs… Instead of custom GPTs, we recommend moving your workflows to Plugins.»* Fechas verificadas (2026-10-08): **retiro estándar: 11 dic 2026**; workspaces Enterprise con aplazamiento aprobado: **11 feb 2027**. Para **Free/Plus/Pro** la doc usa lenguaje condicional («expected to follow the same timeline») → **no hay fecha distinta confirmada por plan**. Además, en cuentas personales (Free, Go, Plus, Pro) **ya no se pueden crear ni publicar nuevos GPTs**. Migración objetivo a «Plugins»: 17 sep 2026. *«Existing GPTs remain usable until retirement.»* Este es un ejemplo —oficial— de dependencia que se rompe por decisión del proveedor (ver tema 07).
- **Privacidad:** *«When you interact with a GPT that uses apps or external APIs, relevant parts of your input may be sent to the third-party service… ChatGPT may ask you to approve the request before information is sent or the action runs.»*
- **Plugins (modelo antiguo):** el repo oficial de OpenAI describe su Retrieval Plugin como usable con *«the ChatGPT plugins model (deprecated)»*.

> Fuentes: https://help.openai.com/en/articles/8554407-gpts-in-chatgpt · https://github.com/openai/chatgpt-retrieval-plugin · https://platform.openai.com/docs/plugins/ · acceso: 2026-10-07.

### ChatGPT — Projects

> «Projects keep related chats, files, and instructions together… ChatGPT can use that context to support ongoing work.»

- Pueden usar **memoria por defecto o memoria solo-del-proyecto**. Con memoria solo-del-proyecto, el contexto se limita a las conversaciones de ese proyecto.
- Admiten subir PDFs, planillas, docs, imágenes o pegar texto: *«Projects have built in memory, which means that it remembers all the chats and files you have created or uploaded in a project.»*

> Fuente: https://help.openai.com/en/articles/10169521-projects-in-chatgpt · acceso: 2026-10-07.

### Claude — Projects

- Documentados originalmente con **200K de contexto** (equivalente a un libro de 500 páginas) — *fuente 2024, posiblemente desactualizada*: hoy el contexto de Claude es mayor (ver tema 04).
- Lo que subís al proyecto se usa en **todos** los chats de ese proyecto: *«Anything you upload to this space will be used across all of your chats within that project.»*
- **RAG automático (gancho de contexto):** *«If you are using a paid Claude plan, when your project knowledge approaches the context window limit, Claude will automatically enable RAG mode to expand your project's capacity.»* Y aclara: *«Context is not shared across chats within a project unless the information is added into the project knowledge base.»*

> Fuentes: https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects · https://www.anthropic.com/news/projects · acceso: 2026-10-07.

---

## D) Comandos del sistema

Los comandos de terminal ligados a IA (`curl`, `jq`, scripting, CLIs de LLM) están en el **tema 11**. No se duplican acá.

---

## Cómo organizar tus skills / extensiones / plugins

1. **Necesario vs «agradable de tener».**
2. **Qué datos envía cada cosa** (sobre todo extensiones y plugins con Actions).
3. **Duplicados:** si una extensión y un plugin hacen lo mismo, quedate con uno.
4. **Actualización:** lo que no se actualiza puede romperse o volverse inseguro.

---

## Checklist de «indispensables»

- [ ] ¿Sé qué skills tengo disponibles y cómo se cargan (progressive disclosure)?
- [ ] ¿Revisé las extensiones de Chrome activas (qué datos envían)?
- [ ] ¿Sé si mis flujos dependen de GPTs que se van a retirar?
- [ ] ¿Conozco los comandos mínimos para hablar con una API (tema 11)?
- [ ] ¿Tengo claro qué es skill, qué es extensión y qué es plugin?

---

## Optimización de tokens en skills/plugins

- **Un skill que se carga siempre** es contexto que pagás en cada turno. El patrón correcto es cargarlo **bajo demanda** (progressive disclosure).
- **Las instrucciones enormes son caras.** Un `SKILL.md` corto + referencias que se cargan aparte es más barato que un archivo monolítico.
- **Los Projects cargan su contexto en cada chat.** Más archivos en el proyecto = más tokens por consulta.
- **Una extensión que procesa toda la página** manda mucho más texto del necesario al modelo. Fijate si podés limitar qué se envía.
- **Un plugin con Actions** agrega contexto de la definición de la tool **y** del resultado. Evaluá si vale la pena en cada caso.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07:
- https://hermes-agent.nousresearch.com/docs/user-guide/features/skills · /developer-guide/creating-skills/
- https://help.openai.com/en/articles/8554407-gpts-in-chatgpt · /10169521-projects-in-chatgpt
- https://github.com/openai/chatgpt-retrieval-plugin · https://platform.openai.com/docs/plugins/
- https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects · https://www.anthropic.com/news/projects

## Pendientes (no inventar si falta)

- [x] ~~Fecha de retiro de los custom GPTs para planes Free/Plus/Pro~~ → **parcial:** la fecha estándar es 11 dic 2026; para Free/Plus/Pro la doc solo dice «expected to follow». Enterprise con aplazamiento: 11 feb 2027.
- [x] ~~Detalle del Skills Hub de Hermes~~ → **resuelto** (ver arriba).
- [x] ~~Especificación de agentskills.io~~ → **resuelto** (ver arriba).
- [ ] Documentación técnica completa del sistema de «Plugins» que reemplaza a los GPTs.

---

*Sección 05 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*