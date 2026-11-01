# opencode-skills

Skills, agente y comandos para [opencode](https://opencode.ai), creados y probados en uso real. Funcionan en cualquier proyecto y con el modelo que tengas configurado.

## Contenido

| Ruta | Tipo | Para qué |
|---|---|---|
| `agent/prompt-engineer.md` | Agente | Convierte ideas en especificaciones y prompts de nivel experto |
| `skill/professional-prompts/SKILL.md` | Skill | Decálogo, frameworks (RTF, COSTAR, CRISPE, RISEN), plantillas y rúbrica |
| `skill/github-repos/SKILL.md` | Skill | Clonar, crear, subir, pull/push, ramas, commits, PRs e issues con `gh` |
| `skill/spec-driven-development/SKILL.md` | Skill | SDD: constitución → spec → plan → tasks → implementación con TDD |
| `command/prompt.md` | Comando | `/prompt` — activa el ingeniero de prompts |
| `command/watch.md` | Comando | `/watch` — ve un video y responde con timestamps |
| `command/skill-from-video.md` | Comando | `/skill-from-video` — un tutorial → una skill del proyecto |
| `command/skills-from-playlist.md` | Comando | `/skills-from-playlist` — varios tutoriales → varias skills |

## Instalación

### Global (todos los proyectos)

Copia el contenido en tu config de opencode:

```powershell
# Windows
Copy-Item -Recurse .\agent   "$env:USERPROFILE\.config\opencode\"
Copy-Item -Recurse .\skill   "$env:USERPROFILE\.config\opencode\"
Copy-Item -Recurse .\command "$env:USERPROFILE\.config\opencode\"
```

```bash
# macOS / Linux
cp -R agent skill command ~/.config/opencode/
```

Reinicia opencode.

### Por proyecto

Copia `agent/`, `skill/` y `command/` dentro de `.opencode/` en la raíz del proyecto.

## Requisitos por skill

- **prompt-engineer / professional-prompts**: ninguno.
- **github-repos**: `git` y `gh` autenticado (`gh auth login`).
- **spec-driven-development**: ninguno. Usa el agente `plan` para las fases de diseño y `build` para implementar.
- **watch / skill-from-video / skills-from-playlist**: el CLI [`watch-skill`](https://github.com/oxbshw/watch-skill) (`pipx install watch-skill`), `ffmpeg` y `yt-dlp`. Una API key de visión es opcional.

## Uso

```
/prompt quiero una app para registrar hábitos
/watch https://youtu.be/... resumen con timestamps
/skill-from-video https://www.youtube.com/watch?v=...
/skills-from-playlist https://www.youtube.com/playlist?list=...
```

Para SDD, pide directamente "usa spec-driven-development" o empieza un proyecto/feature y la skill se activa.

## Notas

- El agente `prompt-engineer` carga `professional-prompts` como knowledge y hace intake dirigido.
- `watch` no genera skills por sí solo: observa y produce evidencia; los comandos de video hacen la autoría.
- Las skills de terceros (Superpowers, Ponytail, Impeccable, Strix, Watch Skill) no se incluyen aquí; instálalas desde sus repos originales.

## Licencia

MIT.
