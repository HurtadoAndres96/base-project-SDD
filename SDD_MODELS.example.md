# Política opcional de modelos SDD

Esta política solo se activa si el archivo se llama exactamente
`SDD_MODELS.md` y está en la raíz del proyecto. Con el nombre
`SDD_MODELS.example.md` es un ejemplo inactivo. Ajusta los modelos a los
disponibles en tu host; sin el archivo activo, SDD mantiene la selección actual.

La política solo afecta la delegación a `planner`, `implementer` y `reviewer`;
no cambia permisos, gates, requisitos ni aprobaciones.

## Perfiles

| Perfil | Elegir cuando |
|---|---|
| Alto | Hay ambigüedad relevante, decisiones de arquitectura, seguridad, privacidad, datos, migraciones o un fallo difícil de diagnosticar. |
| Equilibrado | Hay varias piezas conectadas y decisiones técnicas ya delimitadas, pero aún se necesita juicio. |
| Ligero | La tarea es pequeña, aislada, aprobada, verificable y no presenta riesgos especiales. |

El `coordinator` elige el perfil por el encargo concreto, no por el nombre del
agente. Si aparecen riesgos o contradicciones, detiene la etapa y comunica la
necesidad de revisar el plan o usar un perfil más capaz en un intento posterior.

## Mapeo por host

Estos modelos Codex son solo un ejemplo: comprueba su disponibilidad antes de
activar el archivo. Cambia o añade filas para Claude Code y OpenCode si el host
permite elegir modelo por delegación. `heredar` conserva la selección existente.

| Host | Perfil | Modelo | Esfuerzo |
|---|---|---|---|
| Codex | Alto | `gpt-6-astra` | `high` |
| Codex | Equilibrado | `gpt-6-sol` | `medium` |
| Codex | Ligero | `gpt-6-luna` | `medium` |
| Claude Code | Alto | heredar | heredar |
| Claude Code | Equilibrado | heredar | heredar |
| Claude Code | Ligero | heredar | heredar |
| OpenCode | Alto | heredar | heredar |
| OpenCode | Equilibrado | heredar | heredar |
| OpenCode | Ligero | heredar | heredar |

Si una fila no existe, el modelo indicado no está disponible o la herramienta
no permite elegirlo, el `coordinator` mantiene la herencia y lo informa. El
modelo fijado en la configuración de un agente puede tener prioridad sobre este
archivo; en ese caso tampoco se fuerza ni se declara aplicado el cambio. El
objetivo es reducir el coste de completar bien la tarea, incluidos reintentos,
no solo bajar el precio de una llamada aislada.
