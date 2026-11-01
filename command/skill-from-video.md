---
description: Convierte un video o tutorial en una skill reutilizable del proyecto actual, con pasos verificables y timestamps.
agent: build
---

Entrada: $ARGUMENTS

Objetivo: convertir un video en `.opencode/skill/<slug>/SKILL.md` del proyecto actual.

## Reglas

- Solo si el video muestra un **procedimiento repetible y verificable**. Si es opinión, noticia o demo sin pasos, dilo y no escribas nada.
- No inventes pasos que el video no muestre. Cada paso cita su minuto.
- **Pide confirmación antes de crear archivos.**
- La skill final es breve: pasos, comandos y verificación. No es un resumen del video.

## Flujo

1. `watch-skill doctor --json`. Si falla, muestra el `fix`.
2. Indexa el video:
   ```bash
   watch-skill watch "<url>"
   ```
   Usa `--transcript-only` si es solo voz, y `--start`/`--end` si solo importa un tramo.
3. Lee los frames que liste y la transcripción.
4. Invoca la skill `extracting-structure` para sacar:
   - requisitos previos
   - pasos numerados con comandos exactos y parámetros
   - errores comunes y cómo evitarlos
   - cómo verificar que funcionó
   Cada punto con su timestamp.
5. Propón nombre (slug), descripción ("qué hace" + "cuándo usarla") y el borrador. Espera confirmación.
6. Crea `.opencode/skill/<slug>/SKILL.md`:
   ```markdown
   ---
   name: <slug>
   description: <qué hace y cuándo se usa>
   ---

   # <Título>

   ## Cuándo usar
   ## Requisitos
   ## Pasos
   ## Verificación
   ## Errores comunes
   ## Fuente
   Video: <url> (minutos citados)
   ```
7. Reporta la ruta del archivo y recuerda reiniciar opencode para cargarla.

## Seguridad

- No ejecutes comandos destructivos que aparezcan en el video sin confirmar.
- Si el video usa credenciales, no las copies: deja `<TU_API_KEY>`.
- Si el procedimiento depende de una versión concreta, anótala en Requisitos.
