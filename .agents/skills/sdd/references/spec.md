# Gestión de especificaciones

Vamos a gestionar una especificación. Actúa como Tech Lead y no escribas código ni plan técnico.

PREPARACIÓN
1. Lee `docs/constitution.md`, `AGENTS.md`, `MEMORY.md` y `specs/templates/spec.md` completos.
2. Determina el modo solicitado: `NUEVA`, `REVISIÓN`, `CAMBIO DE FOCO` o `CANCELACIÓN`. Una capacidad independiente o un cambio material del objetivo usa `NUEVA`; una corrección dentro del objetivo vigente usa `REVISIÓN`. Si no es inequívoco, esa debe ser tu primera y única pregunta.

MODO NUEVA
1. Inspecciona `specs/` y propone el siguiente número monotónico disponible con tres dígitos y un nombre `kebab-case` sin espacios; no reutilices números ni sobrescribas una spec existente.
2. Hazme una sola pregunta cada vez para definir objetivo, actores, alcance, historias, comportamiento, RNF, datos, riesgos, casos límite, exclusiones, supuestos y dependencias.
3. Cuando exista información suficiente, crea `specs/NNN-nombre-corto/spec.md` usando exactamente la plantilla y con estado `Borrador` y versión `0.1`.
4. Registra esa ruta relativa como “Especificación activa” en `MEMORY.md`, su estado `Borrador`, y reinicia “Plan activo”, “Tareas activas” y “Última validación” a `Ninguno`; no borres aprendizajes relevantes.

MODO REVISIÓN
1. Resuelve la spec exacta desde mi instrucción o `MEMORY.md`. Si no hay una activa o existe ambigüedad, pídeme seleccionarla. Lee completos sus `spec.md`, `plan.md`, `tasks.md` y `validation.md` cuando existan.
2. Distingue un cambio editorial, que no altera comportamiento ni criterios, de un cambio normativo. Si no es claro, pregúntame antes de modificar.
3. Para un cambio editorial, conserva estado y versión, modifica solo la redacción y registra el cambio en el historial.
4. Para un cambio normativo —incluida la reanudación de una spec `Cancelada`— incrementa la versión menor candidata, cambia la spec a `Borrador`, reabre criterios, deja su resultado de validación `Pendiente` para la nueva versión y conserva la evidencia histórica.
5. No renumeres ni reutilices IDs aprobados. Conserva cada elemento retirado como `[RETIRADO en vX.Y]`, con motivo y reemplazo si existe.
6. Marca plan y tareas anteriores `Obsoletos`, conserva `validation.md` como evidencia histórica y actualiza `MEMORY.md` para que no los presente como vigentes; no borres ni sobrescribas artefactos previos.
7. Hazme una sola pregunta cada vez para precisar únicamente el cambio solicitado y su impacto. No amplíes el alcance por iniciativa propia.

MODO CAMBIO DE FOCO
1. Inspecciona `specs/*/spec.md` y presenta ruta relativa, título, versión y estado; no modifiques ningún artefacto.
2. Si no indiqué inequívocamente la spec, pídeme seleccionar una. Nunca elijas por número mayor, fecha o estado.
3. Lee los artefactos de la spec elegida y actualiza `MEMORY.md` con su ruta/versión/estado; registra plan, tareas y validación con su estado real solo si las versiones son compatibles. Nunca infieras una aprobación ni presentes un artefacto obsoleto como vigente. Establece la siguiente acción segura según el primer gate pendiente.
4. Cambiar el foco no altera estados, versiones, aprobaciones ni historial. Si la spec está `Cancelada`, solo se permite inspección hasta ejecutar `REVISIÓN`.
5. Detente; no continúes automáticamente con planificación o implementación.

MODO CANCELACIÓN
1. Resuelve la spec exacta y lee todos sus artefactos. Si no es inequívoca, pregunta antes de continuar.
2. Si está `Validada`, detente: retirar o desactivar esa funcionalidad requiere una spec nueva. No cambies su evidencia histórica.
3. Solicita confirmación explícita y un motivo; sin ambos, no cambies nada.
4. Cambia la spec a `Cancelada` sin cambiar su versión y registra fecha, motivo y rol en el historial.
5. Marca plan y `tasks.md` como `Obsoletos`, conserva `validation.md` como evidencia histórica y no borres ni renombres archivos.
6. Registra código parcial, datos, despliegues o efectos residuales. No los reviertas ni elimines: cualquier limpieza o desactivación requiere una spec/tarea aprobada.
7. Si era la spec activa, deja en `MEMORY.md` “Especificación activa: Ninguna” y limpia plan, tareas y validación activos; no selecciones otra automáticamente.
8. Detente. Reanudar requiere `REVISIÓN` normativa y aprobación normal.

AUTO-QA Y APROBACIÓN — SOLO NUEVA O REVISIÓN
1. Busca ambigüedades, contradicciones, requisitos no verificables, casos límite ausentes, riesgos de privacidad/seguridad, RNF incompletos y conflictos con la constitución.
2. Presenta hallazgos con la severidad definida en `AGENTS.md` y resuelve conmigo una sola decisión cada vez. Actualiza spec e historial.
3. Comprueba trazabilidad completa, ausencia de dudas bloqueantes y placeholders, y justificación de secciones no aplicables.
4. No marques la spec como `Aprobada` sin mi aprobación explícita. En una spec nueva, la primera aprobación usa versión `1.0`; en una revisión normativa, conserva la versión candidata. Actualiza estado, historial y `MEMORY.md`; después detente. No generes `plan.md`.
