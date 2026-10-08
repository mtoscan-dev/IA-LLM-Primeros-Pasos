---
title: "10 — Ollama y LLMs locales"
type: seccion-curso
---

# 10 — Ollama y LLMs locales

## En 60 segundos

Ollama es hoy la forma más popular de correr modelos abiertos en tu propia máquina. La promesa concreta es la privacidad: *«Your prompts are never stored or trained on»* y, corriendo local, tus datos no salen del equipo. El costo no es por token sino por **hardware** (RAM/VRAM). La decisión no es «local es mejor» ni «cloud es mejor», sino: ¿mis datos pueden salir de mi equipo?, ¿mi hardware alcanza para el modelo que necesito?, ¿el modelo local hace bien mi tarea? Ollama expone además una API REST local (`localhost:11434`), así que se integra igual que cualquier proveedor.

---

## Qué es Ollama

> «Ollama is the most popular way to build with open models. Access the latest open models with complete privacy locally or in the cloud. Your prompts are never stored or trained on.»

Métricas declaradas en su home: «9M+ installs a month», «1B+ model downloads», «200T+ tokens served».

- **Backend de inferencia:** `llama.cpp` (proyecto fundado por Georgi Gerganov). Usa `llama-server` como motor para modelos **GGUF** y un motor **MLX** para safetensors en Mac.
- **API local:** REST en `http://localhost:11434` (por defecto `127.0.0.1:11434`).
- **SDKs oficiales:** `pip install ollama` (Python) y `npm i ollama` (JavaScript).

> Fuentes: https://ollama.com/ · https://github.com/ollama/ollama · https://docs.ollama.com/faq · acceso: 2026-10-07.

---

## Instalación

```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows (PowerShell)
irm https://ollama.com/install.ps1 | iex

# Docker
# imagen oficial: ollama/ollama
```

También disponible vía Homebrew, Pacman, Nix, Helm, etc.

> Fuente: https://github.com/ollama/ollama · acceso: 2026-10-07.

---

## Comandos esenciales

| Comando | Qué hace |
|---------|----------|
| `ollama` | Onboarding. |
| `ollama run <modelo>` | Chat interactivo con un modelo. |
| `ollama launch <agente>` | Integración con agentes de código: Claude Code, Codex, Copilot CLI, OpenCode, OpenClaw, DeepSeek Harness, Droid. |
| `ollama ps` | Modelos cargados en memoria. |
| `ollama serve` | Levantar el servidor. |
| `ollama signin` | Login. |

Dentro del chat: `/set parameter` (por ejemplo, `num_ctx` para la ventana de contexto).

### REST API

```bash
curl http://localhost:11434/api/chat \
  -d '{"model":"gemma4","messages":[{"role":"user","content":"..."}],"stream":false}'
```

Endpoints: `/api/chat`, `/api/generate`, `/api/embed`, etc.

> Fuentes: https://github.com/ollama/ollama · https://docs.ollama.com/faq · acceso: 2026-10-07.

### Contexto por defecto (ojo con esto)

> «By default, Ollama uses a context window size of 4096 tokens. This can be overridden with the `OLLAMA_CONTEXT_LENGTH` environment variable.»

**4.096 tokens** es **chico**. Si venís de modelos cloud con ventanas de cientos de miles o un millón de tokens, esto te va a sorprender: un modelo local bien cargado puede estar viendo muchísimo menos contexto del que esperás. Se ajusta con `OLLAMA_CONTEXT_LENGTH` o con `options.num_ctx` en la API.

---

## Hardware: qué soporta Ollama

- **NVIDIA:** *«Ollama supports Nvidia GPUs with compute capability 5.0+ and driver version 550 and newer. Nvidia GPUs with compute capability 5.0 through 6.2 require driver version 570 or newer.»* Incluye RTX 30/40/50xx y A100/H100/H200.
- **AMD:** requiere **ROCm v7** en Linux y un stack **ROCm v7 / HIP7** en Windows. Lista Radeon RX 7000/9000, PRO, Instinct y Ryzen AI.
- **Apple:** aceleración por **Metal**. Soporte adicional vía **Vulkan** en Windows y Linux.

### Cómo verificar dónde está corriendo el modelo

`ollama ps` muestra una columna **PROCESSOR** con valores como `100% GPU`, `100% CPU` o `48%/52% CPU/GPU`. Si ves `100% CPU`, está corriendo lento en CPU aunque tengas GPU.

### Optimización de memoria

- **Flash Attention** es automático si el backend lo soporta; se puede forzar con `OLLAMA_FLASH_ATTENTION=1`.
- **Cuantización de la K/V cache** con `OLLAMA_KV_CACHE_TYPE`: `f16` (default), `q8_0` (≈½ memoria), `q4_0` (≈¼ memoria).

### Dónde se guardan los modelos

| SO | Ruta |
|----|------|
| macOS | `~/.ollama/models` |
| Linux | `/usr/share/ollama/.ollama/models` |
| Windows | `C:\Users\%username%\.ollama\models` |

Reubicable con `OLLAMA_MODELS`.

### Privacidad

- **Local:** *«Ollama runs locally. We don't see your prompts or data when you run locally.»*
- **Ollama Cloud:** «we process your prompts and responses to provide the service but do not store or log that content and never train on it».
- Se puede forzar modo *local-only* con `OLLAMA_NO_CLOUD=1` o `disable_ollama_cloud` en `~/.ollama/server.json`.

> Todas las fuentes de esta subsección: https://docs.ollama.com/gpu y https://docs.ollama.com/faq · acceso: 2026-10-07.

---

## Modelos corribles localmente (tamaño y contexto, fuente oficial)

Fuente: https://ollama.com/library y páginas de modelo · info: 2026-10-07.

| Modelo (tag) | Proveedor | Contexto | Tamaño en disco |
|--------------|-----------|----------|-----------------|
| gemma3:270m | Google | 32K | 292 MB |
| gemma3:1b | Google | 32K | 815 MB |
| gemma3:4b | Google | 128K | 3.3 GB |
| gemma3:12b | Google | 128K | 8.1 GB |
| gemma3:27b | Google | 128K | 17 GB |
| llama3.1:8b | Meta | 128K | 4.9 GB |
| llama3.1:70b | Meta | 128K | 43 GB |
| llama3.1:405b | Meta | 128K | 243 GB |
| gpt-oss:20b | OpenAI | 128K | 14 GB (corre con ~16 GB RAM, MXFP4) |
| gpt-oss:120b | OpenAI | 128K | 65 GB (cabe en una GPU de 80 GB) |

El catálogo incluye además llama3.2, deepseek-r1, qwen2.5/qwen3/qwen3.5, gemma3/gemma4, mistral, qwen3-coder, llama3.3, entre muchos.

**Requisitos gpt-oss (MXFP4):** *«the smaller model to run on systems with as little as 16GB memory, and the larger model to fit on a single 80GB GPU.»*

> Fuentes: https://ollama.com/library · /library/gemma3 · /library/llama3.1 · /library/gpt-oss · acceso: 2026-10-07.

> ⚠️ **Sin regla «N parámetros = X GB».** No existe una tabla oficial única de RAM↔parámetros en la doc de Ollama. Cualquier regla tipo «7B ≈ 8 GB» es **heurística, no dato oficial**. Lo verificable son los tamaños por modelo (arriba) y la nota de MXFP4 para gpt-oss.

---

## Comparación local vs cloud (según fuentes oficiales)

| Dimensión | Local (Ollama) | Cloud (API) |
|-----------|----------------|-------------|
| Privacidad | «Ollama runs locally. We don't see your prompts or data» (modo local) | El prompt sale del equipo al proveedor (política de cada uno) |
| Costo | Gratis el software; el costo es el hardware propio | Pago por token o suscripción |
| Contexto default | 4.096 tokens (configurable) | Según modelo (hasta 1M en frontera) |
| Hardware | Requiere GPU/CPU: NVIDIA cc5.0+, AMD ROCm v7, Apple Metal, Vulkan | No local |
| Modo offline | Sí, una vez descargado el modelo | No |

**Pendientes honestos sobre esta comparación:**

- **No hay comparación de calidad cuantitativa** local vs cloud en fuentes oficiales. Existen benchmarks por modelo (p. ej. Gemma 3), pero no una comparación directa oficial local-vs-cloud.
- **No hay números oficiales de latencia local:** depende del hardware y del modelo.
- **Ollama Cloud (verificado 2026-10-08):** existe y tiene precios públicos en https://ollama.com/pricing. Planes: **Free** ($0, créditos starter), **Pro** $20/mes (incluye $60 de créditos), **Max** $100/mes ($300 de créditos, 10 concurrentes), **Team** $500/mes ($1.000 de créditos), **Enterprise** a medida. Concurrencia: Free 1, Pro 3, Max/Team 10. Modelos cobrados por millón de tokens (input/cached/output), con tarifa off-peak. Ejemplos oficiales: `gpt-oss:20b` $0.07/$0.035/$0.30; `gemma4` $0.14/$0.05/$0.40.

---

## Casos donde vale la pena local (y dónde no)

**Vale la pena cuando:**

- Tenés datos sensibles y no querés que salgan del equipo.
- Tu hardware alcanza para el modelo que necesita tu tarea.
- Querés experimentar sin costo por token.
- Tu tarea no requiere la capacidad de un modelo de frontera.

**No vale la pena (o es complicado) cuando:**

- Tu hardware no alcanza para el modelo que necesitás.
- Necesitás la calidad de un modelo de frontera.
- No querés mantener ni actualizar modelos localmente.
- El costo del hardware supera el de usar un modelo cloud para tu volumen de uso.
- Necesitás contexto muy grande (recordá el default de 4.096 tokens).

> **Nota editorial:** estas listas son criterios de decisión, no resultados medidos. El curso no afirma «local es mejor» ni al revés.

---

## Checklist de Ollama

- [ ] ¿Evalué mi hardware y qué modelos puedo correr de verdad?
- [ ] ¿Sé cuál es la ventana de contexto **real** que está usando el modelo (default 4.096)?
- [ ] ¿Corrí `ollama ps` para ver si está en GPU o CPU?
- [ ] ¿Revisé `OLLAMA_KV_CACHE_TYPE` si me falta memoria?
- [ ] ¿Decidí si necesito modo local-only (`OLLAMA_NO_CLOUD=1`)?
- [ ] ¿Entendí que un modelo local no es «mejor» que uno cloud por ser local?

---

## Optimización de tokens en LLMs locales

- **Los tokens no se cobran en dinero, pero importan igual.** El contexto default es 4.096: si subís `num_ctx` sin memoria suficiente, el modelo puede irse a CPU o volverse lentísimo.
- **Elegí el modelo por tarea, no por tamaño.** Un modelo chico puede hacer el trabajo y ocupar menos recursos.
- **Cuantizá la K/V cache antes que resignar contexto:** `q8_0` es ~½ memoria, `q4_0` ~¼.
- **Verificá con `ollama ps` si va a GPU.** Un `100% CPU` silencioso es la causa número uno de «por qué es tan lento».
- **Mismo disciplina de prompt que en cloud:** delimitadores, formato de salida definido, no prompts gigantes.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07:
- https://ollama.com/ · https://ollama.com/library · /library/gemma3 · /library/llama3.1 · /library/gpt-oss
- https://github.com/ollama/ollama
- https://docs.ollama.com/faq · /gpu · /cli · /api

## Pendientes (no inventar si falta)

- [ ] Extraer tamaños GB de más tags relevantes (qwen3.5, gemma4, mistral-small, deepseek-r1).
- [x] ~~Confirmar si existe una tabla oficial de RAM recomendada~~ → **no existe** una tabla RAM↔parámetros; solo tamaños por modelo.
- [x] ~~Esquema de precios de Ollama Cloud~~ → **resuelto** (ver arriba, https://ollama.com/pricing).
- [ ] Definir, con datos, la recomendación «local vs cloud» sin caer en opinión.

---

*Sección 10 del curso — integrada con investigación de Marco (2026-10-07) — 2026-10-08.*