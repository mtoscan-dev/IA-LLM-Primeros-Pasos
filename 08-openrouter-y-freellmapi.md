---
title: "08 — OpenRouter y freeLLMAPI"
type: seccion-curso
---

# 08 — OpenRouter y freeLLMAPI

## En 60 segundos

OpenRouter y freeLLMAPI son **dos modelos distintos del mismo concepto**: hablar con muchos modelos por un solo endpoint compatible con OpenAI. OpenRouter es un **agregador comercial**: rutea a cientos de modelos, hace fallback automático y pasa el precio del proveedor sin markup (cobra comisión al cargar créditos). freeLLMAPI es un **router open source y self-hosted**: corre en tu máquina y pone detrás de un mismo endpoint a todos los proveedores que tengan free tier real. Uno es pago y gestionado; el otro es gratis y lo administrás vos. Y como siempre en este curso: **los free tiers existen, pero casi nunca son «ilimitados»**, y varios de los que circulan por ahí no son gratis en absoluto.

---

## Qué es OpenRouter

> «OpenRouter gives you access to hundreds of AI models through a single API endpoint. It handles fallbacks automatically and picks the most cost-effective option for each request.»

- **API compatible con OpenAI:** `POST https://openrouter.ai/api/v1/chat/completions`. Podés apuntar el SDK de OpenAI a `baseURL: 'https://openrouter.ai/api/v1'`.
- **Routing y fallback:** ante un error de un proveedor, cae automáticamente al siguiente; la selección de proveedor es configurable.
- **Variantes:** `:nitro` (throughput), `:floor` (precio), `:exacto` (calidad/tool-calling), `:free` (gratuita con sus límites), `:batch` (precio batch).
- **Casos de uso documentados:** API directa, SDKs cliente (`@openrouter/sdk`, `pip install openrouter`), Agent SDK (`@openrouter/agent`) y MCP server remoto.

> Fuentes: https://openrouter.ai/docs/quickstart · https://openrouter.ai/docs/faq · acceso: 2026-10-07/08.

### Pricing de OpenRouter

- **Pass-through sin markup sobre la inferencia:** *«We pass through the pricing of the underlying providers; there is no markup on inference pricing (however we do charge a fee when purchasing credits).»*
- **Fee al cargar créditos: 5.5% ($0.80 mínimo).** Cripto: 5%.
- **BYOK:** el pay-as-you-go incluye $25.000/mes sin fee de BYOK; por encima, 5%.

### Free tier y límites (página oficial)

- **Página oficial de límites:** *«API Credit & Rate Limits»* — https://openrouter.ai/docs/api-reference/limits.
- **Límite de modelos `:free`:** con menos de 10 créditos comprados (all-time): **20 req/min · 50 req/día**; con 10+ créditos comprados: **20 req/min · 1.000 req/día**. El límite por minuto **no** sube comprando más.
- **Router automático:** `openrouter/free` selecciona automáticamente un modelo gratuito (https://openrouter.ai/docs/faq).

### Privacidad

- *«We log basic request metadata (timestamps, model used, token counts). Prompt and completion are not logged by default… unless you opt-in.»*
- El **opt-in** da **1% de descuento** a cambio de loguear prompts y completions.

> Fuentes: https://openrouter.ai/docs/faq · /api_reference/overview · /api-reference/parameters · acceso: 2026-10-07.

---

## freeLLMAPI: existe, y es otra cosa

**Verificado: freeLLMAPI es un proyecto real.**

> «FreeLLMAPI is a free LLM API: an open-source, self-hosted router that puts every provider with a real free tier behind a single OpenAI-compatible endpoint.»

- **Cómo funciona:** responde en `/v1/chat/completions` (y el resto de superficies OpenAI, más la *Messages API* de Anthropic); los clientes solo cambian el `base_url`. Corre en `http://localhost:3001/v1`.
- **Instalación:**
  ```bash
  curl -fsSL https://freellmapi.co/install.sh | bash   # macOS/Linux/WSL
  iwr -useb https://freellmapi.co/install.ps1 | iex    # Windows
  ```
- **Advertencia del propio sitio:** *«open source, self-hosted, single-user… Built for personal use only… Don't expose this proxy publicly.»*

### Repositorio y licencia (corregido)

- **El repositorio oficial es `github.com/tashfeenahmed/freellmapi`** — **MIT**, activo (851 commits, último commit el mismo día de esta verificación). Su descripción: *«7.4 billion tokens per month. 34 free LLM providers. 635 free model endpoints.»*
- ⚠️ **`github.com/mmnaderi/freellmapi` NO es el upstream:** es un fork desactualizado (0 estrellas, ~800 commits por detrás). La investigación previa lo citaba por error; queda corregido.
- **Modelo de negocio:** el router y el código son **MIT / gratis para siempre**. Lo que se paga es el **catálogo «live» Premium**: $19/año o $49 pago único (vendedor: Neu Software LLC).

### La contradicción de cifras (no se resuelve — se reporta)

El propio proyecto publica **al menos cuatro juegos de números** que no cuadran entre sí:

| Fuente oficial | Modelos | Proveedores | Endpoints |
|----------------|---------|-------------|-----------|
| Home (freellmapi.co) | 855 | 51 | — |
| /about y /faq | 600+ | 34 | — |
| /models (catálogo real) | 343 | 25 | 312 |
| README (GitHub) | 474 familias | 34 | 635 |

**Regla del curso:** cuando una fuente tiene cifras contradictorias, se reportan todas y no se elige la que más te gusta. Acá el **34 proveedores** es lo más consistente entre /about, /faq y README; el 855/51 solo aparece en la home.

> Fuentes: https://freellmapi.co/ · /about · /faq · /models · https://github.com/tashfeenahmed/freellmapi · acceso: 2026-10-08.

---

## Free tiers de proveedores (verificado en sitio oficial, oct 2026)

| Proveedor | ¿Free tier? | Límite concreto |
|-----------|-------------|-----------------|
| **Groq** | **Sí** | Por modelo (ej. gpt-oss-120b, qwen3.8-27b): 30 req/min · 1.000 req/día · 8K tokens/min · 200K tokens/día |
| **Google AI Studio / Gemini** | **Sí** | Tiene tier gratuito, pero **la doc no publica la tabla de límites** (se consulta en AI Studio) |
| **Mistral** | **Sí** | «free API tier» para evaluación; **no publica cifras** (se ven en el panel) |
| **Cohere** | **Sí** | Trial key: 1.000 llamadas/mes; chat 20 req/min |
| **Cloudflare Workers AI** | **Sí** | 10.000 Neurons/día (plan Workers Free) |
| **Z.ai (Zhipu)** | **Sí** | GLM-4.5-Flash y GLM-4.7-Flash gratis; sin RPM/TPM publicados |
| **NVIDIA NIM** | **Créditos** | 1.000 créditos al registrarse (5.000 con email corporativo); no es ilimitado |
| **Hugging Face Inference Providers** | **⚠️ Contradictorio** | La propia doc se contradice: una página dice $0,10/mes para Free; otra dice «None». PRO: $2/mes |
| **Cerebras** | **⚠️ Solo trial** | Trial de $5 que expira a los 30 días (requiere tarjeta); la doc dice que **no** hay tier gratuito recurrente |
| **Together AI** | **❌ No** | *«Together AI does not currently offer free trials. Access… requires a minimum $15 credit purchase.»* |

**Qué evitar:** no asumir que «free» significa «sin límites», y no listar como gratis a proveedores que exigen pago (Together, Cerebras). La doc de NVIDIA y la de Hugging Face tienen páginas que se contradicen o dan 404 — por eso este curso cita el estado real al 2026-10-08.

> Fuentes (acceso 2026-10-08): https://console.groq.com/docs/rate-limits · https://ai.google.dev/gemini-api/docs/rate-limits · https://help.mistral.ai/en/articles/392924 · https://docs.cohere.com/v2/docs/rate-limits · https://developers.cloudflare.com/workers-ai/platform/pricing/ · https://docs.z.ai/guides/overview/overview · https://developer.nvidia.com/blog/access-to-nvidia-nim-now-available-free-to-developer-program-members/ · https://huggingface.co/docs/inference-providers/pricing · https://inference-docs.cerebras.ai/support/rate-limits · https://docs.together.ai/docs/billing-credits

---

## Comparación: OpenRouter vs puente directo al proveedor

| Dimensión | OpenRouter (agregador) | Puente directo al proveedor |
|-----------|------------------------|------------------------------|
| Pricing de inferencia | Pass-through, sin markup; fee 5.5% al cargar créditos | Precio del proveedor directo |
| Model switching | Un endpoint, cientos de modelos, cambiás el slug | Integrar SDK/endpoint por proveedor |
| Resiliencia | Fallback automático a otro proveedor | Sin fallback salvo que lo implementes |
| Facturación | Créditos unificados + analytics | Billing por proveedor |
| Latencia | Suma un salto (proxy); `:nitro` prioriza throughput | Sin salto de agregador |
| Dependencia | Depende de la continuidad de OpenRouter | Depende solo del proveedor |

**Pendiente:** no hay medición cuantitativa de latencia agregador vs directo. Queda como benchmark propio fuera de alcance.

---

## Elegir el modelo correcto (el hilo de tokens)

No tenemos una medición propia que pruebe que «modelo más grande = menos alucinaciones» (ver tema 02). Lo verificable:

- **El costo escala con el modelo, el largo del prompt y el tamaño del contexto.**
- **Existe evidencia de terceros** de que rutear tareas de clasificación a un modelo chico recorta costos fuerte (reporte de un hilo técnico, −60% mensuales en un caso).
- **La decisión es por tarea:** usá el modelo más barato que haga bien *esa* tarea, y verificá el resultado. «No me creas. Probalo.»

---

## Checklist de OpenRouter / freeLLMAPI

- [ ] ¿Entiendo que son dos cosas distintas: agregador pago vs router self-hosted?
- [ ] ¿Sé qué modelos tengo y a qué precio (input/output por 1M)?
- [ ] ¿Revisé los límites reales del free tier que voy a usar (no asumir «ilimitado»)?
- [ ] ¿Leí la advertencia de freeLLMAPI de **no exponer el proxy públicamente**?
- [ ] ¿Probé la misma tarea con dos modelos y comparé resultado y tokens?

---

## Optimización de tokens en OpenRouter / freeLLMAPI

- **Un endpoint, muchos modelos:** compará el mismo prompt entre modelos cambiando el slug.
- **`:floor` para precio, `:nitro` para velocidad.**
- **Fallback no es neutral:** si cae a otro proveedor, podés terminar pagando un modelo más caro sin querer.
- **El opt-in de logging de OpenRouter da 1% de descuento** a cambio de loguear tus prompts — decisión consciente.
- **En freeLLMAPI, los free tiers tienen límites reales:** rutear todo por ahí sin entenderlos choca con un 429.
- **Cuidado con los «gratis»:** Together pide $15 mínimo y Cerebras es solo trial. Verificá antes de confiar.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07/08: ver URLs citadas arriba (OpenRouter, freeLLMAPI, proveedores).

## Pendientes (no inventar si falta)

- [ ] Benchmark propio de latencia OpenRouter vs directo.

---

*Sección 08 del curso — integrada con investigación de Marco (2026-10-07) y verificación propia (2026-10-08).*