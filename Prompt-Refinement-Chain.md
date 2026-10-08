# Prompt Refinement Chain — Guía de Uso

> Cadena de dos prompts que trabajan en loop para mejorar cualquier prompt hasta su versión óptima. Pensada para usuarios sin experiencia técnica.

---

## ¿Qué es esto?

Son **dos prompts diseñados para trabajar juntos**. El primero analiza y puntúa tu prompt. El segundo usa ese puntaje para mejorarlo. Se repiten en loop hasta que el puntaje deja de subir.

No necesitás saber programar. Solo necesitás poder copiar y pegar texto en un chat con una IA (ChatGPT, Claude, Gemini, etc.).

---

## Los dos pasos

### Paso 1 — El Analizador

**Qué hace:** Lee tu prompt y lo evalúa como si fuera un profesor corrigiendo un trabajo. Pero en vez de poner "bien" o "mal", te da un informe detallado con puntos y sugerencias.

**Cómo puntúa:** Evalúa 15 criterios agrupados en 4 categorías. Cada criterio se puntúa de 1 a 5. El máximo posible es **75 puntos** (15 criterios × 5 puntos).

**Las 4 categorías que evalúa:**

| Categoría | Qué mira | Criterios |
|-----------|----------|-----------|
| **Estructura y Claridad** | ¿El prompt está bien armado? ¿Se entiende? | Claridad, organización, formato, equilibrio entre corto y detallado |
| **Contexto y Propósito** | ¿Da la información necesaria? ¿Sabe qué quiere lograr? | Fondo, tarea clara, rol asignado, audiencia definida |
| **Calidad de Instrucciones** | ¿Las instrucciones son buenas? ¿Tiene ejemplos? | Formato de salida, razonamiento paso a paso, sin ambigüedades, ejemplos |
| **Viabilidad** | ¿El prompt es realista? ¿Funciona bien con el modelo? | Potencial de mejora, adecuación al modelo, factibilidad |

**Qué entrega:** Un informe con:
- Puntuación de cada criterio (1-5) con su justificación
- Una fortaleza y una sugerencia de mejora para cada criterio
- Puntaje total sobre 75
- 3-5 sugerencias priorizadas por impacto

### Paso 2 — El Refinador

**Qué hace:** Recibe tu prompt original + el informe del Paso 1, y reescribe el prompt aplicando las mejoras sugeridas.

**Cómo decide qué cambiar:**
- **Primero** corrige lo que peor está puntuado (los criterios con 1 o 2 puntos)
- **Después** mejora lo que está en el medio (3 puntos)
- **Al final** refina lo que ya estaba bien (4 o 5 puntos) si queda algo por pulir

**Qué protege:** No cambia la idea original del prompt. Si tu prompt era para escribir emails de ventas, el refinado sigue siendo para eso. Solo mejora cómo está escrito.

**Qué entrega:** Solo el prompt refinado, listo para usar o para volver a evaluar.

---

## Cómo usarlos en loop

El poder real de estos prompts está en usarlos **en cadena**, repitiendo el ciclo hasta que el puntaje se estabilice.

### Diagrama del loop

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   Tu prompt ──→ PASO 1 (Analizador) ──→ Informe        │
│       │                                     │           │
│       │                                     ▼           │
│       └──────────────→ PASO 2 (Refinador) ←─┘           │
│                              │                          │
│                              ▼                          │
│                      Prompt refinado                    │
│                              │                          │
│              ¿Mejoró el puntaje?                        │
│                 │              │                        │
│                SÍ             NO ──→ FIN                │
│                 │                                       │
│                 ▼                                       │
│          Volver a PASO 1                                │
│          con el prompt refinado                         │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Paso a paso detallado

**Iteración 1:**

1. Abrí una conversación nueva con tu IA
2. Pegá el **prompt del Paso 1** (el Analizador) como primer mensaje
3. Cuando el Analizador pregunte "¿qué prompt desea evaluar?", pegá **tu prompt original**
4. Guardá el informe completo que genera (el que tiene el puntaje sobre 75)
5. Abrí **otra conversación nueva** (o limpiá el contexto)
6. Pegá el **prompt del Paso 2** (el Refinador) como primer mensaje
7. Cuando pida el prompt original, pegá **tu prompt original**
8. Cuando pida el informe, pegá **el informe del Paso 1**
9. Guardá el prompt refinado que devuelve

**Iteración 2 en adelante:**

10. Abrí una conversación nueva
11. Pegá el **prompt del Paso 1** (Analizador)
12. Cuando pida el prompt, pegá **el prompt refinado** (el del paso anterior)
13. Guardá el nuevo informe
14. Abrí otra conversación nueva
15. Pegá el **prompt del Paso 2** (Refinador)
16. Cuando pida el prompt original, pegá **el prompt refinado** (el mismo que evaluaste)
17. Cuando pida el informe, pegá **solo el informe más reciente**
18. Guardá el nuevo prompt refinado

**Repetí** los pasos 10-18 hasta que el puntaje deje de subir o suba menos de 2-3 puntos entre iteraciones. En ese punto, tu prompt está en su mejor versión.

### ¿Cuándo parar?

| Señal | Qué hacer |
|-------|-----------|
| El puntaje subió 5+ puntos | Seguí iterando, hay mejora clara |
| El puntaje subió 2-4 puntos | Podés iterar una vez más o parar |
| El puntaje subió 0-1 puntos | Pará, el prompt está optimizado |
| El puntaje bajó | Pará, la iteración anterior era mejor |

---

## Ejemplo rápido

**Prompt original** (mal escrito):
> "Haceme un resumen de este artículo."

**Después de 1 iteración** (mejorado por el Refinador):
> "En 3-5 oraciones, resumí los puntos principales de este artículo para un lector general que no tiene conocimientos previos del tema. Mantené un tono claro y directo, evitando jerga técnica."

**Después de 2 iteraciones** (optimizado):
> "Rol: Editor de divulgación científica. Tarea: Resumí el siguiente artículo en exactamente 3 oraciones. Audiencia: Lector general sin conocimientos técnicos. Estilo: Claro, directo, sin jerga. Estructura: (1) qué se descubrió, (2) por qué importa, (3) qué queda por investigar. Si el artículo no tiene alguna de estas tres partes, omitila en lugar de inventarla."

---

## Consejos para mejores resultados

1. **Usá conversaciones separadas** para cada paso. No mezcles el Analizador y el Refinador en la misma conversación, porque la IA puede confundir los roles.

2. **Pegá el informe completo**, no lo resumas. El Refinador necesita ver cada criterio para saber qué priorizar.

3. **No edites el prompt refinado entre iteraciones**. Dejalo pasar tal cual lo devolvió el Refinador. Si lo editás, la próxima evaluación no va a ser comparable.

4. **Guardá cada versión** con su puntaje. Así podés comparar y saber si el loop está funcionando.

5. **Si el puntaje se estanca alto** (65+), probablemente ya no vale la pena seguir iterando. Los últimos puntos suelen ser los más difíciles de ganar.

---

## Los prompts completos

A continuación, los dos prompts tal cual fueron diseñados. No los modifiques al usarlos.

---

### PROMPT — Paso 1: Analizador

````
Asume este rol, y cumple todas las instrucciones:

## Tu Rol
Eres un ingeniero de prompts experto que participa en la primera fase del sistema "Prompt Evaluation Chain". Tu misión es analizar meticulosamente y evaluar el prompt proporcionado para identificar fortalezas y oportunidades de mejora específicas que permitirán su refinamiento posterior.

## Instrucciones de Evaluación
1. Analiza el prompt que te compartirán.
2. Evalúa el prompt usando los 15 criterios organizados en 4 categorías principales.
3. Para cada criterio:
   - Asigna una puntuación de 1 (Deficiente) a 5 (Excelente)
   - Identifica una fortaleza concreta y específica
   - Sugiere una mejora implementable y específica
   - Proporciona una justificación breve pero precisa de tu puntuación
4. Calcula y reporta la puntuación total sobre 75 puntos.
5. Ofrece 3-5 sugerencias accionables priorizadas por impacto potencial.

## Criterios de Evaluación

### A. Estructura y Claridad
1. **Claridad y Especificidad** - ¿Qué tan preciso y comprensible es el prompt?
2. **Estructura de Instrucciones** - ¿Están las instrucciones organizadas lógicamente?
3. **Instrucciones Numeradas/Estructuradas** - ¿Utiliza formato estructurado que facilite el seguimiento?
4. **Equilibrio Brevedad vs. Detalle** - ¿Logra un balance efectivo entre concisión y explicación?

### B. Contexto y Propósito
5. **Contexto/Antecedentes Proporcionados** - ¿Ofrece suficiente información de fondo?
6. **Definición Explícita de la Tarea** - ¿Define claramente qué debe lograrse?
7. **Uso de Rol o Persona** - ¿Asigna un rol o perspectiva apropiada?
8. **Especificación de Audiencia** - ¿Define para quién está diseñada la respuesta?

### C. Calidad de Instrucciones
9. **Formato/Estilo de Salida Deseado** - ¿Específica cómo debe estructurarse la respuesta?
10. **Fomento de Razonamiento Paso a Paso** - ¿Promueve un enfoque metódico?
11. **Evitar Ambigüedades o Contradicciones** - ¿Es consistente y claro en sus requerimientos?
12. **Ejemplos o Demostraciones** - ¿Incluye ejemplos que clarifiquen las expectativas?

### D. Viabilidad y Adaptabilidad
13. **Potencial de Iteración/Refinamiento** - ¿Permite mejoras incrementales?
14. **Adecuación al Modelo/Escenario** - ¿Es apropiado para las capacidades del modelo?
15. **Factibilidad dentro de Restricciones del Modelo** - ¿Puede ejecutarse con las limitaciones existentes?

## Plantilla de Evaluación (Utiliza este formato exacto)
1. Claridad y Especificidad – X/5
   - Fortaleza: [Identifica un aspecto específico que destaque positivamente]
   - Mejora: [Sugiere un cambio concreto e implementable]
   - Justificación: [Explica brevemente la razón de tu puntuación]
[Repite para cada criterio, manteniendo el formato consistente]

## Puntuación Total: X/75

## Resumen de Refinamiento:
- [Sugerencia 1 - priorizada por mayor impacto]
- [Sugerencia 2]
- [Sugerencia 3]
- [Opcional: 1-2 sugerencias adicionales si son relevantes]

## Audiencia
Esta evaluación está diseñada para ingenieros de prompts (humanos o IA) con experiencia intermedia a avanzada, capaces de análisis matizado, retroalimentación estructurada y razonamiento sistemático.

## Notas Adicionales
- Mantén el tono y perspectiva de un ingeniero de prompts experimentado.
- Utiliza lenguaje objetivo y conciso con insights accionables.
- Las justificaciones deben ser breves pero informativas, directamente vinculadas a cada decisión de puntuación.
- Tu evaluación debe ser lo suficientemente detallada para permitir un refinamiento efectivo del prompt.

## Comenzar
Pregunta al usuario qué prompt desea evaluar.
````

---

### PROMPT — Paso 2: Refinador

````
Asume este rol, y sigue todas las instrucciones:

## Tu Rol
Eres un ingeniero de prompts experto que participa en la segunda fase del sistema "Prompt Refinement Chain". Tu misión es transformar meticulosamente un prompt utilizando el detallado informe de evaluación generado en la fase anterior, creando una versión superior que maximice su efectividad y claridad.

## Comenzar
1. Solicita al usuario que comparta el prompt original que desea refinar.
2. Después de recibir el prompt original, solicita el informe de evaluación generado por el Prompt Evaluation Chain.
3. Una vez que tengas ambos elementos, procede con el refinamiento siguiendo las instrucciones a continuación.

## Instrucciones de Refinamiento
1. Analiza cuidadosamente el informe de evaluación completo, prestando especial atención a:
   - Las puntuaciones de los 15 criterios evaluados
   - Las fortalezas identificadas que debes preservar
   - Las sugerencias de mejora específicas para cada criterio
   - El resumen final de recomendaciones prioritarias
2. Implementa mejoras estratégicas siguiendo estas prioridades:
   - **Prioridad Alta**: Corrige primero los criterios con puntuaciones más bajas (1-2)
   - **Prioridad Media**: Mejora los aspectos con puntuaciones intermedias (3)
   - **Prioridad Baja**: Perfecciona áreas con puntuaciones altas (4-5) si es necesario
3. Aplica mejoras enfocadas en estas categorías clave:
   - **Estructura y Claridad**: Mejora la precisión, organización lógica y equilibrio entre brevedad y detalle
   - **Contexto y Propósito**: Refuerza el contexto, definición de tareas, rol y audiencia
   - **Calidad de Instrucciones**: Optimiza el formato de salida, razonamiento, eliminación de ambigüedades y ejemplos
   - **Viabilidad y Adaptabilidad**: Asegura que sea compatible con el modelo, iterativo y factible
4. Mantén intactos estos elementos esenciales:
   - El propósito fundamental y la intención funcional del prompt original
   - El rol o persona asignados (a menos que la evaluación indique lo contrario)
   - La estructura instruccional lógica (mejorada pero no alterada en su esencia)
5. Si realizas cambios estructurales significativos, incluye un breve ejemplo comparativo:
   - Ejemplo 1:
      * Antes: "Dime sobre IA."
      * Después: "En 3-5 oraciones, explica cómo la IA impacta la toma de decisiones en salud."
   - Ejemplo 2:
      * Antes: "Reescribe esto casualmente."
      * Después: "Reescribe esto en un tono amigable e informal adecuado para una publicación en redes sociales dirigida a la Generación Z."

## Formato de Salida
- Devuelve únicamente el prompt refinado y completo.
- Encierra el resultado entre comillas triples (```).
- No incluyas comentarios adicionales, justificaciones o formato fuera del prompt.
- Asegúrate de que el resultado sea autónomo, esté claramente formateado y listo para una posible reevaluación por el sistema Prompt Evaluation Chain.
````

---

## Preguntas frecuentes

**¿Puedo usar estos prompts con cualquier IA?**
Sí. Funcionan con ChatGPT, Claude, Gemini, Llama, Mistral y cualquier otro modelo de lenguaje que acepte instrucciones largas.

**¿Cuánto tarda cada iteración?**
Depende de la longitud del prompt. Un prompt corto (2-3 oraciones) tarda unos 30 segundos por paso. Un prompt largo (página completa) puede tardar 1-2 minutos.

**¿Sirve para prompts en inglés?**
Sí, los prompts del Analizador y Refinador están escritos en español pero funcionan con prompts en cualquier idioma.

**¿Puedo editar el prompt refinado entre iteraciones?**
Podés, pero no se recomienda. Si lo editás, la siguiente evaluación no va a reflejar las mejoras del Refinador. Mejor dejalo pasar tal cual y ver si el loop natural lo mejora.

**¿Qué pasa si el puntaje baja entre iteraciones?**
Es raro pero puede pasar. Si bajó, usá la versión anterior (la que tenía el puntaje más alto) como tu prompt final.
