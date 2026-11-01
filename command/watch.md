---
description: Ve un video (URL o archivo), extrae frames/OCR/transcripción y responde con timestamps.
agent: build
---

Entrada: $ARGUMENTS

Invoca la skill `watch` y sigue su flujo completo:

1. `watch-skill doctor --json`.
2. `watch-skill watch "<entrada>"`.
3. Lee los frames que liste el reporte.
4. Responde usando frames + OCR + transcripción, citando `t=MM:SS`.

Si la entrada está vacía, pide la URL o la ruta del video y no ejecutes nada más.
