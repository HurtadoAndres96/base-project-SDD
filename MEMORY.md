# MEMORY.md — {{NOMBRE_PROYECTO}}

Memoria operativa entre sesiones. Máximo ~50 líneas; no contiene secretos ni reemplaza la spec.

## Contexto activo

- **Especificación activa:** Ninguna
- **Estado:** Configuración inicial
- **Plan activo:** Ninguno
- **Tareas activas:** Ninguna
- **Última validación:** Ninguna
- **Siguiente acción:** Abrir el menú guiado con `$sdd` (o `/sdd`) y completar la configuración.

## Estado actual

- Starter kit SDD preparado con una skill portable; todavía no existe una funcionalidad especificada.
- El proyecto aún debe definir stack, comandos, arquitectura y convenciones.
- `coordinator`, `planner`, `implementer` y `reviewer` están adaptados para Codex, OpenCode y Claude Code; no quedan placeholders de agentes.
- Repositorio Git inicializado en `main` y conectado a `git@github.com:HurtadoAndres96/base-project-SDD.git`.
- `README.md` documenta instalación, entrada SDD coordinada, flujo, gates, agentes y estructura del starter.

## Decisiones vigentes (y por qué)

- **Base SDD:** Constitución 1.8; las reglas permanentes viven allí y en `AGENTS.md`, no se duplican en esta memoria.
- **Entrada portable:** `.agents/skills/sdd/SKILL.md` y sus runbooks en `references/` forman una unidad autocontenida; los adaptadores de Claude Code y OpenCode no duplican el flujo.
- **Claude Code:** Se retiró el puente `CLAUDE.md` porque 2.1.277+ puede leer `AGENTS.md` directamente sin él; `README.md` explica las condiciones y el respaldo para entornos sin soporte.
- **Interacción guiada:** `$sdd` o `/sdd` pasa siempre por `coordinator`, muestra solo acciones válidas y acepta números o lenguaje natural; `inicio` retoma una configuración pendiente y conduce la primera spec también en proyectos existentes.
- **Gate de modo:** Las etapas con efectos requieren Build/Default cuando el host distingue modos; un modo incompatible detiene el flujo antes del runbook y nunca se cambia automáticamente.
- **Coordinación portable:** El coordinador es de solo lectura, delega únicamente en `planner`, `implementer` y `reviewer`, y no duplica el flujo canónico de la skill SDD.
- **Planificación acotada:** El planner no toca código; escribe artefactos SDD y memoria, y solo durante `inicio` puede configurar `AGENTS.md` y la constitución.
- **Ejecución acotada:** El implementer ejecuta una sola tarea elegible, usa la verificación declarada y registra evidencia antes de cambiar estados.
- **Tareas extensas:** `$sdd tareas` modulariza al superar 15 tareas o 300 líneas monolíticas (umbrales ajustables en `AGENTS.md`), conserva IDs/evidencia y pide aprobar el conjunto. `ejecutar` solo avisa y elige desde el índice vigente.
- **Revisión independiente:** El reviewer no corrige código; en QA solo detecta y, al validar, escribe únicamente evidencia durable, resultado de la spec y memoria según el runbook.

## Aprendizajes y errores a evitar

- No inferir la aprobación de la configuración por ausencia de placeholders: `MEMORY.md` debe registrar la decisión explícita antes de crear la primera spec.
- Constitución 1.7 aprobada para permitir tareas modulares sin duplicar estado; no hay spec, plan ni tareas activas que reconciliar en esta base.
- Constitución 1.8 aprobada para modularizar por umbral en `tareas`; no hay spec, plan ni tareas activas que reconciliar en esta base.

## Próximos pasos

- [ ] Ejecutar `$sdd` o `/sdd` y elegir la opción recomendada para completar los placeholders.
- [ ] Crear, revisar y aprobar la primera especificación.
