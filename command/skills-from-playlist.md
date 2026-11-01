---
description: Convierte una playlist o lista de tutoriales de YouTube en skills del proyecto actual, con confirmación por video.
agent: build
---

Entrada: $ARGUMENTS

Objetivo: convertir tutoriales en skills reutilizables dentro de `.opencode/skill/<slug>/SKILL.md` del proyecto actual.

## Reglas

- No proceses más de **3 videos** sin que el usuario lo pida explícitamente.
- Pide confirmación por cada skill antes de escribirla.
- No inventes pasos: cada paso cita su minuto del video.
- Si un video no muestra un procedimiento repetible y verificable, descártalo y dilo.
- Si la entrada es una playlist privada o "Guardados", explica que `watch` no inicia sesión: usa URLs públicas o pega los enlaces uno a uno.

## Flujo

1. Si la entrada es una playlist, lista los videos:
   ```powershell
   & "$env:USERPROFILE\.watch-skill\bin\yt-dlp.exe" --flat-playlist --print "%(webpage_url)s`t%(title)s" "<url>"
   ```
   Si son URLs sueltas, úsalas tal cual.
2. Muestra la lista y pregunta cuáles procesar (por defecto, los 3 primeros).
3. Para cada video confirmado:
   - `watch-skill doctor --json`
   - `watch-skill watch "<url>"` (usa `--transcript-only` si es solo narración)
   - lee frames + OCR + transcripción
   - invoca `extracting-structure` para sacar requisitos, pasos, verificación y errores, con timestamps
   - propón nombre (slug), descripción y borrador
   - espera confirmación y escribe `.opencode/skill/<slug>/SKILL.md`
4. Al final, entrega una tabla: skill, qué resuelve, ruta y minutos citados. Recuerda reiniciar opencode.

## Formato de la skill generada

```markdown
---
name: <slug>
description: <qué hace y cuándo usarla>
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

## Seguridad

- No ejecutes comandos destructivos del video sin confirmar.
- Credenciales del video → `<TU_API_KEY>`, nunca la clave real.
