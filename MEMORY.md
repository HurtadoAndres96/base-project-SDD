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
- `README.md` documenta instalación, entradas directa/multiagente, flujo, gates, agentes y estructura del starter.

## Decisiones vigentes (y por qué)

- **Base SDD:** Constitución 1.6; las reglas permanentes viven allí y en `AGENTS.md`, no se duplican en esta memoria.
- **Entrada portable:** `.agents/skills/sdd/SKILL.md` y sus runbooks en `references/` forman una unidad autocontenida; los adaptadores de Claude Code y OpenCode no duplican el flujo.
- **Interacción guiada:** `$sdd` o `/sdd` muestra solo acciones válidas según el gate actual y acepta números o lenguaje natural; `inicio` retoma una configuración pendiente y conduce la primera spec también en proyectos existentes.
- **Gate de modo:** Las etapas con efectos requieren Build/Default cuando el host distingue modos; un modo incompatible detiene el flujo antes del runbook y nunca se cambia automáticamente.
- **Coordinación portable:** El coordinador es de solo lectura, delega únicamente en `planner`, `implementer` y `reviewer`, y no duplica el flujo canónico de la skill SDD.
- **Planificación acotada:** El planner no toca código; escribe artefactos SDD y memoria, y solo durante `inicio` puede configurar `AGENTS.md` y la constitución.
- **Ejecución acotada:** El implementer ejecuta una sola tarea elegible, usa la verificación declarada y registra evidencia antes de cambiar estados.
- **Revisión independiente:** El reviewer no corrige código; en QA solo detecta y, al validar, escribe únicamente evidencia durable, resultado de la spec y memoria según el runbook.

## Aprendizajes y errores a evitar

- No inferir la aprobación de la configuración por ausencia de placeholders: `MEMORY.md` debe registrar la decisión explícita antes de crear la primera spec.

## Próximos pasos

- [ ] Ejecutar `$sdd` o `/sdd` y elegir la opción recomendada para completar los placeholders.
- [ ] Crear, revisar y aprobar la primera especificación.
