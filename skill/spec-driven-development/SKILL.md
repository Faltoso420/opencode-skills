---
name: spec-driven-development
description: Desarrollo dirigido por especificaciones (SDD). Use when starting a new project, a feature, a significant change or a refactor, or when the user says "SDD", "spec-driven", "especificación", "planificar el proyecto", "nueva feature". Define constitución → spec → plan → tasks → implementación tarea a tarea con tests y trazabilidad.
---

# SDD — Spec-Driven Development

El código es el **cómo**; la spec es el **qué** y el **porqué**. Ninguna línea de código antes de que exista una spec aprobada.

## Artefactos

```
AGENTS.md (o CLAUDE.md)
docs/constitution.md
specs/NNN-<feature>/
  spec.md
  plan.md
  tasks.md
```

## Reparto de agentes en opencode

- Fases 0–3 (constitución, spec, plan, tasks): agente **`plan`** (no escribe código).
- Fase 4 (implementación): agente **`build`**.

## Fase 0 — Constitución

Crea `docs/constitution.md` con **6 principios innegociables, cortos y verificables**:

1. Simplicidad del stack.
2. La spec manda: si código y spec difieren, se corrige uno de los dos; nunca se ignora.
3. Lógica separada de la interfaz.
4. Política de tests (TDD, qué se prueba, umbral).
5. Persistencia de datos (dónde y cómo).
6. Idioma y convenciones.

Toda spec, plan y tarea debe cumplirla. Referénciala desde `AGENTS.md`.

## Fase 1 — Spec

Prompt base:

> Lee `docs/constitution.md`. No escribas código. Idea: <idea>.
> Hazme preguntas de una en una para eliminar ambigüedades, casos límite y errores fuera del MVP. Máximo 6 preguntas.
> Con mis respuestas genera `specs/NNN-<feature>/spec.md`.

Contenido obligatorio:

- Contexto y objetivo.
- Usuarios e historias de usuario.
- Requisitos funcionales (RF) con **criterios de aceptación en notación EARS**.
- Requisitos no funcionales.
- Casos límite y comportamiento ante error.
- Criterios de finalización.

**No** incluye stack, arquitectura ni nombres: eso es el plan.

**Clarificación** (repetir hasta que esté bien):

> Revisa la spec como un QA muy profesional. Lista ambigüedades, contradicciones, casos límite no cubiertos y conflictos con la constitución. No propongas soluciones todavía. Formato: lista numerada.

## Fase 2 — Plan

> Lee la constitución y `spec.md`. No escribas código.
> Genera `plan.md` con: estructura de módulos, modelo de datos, algoritmo, comandos y salidas, decisiones técnicas y estrategia de test.

Clarifica el plan igual que la spec.

## Fase 3 — Tasks

> A partir de `spec.md` y `plan.md`, genera `tasks.md`. Tareas pequeñas (máximo 20-30 minutos), en orden de dependencia, cada una con el RF que cubre y una línea de "hecho verificable". Usa checkboxes.

Cada tarea referencia su RF → trazabilidad.

## Fase 4 — Implementación (una tarea a la vez)

> Implementa solo la tarea T2 de `tasks.md`, siguiendo `plan.md` y la constitución.
> Escribe primero los tests, luego el código.
> Ejecuta los tests y muéstrame el resultado.
> Al terminar, marca T2 en `tasks.md`, indica qué RF cubre y párate. No empieces T3.

Al cerrar cada tarea: tests en verde, `git diff` revisado y tasks actualizado. Si el comportamiento cambió respecto a la spec, actualiza la spec.

## Proyectos existentes

No reescribas todo. Trata el cambio como una spec nueva:

- Bugfix pequeño → mini-spec + tasks (sin plan).
- Feature o refactor → spec + plan + tasks.
- Antes de tocar código, lee `AGENTS.md`, la constitución si existe y la spec afectada.

## Reglas

- Ningún código antes de spec aprobada.
- Usa modo plan en todo menos la implementación.
- Para la spec, usa el modelo más potente disponible: sin buena spec no hay buen código.
- Una spec por funcionalidad; numera `001`, `002`, `003`…
- Si el código contradice la spec, para y decide: actualizar spec o corregir código.
- TDD: test primero, implementación después.
- Trazabilidad RF ↔ tarea ↔ test.
- La IA propone; tú apruebas en cada puerta (constitución, spec, plan, tasks).

## Checklist de cierre

- [ ] `docs/constitution.md` existe y se cumple.
- [ ] `spec.md` con RF, criterios EARS y casos límite.
- [ ] `plan.md` con módulos, datos, algoritmo y estrategia de test.
- [ ] `tasks.md` con tareas ≤30 min y checkboxes.
- [ ] Cada tarea referencia un RF.
- [ ] Tests escritos y ejecutados antes de marcar una tarea como hecha.
- [ ] Spec y tasks actualizadas al terminar.

## Notación EARS (resumen)

- Ubicuo: "El sistema debe <respuesta>".
- Por evento: "Cuando <evento>, el sistema debe <respuesta>".
- Por estado: "Mientras <estado>, el sistema debe <respuesta>".
- Condicional: "Si <condición>, el sistema debe <respuesta>".
- No deseado: "Si <evento no deseado>, el sistema debe <respuesta>".

## Fuente

Video: "El fin del Vibe Coding: Crea software robusto con este método" — MoureDev (2:03:16).
Timestamps: constitución 1:03–1:08 · spec 1:10–1:26 · plan 1:26–1:30 · tasks 1:30–1:36 · implementación 1:37–1:39.
