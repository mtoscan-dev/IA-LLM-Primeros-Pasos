---
title: "11 — Comandos útiles en Windows, Mac y Linux"
type: seccion-curso
---

# 11 — Comandos útiles en Windows, Mac y Linux

## En 60 segundos

No necesitás ser experto de terminal para trabajar con LLMs, pero un set básico te cambia la vida: `curl` para llamar APIs, `jq` para procesar el JSON de respuesta, y algunas CLIs específicas de IA (`llm`, `aichat`, `sgpt`, `ollama`). El patrón mínimo de «LLM en una línea» es `curl ... | jq -r '.choices[0].message.content'`. Mac y Linux comparten casi todo; **Windows ya trae `curl.exe` de fábrica** y tiene sus equivalentes en PowerShell.

> Datos verificados con fuente oficial el **2026-10-08**. Los comandos de cada SO son estables, pero las versiones de las CLIs cambian: revalidá antes de publicar.

---

## El patrón base: `curl` + `jq`

### `curl` — llamar a una API de chat

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ***" \
  -d '{
    "model": "gpt-6.1-sol",
    "messages": [{"role": "user", "content": "Decime hola en una línea."}]
  }'
```

> Fuente: https://developers.openai.com/api/reference/resources/chat/subresources/completions/methods/create · https://platform.openai.com/docs/quickstart · acceso: 2026-10-07.

*(El `model` de ejemplo es del catálogo vigente — ver temas 03/04.)*

### `jq` — extraer solo lo que necesitás

> «jq can transform JSON in various ways, by selecting, iterating, reducing and otherwise mangling JSON documents. For instance, running the command `jq 'map(.price) | add'` will take an array of JSON objects as input and return the sum of their 'price' fields.»

Lo mínimo: `.` devuelve la entrada; `.foo` extrae el campo `foo`; `|` encadena filtros.

```bash
curl ... | jq -r '.choices[0].message.content'
```

> El objeto `choices` con `message` está documentado en la referencia oficial de OpenAI; el pipe a `jq` es la técnica estándar (inferencia sobre el esquema oficial).

---

## El pipeline canónico de la comunidad

Del creador del CLI `llm`:

```bash
curl -s https://www.nytimes.com/ | strip-tags .story-wrapper | ttok -t 4000 | llm --system 'summary bullet points'
```

- `strip-tags` quita el HTML; `ttok` cuenta/trunca por tokens; `llm` manda al modelo. Instalación: `pipx install llm/ttok/strip-tags`.

**Ejemplo real de conteo:** `curl -s https://simonwillison.net/ | ttok` → **21543** tokens; tras `strip-tags` → **9688**. El HTML crudo costaba *más del doble* de tokens que el texto limpio.

> Fuente: https://simonwillison.net/2023/May/18/cli-tools-for-llms/ · acceso: 2026-10-07. ⚠️ Fuente de may-2023 → posiblemente desactualizada; `llm` sigue activo.

---

## CLIs de IA — estado verificado (2026-10-08)

| CLI | Qué es | Estado / último release |
|-----|--------|--------------------------|
| **`llm`** (simonw) | «Pipeline tool» open source; modelos locales vía plugins; llega a Ollama y a cualquier endpoint compatible. | **Activo** — último release **0.36 (22 sep 2026)** |
| **ShellGPT (`sgpt`)** | Genera y ejecuta comandos de shell; «command-line productivity tool powered by AI». | **Activo** — último release **1.5.1 (6 may 2026)** |
| **`aichat`** (sigoden) | «All-in-one LLM CLI tool featuring Shell Assistant, Chat-REPL, RAG, AI Tools & Agents…». | **Baja actividad** — último release **v0.30.0 (6 jul 2025)**; no archivado, pero ~15 meses sin release |
| **`ollama`** | LLM local por CLI (ver tema 10). | Activo |

> Fuentes (GitHub, acceso 2026-10-08): https://github.com/simonw/llm/releases · https://github.com/TheR1D/shell_gpt/releases · https://github.com/sigoden/aichat/releases

---

## Comandos por sistema operativo

### Windows (PowerShell, cmd, winget) — con ejemplos oficiales

**`winget` — instalar software.** Sintaxis: `winget install [[-q] <query> ...] [<options>]`. Ejemplos reales de la doc de Microsoft:

```powershell
winget install --id Git.Git -e
winget install --id Microsoft.PowerToys
winget install --id Microsoft.PowerToys --version 0.15.2
```

> Fuente: https://learn.microsoft.com/en-us/windows/package-manager/winget/install · acceso: 2026-10-08.

**`Invoke-RestMethod` — llamar a una API.** POST con headers y body JSON; la respuesta se deserializa a `[pscustomobject]`:

```powershell
$body    = @{ model = "gpt-6.1-sol"; messages = @(@{ role = "user"; content = "hola" }) } | ConvertTo-Json
$headers = @{ Authorization = "Bearer $apiKey" }
$resp = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body -ContentType "application/json"
$resp.choices[0].message.content
```

Formas **oficiales** del `Authorization: Bearer`: (a) `-Authentication Bearer -Token $tok` (PowerShell 7.x), o (b) header manual `-Headers @{ Authorization = "Bearer $TOKEN" }`. La lectura de respuesta (deserialización a objeto) está documentada en MS Learn.

> Fuentes: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-7.6 · https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/enable-agent-to-agent-endpoint · acceso: 2026-10-08.

**`curl.exe` ya viene en Windows.** Desde **Windows 10 build 17063 (1803)** y en todo Windows 11, en `C:\Windows\System32\curl.exe`. **Ojo con el alias:** en PowerShell, `curl` es un alias de `Invoke-WebRequest` (no del binario); para usar el curl real escribí **`curl.exe`**. En `cmd.exe` no hay alias.

> Fuentes: https://techcommunity.microsoft.com/blog/containers/tar-and-curl-come-to-windows/382409 · https://curl.se/windows/microsoft.html · acceso: 2026-10-08.

### Mac (bash/zsh, Homebrew)

- `brew install llm` / `brew install aichat` para las CLIs de IA; `brew install jq` si no lo tenés; `curl` viene por defecto.

### Linux (bash)

- `curl` + `jq` + pipes. Instalación: `sudo apt install curl jq` (Debian/Ubuntu) o `sudo dnf install curl jq` (Fedora/RHEL).

### AppleScript — sin respaldo oficial para llamar a un LLM

**No hay fuente oficial de Apple** ni proyecto reconocido que documente AppleScript como *cliente* de una API de LLM. Lo que existe son ejemplos de **terceros** que usan `do shell script "curl ..."` contra la API. Los proyectos reconocidos van en la dirección inversa (el LLM **invoca** AppleScript). Se presenta como práctica de terceros, no como camino soportado.

> Fuentes (terceros): foros y gists documentados en `verificacion/tema-11.md` · https://developer.apple.com/documentation/foundation/urlsession (mecanismo oficial de Apple para HTTP) · acceso: 2026-10-08.

---

## Herramientas de organización y procesamiento de texto

- `find`, `grep`, `head`, `tail`, `wc`, `sort`, `uniq`, `cut`, `awk`, `tr` para organizar/limpiar antes de mandar al modelo.

```bash
find . -name "*.md" | head -10     # ver archivos antes de pasarlos
wc -l documento.md                 # estimar tamaño antes de enviar
```

---

## Checklist de comandos para empezar

- [ ] Sé usar `curl` para hacer requests HTTP.
- [ ] Sé usar `jq` para procesar JSON.
- [ ] En Windows: sé que `curl` es alias en PowerShell y uso `curl.exe`.
- [ ] Sé instalar herramientas (brew/apt/winget).
- [ ] Sé contar líneas/palabras antes de mandar un archivo grande.
- [ ] Sé los comandos básicos de Ollama (si uso modelos locales).

---

## Optimización de tokens con comandos

- **`ttok` y `jq` te dicen y recortan el tamaño real.** `ttok -t 4000` corta antes de enviar.
- **`strip-tags` baja el costo a la mitad o menos** (21543 → 9688 tokens en el ejemplo).
- **`wc -l` estima** antes de mandar un archivo grande; si es enorme, segmentalo.
- **Medí con la CLI, no a ojo.** El pipeline te deja ver los tokens **antes** de gastar.

---

## Fuentes de esta sección

Todas con acceso 2026-10-07/08: ver URLs citadas arriba (OpenAI, jq manpage, simonwillison.net, GitHub de las CLIs, MS Learn, curl.se).

## Pendientes (no inventar si falta)

- [ ] Nada bloqueante. (AppleScript queda como práctica de terceros; `aichat` puede quedar desactualizada con el tiempo.)

---

*Sección 11 del curso — integrada con investigación de Marco (2026-10-07) y verificación propia (2026-10-08).*