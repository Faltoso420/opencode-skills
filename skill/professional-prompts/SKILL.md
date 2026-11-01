---
name: professional-prompts
description: Metodología para escribir prompts profesionales. Use when creating, auditing, or improving a prompt for any LLM or image model, when choosing a prompt framework (RTF, TAG, COSTAR, CRISPE, RISEN), when standardizing system prompts, or when a vague request must become a precise, testable instruction.
---

# Prompts profesionales

Un prompt es una especificación, no una conversación. Si no define rol, contexto, tarea, restricciones, formato y criterio de éxito, el modelo rellena los huecos con promedios.

## 0. Decálogo del buen prompt (gate obligatorio)

Aplica a todo prompt que entregues. Si un punto no aplica al caso, justifícalo; no lo ignores.

1. **Empieza por el entregable, no por el verbo.** Antes de redactar responde: qué quiero obtener exactamente, para qué se usará ese resultado y cómo sabré que está bien hecho. Un buen prompt empieza por el resultado, no por "hazme", "analiza" o "escribe".
2. **Da contexto suficiente.** El modelo no adivina tu situación: datos relevantes, público objetivo, restricciones del negocio o proyecto, qué se intentó antes y nivel de profundidad deseado. Regla: contexto útil, no relleno.
3. **Asigna rol o perspectiva.** "Actúa como editor técnico especializado en…", "piensa como revisor de calidad de…", "responde como un consultor que debe justificar decisiones…". No es obligatorio, pero fija marco, precisión y tono.
4. **Formula la tarea con verbos concretos.** Sustituye "mejora", "analiza" u "optimiza" sin criterio por operaciones medibles: "reescribe para reducir 30 % la longitud sin perder…", "identifica los 5 riesgos principales y clasifícalos por impacto/probabilidad", "genera 3 alternativas y recomienda una con justificación".
5. **Especifica el formato de salida.** Estructura exacta: tabla con columnas, lista numerada, JSON con esquema, Markdown con secciones, máximo de palabras o caracteres, idioma y tono. Esto reduce drásticamente la variabilidad.
6. **Establece restricciones y criterios de éxito.** Di qué sí, qué no y cuándo está bien: "no inventes fuentes; si falta información, dilo", "máximo 200 palabras", "incluye supuestos explícitos", "cada recomendación debe tener impacto, esfuerzo y riesgo", "no uses jerga técnica con audiencia no técnica". Los criterios de éxito son la base para evaluar la respuesta.
7. **Usa ejemplos cuando el formato importe.** Un ejemplo de entrada/salida vale más que diez adjetivos: estilo de redacción, estructura de documentos, clasificaciones, extracción de datos. Si no hay ejemplo completo, da una fila o sección mínima.
8. **Delimita claramente las partes.** Separa instrucciones, contexto, datos y ejemplos con encabezados o delimitadores (`### Instrucción`, `### Contexto`, `### Datos`, `### Formato de salida`). Evita que el modelo confunda tus datos con órdenes.
9. **Pide razonamiento estructurado, no "cadena de pensamiento" privada.** En lugar de "piensa paso a paso" sin más, pide: supuestos, criterios de decisión, comparación de alternativas, justificación breve de la recomendación, nivel de confianza y vacíos de información. Produce respuestas auditables sin exigir razonamiento interno no verificable.
10. **Itera y versiona.** Versión base → probar con 2-3 casos distintos → detectar fallos de formato, omisiones, invenciones o tono → añadir una restricción o ejemplo por fallo → guardar la versión que funciona como plantilla reutilizable.

**Regla de entrega:** si faltan los puntos 1, 4, 5, 6 u 8, el prompt no está listo. Los demás se añaden cuando aporten.

## 1. Intake

Responde antes de escribir. Si falta algo crítico, pregunta; si no, asume y marca el supuesto.

| Dimensión | Pregunta |
|---|---|
| Objetivo | ¿Qué resultado concreto debe existir al final? |
| Modelo | ¿A qué modelo va dirigido y con qué ventana/contexto? |
| Audiencia | ¿Quién consume la salida y con qué nivel? |
| Formato | ¿Markdown, JSON, tabla, código, longitud máxima? |
| Restricciones | Qué prohibir, qué fuentes usar, tono, idioma |
| Éxito | ¿Cómo se verifica que la salida es buena? |

### 1.1 Cuándo interrogar

Pregunta solo si faltan **dos o más** dimensiones críticas: entregable, uso/audiencia, modelo destino, formato. Si falta una sola, asume y marca el supuesto. Si la descripción ya cubre entregable, uso y formato, no preguntes.

### 1.2 Cómo interrogar

- Máximo 5 preguntas, todas en un solo turno.
- Cada pregunta con opciones concretas y respuesta libre.
- Máximo 2 rondas; después asume, marca `[SUPUESTO: ...]` y entrega.
- No preguntes lo ya dicho ni detalles que no cambien el prompt.
- Ordena por impacto: entregable → uso → formato → modelo → límites.
- Usa la herramienta `question` si está disponible; si no, preguntas numeradas.

## 2. Anatomía

Todo prompt robusto tiene siete bloques; omite solo los que no aporten.

1. **Rol** — función operativa, no adjetivos: "actúas como revisor de contratos que detecta cláusulas de riesgo", no "eres un experto brillante".
2. **Contexto** — datos, audiencia, estado previo, glosario.
3. **Tarea** — verbo único y medible. Una tarea por prompt.
4. **Restricciones** — límites duros: longitud, fuentes, tono, qué no hacer.
5. **Formato de salida** — esquema, ejemplo o plantilla exacta.
6. **Ejemplos** (few-shot) — solo si el formato o el criterio son difíciles de verbalizar.
7. **Criterios de éxito / autoverificación** — checklist que el modelo aplica antes de responder.

## 3. Frameworks

| Framework | Bloques | Úsalo para |
|---|---|---|
| **RTF** | Role, Task, Format | Tareas simples y rápidas |
| **TAG** | Task, Action, Goal | Automatizaciones y agentes |
| **COSTAR** | Context, Objective, Style, Tone, Audience, Response | Redacción y contenido |
| **CRISPE** | Capacity, Role, Insight, Statement, Personality, Experiment | Exploración creativa y variantes |
| **RISEN** | Role, Instructions, Steps, End goal, Narrowing | Tareas largas y multi-paso |

Técnicas transversales:

- **Few-shot**: 2-5 ejemplos contrastivos (uno correcto, uno incorrecto) cuando el criterio es sutil.
- **Chain-of-thought**: pedir razonamiento paso a paso solo para análisis; en modelos modernos, pedir "criterios y verificación" suele superar a "piensa paso a paso".
- **Decomposición**: dividir en subprompts con contrato de entrada/salida cuando la tarea excede una sola pasada.
- **Salida estructurada**: esquema JSON explícito con tipos y campos obligatorios; nunca "devuelve JSON" a secas.
- **Roles separados**: generador y crítico como prompts distintos mejora calidad frente a pedir "hazlo bien".

Guía rápida por tipo de tarea:

- Redacción → COSTAR + ejemplos de tono.
- Análisis → RISEN + criterios de evidencia y formato de conclusión.
- Extracción → esquema JSON + reglas de campos ausentes (`null`, no inventar).
- Código → restricciones de stack, contratos de función, tests esperados.
- Imagen → sujeto, estilo, composición, iluminación, lente, aspect ratio, negativos.
- Agente con tools → objetivo, tools permitidas, política de cuándo usarlas, condición de parada, formato de traza.

## 4. Plantillas

**Sistema (asistente):**

```
Rol: [función operativa].
Contexto: [dominio, datos disponibles, límites].
Tarea: [verbo medible] para [audiencia].
Restricciones:
- [límite duro]
- Si falta información, [comportamiento explícito].
Formato: [esquema o plantilla exacta].
Verificación: antes de responder, comprueba [checklist].
```

**Extracción estructurada:**

```
Extrae [entidades] del texto.
Devuelve SOLO JSON válido con este esquema:
{"campo": {"tipo": "string|null", "regla": "..."}}
Reglas:
- Campo ausente → null. No inferir.
- Fechas → ISO 8601.
- Si el texto es ambiguo, añade "confianza": 0-1 por campo.
```

**Imagen:**

```
[SUJETO] en [ENTORNO], [ESTILO], [COMPOSICIÓN/ÁNGULO], [ILUMINACIÓN],
[PALETA], [LENTE/TEXTURA], [ASPECT RATIO].
Negativos: [elementos a evitar].
```

## 5. Rúbrica (0-2 por criterio, alineada al decálogo)

1. Entregable definido: qué, para qué y cómo se verifica.
2. Contexto suficiente sin relleno.
3. Rol o perspectiva operativa (o ausencia justificada).
4. Tarea con verbos concretos y resultado medible.
5. Formato de salida exacto y verificable.
6. Restricciones y criterios de éxito explícitos.
7. Ejemplos útiles cuando el formato lo exige (o ausencia justificada).
8. Partes delimitadas: instrucción, contexto, datos, formato.
9. Razonamiento estructurado: supuestos, criterios, alternativas, confianza.
10. Reutilizable: variables claras, sin depender de un proveedor si no hace falta.

Menos de 16/20 → reescribir antes de entregar.

## 6. Anti-patrones

- Adjetivos sin función ("experto", "brillante", "detallado").
- Varias tareas mezcladas en un prompt.
- "Sé creativo" o "hazlo bien" como única guía.
- Formato mencionado pero no especificado.
- Ejemplos que contradicen las reglas.
- Instrucciones que el modelo no puede verificar.
- Pedir JSON sin esquema ni ejemplo.
- Sobrecargar de contexto irrelevante ("lost in the middle").

## 7. Ciclo de refinamiento

1. Escribir con la anatomía completa.
2. Puntuar con la rúbrica.
3. Identificar el criterio más bajo y corregirlo.
4. Probar con un caso normal y uno límite.
5. Versionar: guarda el prompt con fecha y un caso de prueba.

## 8. De idea a especificación técnica

Aplica cuando la idea es una app, programa, automatización, script o tarea operativa. El objetivo no es describirla mejor: es volverla **ejecutable**.

### 8.1 Mejorar la idea antes de escribirla

Detecta y corrige:

- **Contradicción**: pide dos resultados incompatibles o un flujo que se cierra sobre sí mismo.
- **Alcance inflado**: más de 5 capacidades para una v1. Mueve el resto a "futuro".
- **Solución sin problema**: propone una app cuando el problema se resuelve con una hoja de cálculo o un script de 20 líneas.
- **Requisito sin criterio**: "que sea rápido", "que sea seguro", "que funcione". Traducir a umbral verificable.
- **Dependencia oculta**: credenciales, API de pago, scraping, datos que el usuario no tiene, permisos del sistema.

### 8.2 Brief canónico

```
OBJETIVO:       [resultado observable en una frase]
USUARIO:        [quién lo usa y en qué momento]
PROBLEMA HOY:   [cómo se hace actualmente y qué duele]
ALCANCE v1:     [3-5 capacidades]
NO-OBJETIVOS:   [lo que explícitamente no se hará]
ENTRADAS:       [datos, formato, origen]
SALIDAS:        [formato exacto, destino]
FLUJO:          [pasos numerados, extremo a extremo]
STACK:          [lenguaje/framework o "decisión abierta"]
INTEGRACIONES:  [APIs, archivos, servicios]
CASOS LÍMITE:   [vacío, inválido, duplicado, sin permisos, tamaño, sin red]
ACEPTACIÓN:     [criterio verificable por capacidad]
RIESGOS:        [supuestos y dependencias]
```

### 8.3 Checklist técnico

- Un objetivo, no una lista de deseos.
- No-objetivos explícitos (evita *scope creep*).
- Cada capacidad con criterio de aceptación comprobable en una frase.
- Manejo de error definido antes de codificar.
- Permisos, credenciales y límites declarados.
- Una sola fuente de verdad para el flujo.

### 8.4 Prompt para agente de código

```
Rol: implementas [producto] en [stack o "el que propongas"].
Contexto: [usuario, problema, estado actual].
Objetivo: [resultado observable].
Alcance v1:
1. [capacidad] — aceptación: [criterio]
2. ...
No-objetivos: [lista].
Contratos:
- Entrada: [forma exacta]
- Salida: [forma exacta]
- Errores: [qué devolver ante cada caso límite]
Proceso:
1. Antes de escribir código, resume el plan y lista supuestos.
2. Implementa en fases; al final de cada fase indica cómo probarla.
3. No añadas dependencias sin justificarlas.
Entrega: estructura de archivos, código completo y comandos para ejecutar.
```

### 8.5 Anti-patrones técnicos

- Mezclar requisitos con instrucciones de implementación.
- Empezar por la tecnología ("hazme una app en React") sin definir el flujo.
- Pedir "todo integrado" en el primer prompt.
- Omitir qué pasa cuando la entrada es inválida.
- Aceptar una idea sin preguntar de dónde salen los datos.

## 9. Recomendaciones

Además del prompt o la especificación, entrega **2-5 recomendaciones accionables** sobre el propio prompt o el proceso. No son preguntas (eso son "decisiones abiertas") ni justificación (eso es "por qué funciona").

| Tipo | Qué recomendar |
|---|---|
| Enfoque | dividir en varios prompts, cambiar de framework, pasar de una pasada a un pipeline |
| Técnica | añadir few-shot, esquema JSON, ejemplos negativos, criterios de verificación |
| Programación | stack más simple, librería existente en vez de código propio, separar datos de lógica, contrato de entrada/salida, tests antes de implementar |
| Riesgo | qué puede fallar (alucinación, formato, permisos, límites de API) y cómo prevenirlo desde el prompt |
| Validación | 2-3 casos de prueba para comprobar la salida |

Reglas:

- Cada recomendación dice **qué cambiar, por qué y qué efecto esperar**.
- Máximo 5; ordenar por impacto.
- Concretas y verificables; nunca "mejora el prompt".
- En programación, marcar las sugerencias de stack como propuestas, no como requisitos.
- Si no hay nada relevante, dilo en una línea; no rellenes.

## 10. Skills recomendadas para el proyecto

Cuando el entregable sea una app, programa, automatización o tarea técnica, recomienda skills del ecosistema que ayuden a construirla.

1. Invoca la skill `find-skills` si está disponible.
2. Busca con 2-4 términos de dominio + tarea: `skills find nextjs auth`, `skills find python scraping`, `skills find ui audit`.
3. Filtra por calidad: instalaciones altas (preferible >1K), autor o repo reconocido, actividad reciente. Descarta desconocidos sin señales.
4. Recomienda 2-4: nombre, qué aporta, por qué encaja en este proyecto y comando `skills add <owner/repo@skill> -g`.
5. **No instales nada** sin que el usuario lo pida.
6. Si no hay coincidencias o el CLI no está disponible, dilo en una línea; no inventes nombres, autores ni métricas.
