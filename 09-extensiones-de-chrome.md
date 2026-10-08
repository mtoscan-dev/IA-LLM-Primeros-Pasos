---
title: "09 — Extensiones de Chrome útiles"
type: seccion-curso
---

# 09 — Extensiones de Chrome útiles

## En 60 segundos

Una extensión de IA bien elegida te ahorra tiempo; una mal elegida manda tus datos a un servidor desconocido y gasta tokens sin control. La regla no es «instalar todas»: es **evaluar y quedarte con las que agregan valor real**. Acá hay un listado con datos **reales de las fichas individuales de Chrome Web Store** (URL, valoración, usuarios, versión, fecha, editor y **qué datos declara en su política de privacidad**). Advertencia de método: CWS actualiza estos números **a diario**; cada cifra es una *foto del 2026-10-08*, no un dato permanente.

---

## Advertencia de método (leé esto)

- Los números de Chrome Web Store **cambian todos los días**. Se citan con **fecha de acceso 2026-10-08**.
- Un badge de CWS como *«good record with no history of violations»* **no es una auditoría de privacidad**. La página **`/privacy` del propio listing** sí declara categorías de datos y es la fuente que usamos.
- **CWS no expone el campo de licencia.** La licencia se busca en el sitio o repositorio del editor.
- Verificación hecha con navegador real, locale es-AR; las categorías se citan tal como las muestra CWS.

---

## Tabla de extensiones (13 fichas verificadas, foto 2026-10-08)

| Extensión | Rating | Usuarios | Versión | Actualizada | Editor | Datos declarados en `/privacy` |
|-----------|--------|----------|---------|-------------|--------|--------------------------------|
| **Sider** | 4.9 (114.5 K) | 5.000.000 | 5.34.1 | 1 oct 2026 | Vidline Inc. | Identificación personal; Contenido de sitios web |
| **Monica** | 4.9 (32.3 K) | 3.000.000 | 9.0.22 | 6 ago 2026 | Butterfly Effect PTE. LTD. | Identificación personal; Info financiera; Comunicaciones personales; Actividad del usuario |
| **Merlin AI** | 4.8 (8.8 K) | 900.000 | 8.3.0 | 5 oct 2026 | Foyer Tech | Identificación personal; Ubicación |
| **Glasp Web Highlighter** | 4.5 (989) | 500.000 | 2.1.4 | 4 sep 2026 | GLASP INC | Identificación personal; Contenido de sitios web |
| **YouTube Summary (Glasp)** | 3.9 (215) | 100.000 | 2.4.0 | 4 sep 2026 | GLASP INC | Identificación personal; Contenido de sitios web |
| **NoteGPT** | 4.9 (8.6 K) | 400.000 | 2.0.2.19 | 25 ago 2026 | no expuesto en CWS | **Declara NO recopilar/usar datos** |
| **Immersive Translate** | 4.0 (3.1 K) | 3.000.000 | 1.33.3 | 25 sep 2026 | no expuesto en CWS | **Declara NO recopilar/usar datos** |
| **Perplexity (Computer)** | 5.0 (**1 rating**) | 403 | 0.0.5 | 29 sep 2026 | «Ofrecido por Perplexity AI» | Autenticación; Contenido de sitios web |
| **MaxAI** | 4.7 (14.5 K) | 700.000 | 8.37.3 | 23 ago 2026 | no expuesto en CWS | Identificación personal; Actividad del usuario |
| **HARPA AI** | 4.7 (3.2 K) | 400.000 | 14.4.0 | 29 jun 2026 | HARPA AI Technologies OY | **Declara NO recopilar/usar datos** |
| **AI Prompt Genius** | 3.3 (161) | 100.000 | 5.0.2 | 29 jul 2026 | AI Prompt Genius LLC | **Declara NO recopilar/usar datos** |
| **Prompt Genius** | *(sin rating)* | 404 | 3.1.1 | 29 mar 2026 | FWAI | Identificación personal; Autenticación |
| **Prompt Genie** | 4.8 (304) | 20.000 | 6.3.1 | 9 ago 2026 | YCJ Group Inc. | **Declara NO recopilar/usar datos** |

**Compras dentro de la app:** Monica, Merlin, Glasp, YouTube Summary, HARPA, AI Prompt Genius y Prompt Genie.

**Cambios frente a la investigación previa (queda corregido):**
- **YouTube Summary (Glasp):** CWS dice **100.000 usuarios**, no «+2M» (era un dato de tercero).
- **Perplexity (Computer):** rating 5.0 con **1 sola calificación** y **403 usuarios** → **no es representativo de calidad**.
- **Immersive Translate:** rating real **4.0 (3.1K)**, no 4.5 genérico.
- **HARPA AI:** su `/privacy` vigente declara que **no recopila ni usa datos**; contradice la atribución previa (Web history/User activity). Probable cambio de política; se registra la versión vigente.

---

## Licencias (CWS no las expone → buscadas en el editor)

- **Immersive Translate** — código abierto, **GNU AGPL-3.0** (org `immersive-translate`).
- **AI Prompt Genius** — código abierto, **CC BY-NC-SA 4.0** (no comercial).
- **Glasp Web Highlighter / YouTube Summary (Glasp)** — propietaria/freemium (libre solo un plugin de Obsidian, MIT).
- **NoteGPT, Prompt Genie** — propietarias/freemium.
- **Sider, Monica, Merlin, MaxAI, HARPA, Perplexity, Prompt Genius (FWAI)** — SaaS propietario; licencia **no expuesta** por el editor en esta pasada.

---

## Cómo evaluar una extensión antes de instalar

| Criterio | Qué preguntar |
|----------|----------------|
| **Privacidad** | ¿Qué datos envía? Mirá la página **`/privacy`** del listing, no el badge. |
| **Modelo por detrás** | ¿Está declarado? (Merlin declara ChatGPT/Gemini/Claude/Mistral/DeepSeek; Glasp, ChatGPT/Claude/Mistral/Gemini.) |
| **Costo** | ¿Ofrece compras in-app? ¿Hay plan free con límites? |
| **Utilidad real** | ¿Hace algo que no podrías con el modelo directo? |
| **Permisos** | ¿Son proporcionales a lo que hace? Permisos excesivos = alerta. |
| **Licencia** | CWS no la expone; revisá el sitio/repo del editor si te importa. |

**Regla práctica:** si una extensión solo envuelve un modelo que podés usar directo **y** manda tus datos a un servidor que no conocés, probablemente estés mejor con el modelo directo.

---

## El caso Immersive Translate (cómo manejar cifras que se contradicen)

Tres números, con distinto respaldo:

| Fuente | Cifra | Alcance | Evidencia |
|--------|-------|---------|-----------|
| Chrome Web Store (contador oficial) | **3.000.000** | Solo extensión Chrome | Verificable directamente; dato duro, acotado a una plataforma |
| Sitio del editor | **30 millones** | Todas las plataformas | Autoclam repetido y fechado (informe sep-2026); **no** verificable de forma independiente; su propia ficha de Google Play dice «20M+» |
| Tercero (agregador) | **10 millones** | No especificado | Sin fuente ni fecha → **descartado** |

**Conclusión:** no hay contradicción si distinguís el alcance. Para el curso citamos el **3.000.000 de CWS** (verificable) y aclaramos que el «30M» es autoclam del editor multi-plataforma. **Es el mismo criterio que aplicás a datos de otras fuentes: reportá el alcance, no el número más grande.**

---

## Patrones de alerta (evitá estas)

- Envían todo tu tráfico o toda tu página a un servidor sin explicar por qué.
- Piden permisos para leer/modificar datos en **todos** los sitios sin razón clara.
- Desarrollador desconocido y sin actualizaciones recientes.
- Prometen «IA mágica» sin decir qué modelo usan.
- Cero valoraciones, o valoraciones sospechosas (o como Perplexity: un solo rating).

---

## Checklist de extensiones

- [ ] ¿Revisé qué extensiones de IA tengo instaladas y **qué datos declaran**?
- [ ] ¿Desactivé o desinstalé las que no uso?
- [ ] ¿Antes de instalar, leí la ficha y la página `/privacy`?
- [ ] ¿Sé qué modelo usa cada extensión (o si no está declarado)?
- [ ] ¿Elegí la extensión solo cuando agrega valor sobre el modelo directo?

---

## Optimización de tokens con extensiones

- **Una extensión que procesa toda la página** manda mucho más texto del necesario. Fijate si podés elegir qué se envía.
- **Una extensión que resume y te deja elegir la parte** es más eficiente que mandar todo el documento.
- **Cuanto más contexto agrega la extensión, más pagás en su plan** (o más rápido llegás a sus límites).

---

## Fuentes de esta sección

Todas las fichas y sus páginas `/privacy` están enlazables desde Chrome Web Store; datos verificados con navegador real el **2026-10-08**. Fuentes de licencias: https://github.com/immersive-translate (AGPL-3.0) · https://github.com/benf2004/AI-Prompt-Genius (CC BY-NC-SA 4.0) · https://glasp.co/pricing · https://www.prompt-genie.com/pricing.

## Pendientes (no inventar si falta)

- [ ] Licencia de Sider, Monica, Merlin, MaxAI, HARPA, Perplexity y Prompt Genius (FWAI): revisar TOS/repo de cada editor.
- [ ] Resolución del editor de NoteGPT, Immersive y MaxAI (CWS no lo expone).

---

*Sección 09 del curso — integrada con investigación de Marco (2026-10-07) y verificación propia de fichas (2026-10-08).*