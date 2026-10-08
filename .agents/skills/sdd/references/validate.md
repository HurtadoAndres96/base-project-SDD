# Validación de la funcionalidad

Valida de extremo a extremo la funcionalidad activa. No corrijas defectos durante esta fase: produce evidencia durable y separa validación de implementación.

PREPARACIÓN
1. Lee completos `docs/constitution.md`, `AGENTS.md`, `MEMORY.md`, `spec.md`, `plan.md` y `tasks.md`. Si las tareas son modulares, lee todos los detalles referenciados para comprobar cobertura y evidencia, no solo el índice.
2. Confirma que spec esté `Implementada`, plan y el conjunto de tareas estén `Aprobados`, y que todas las tareas cuya `Vigencia` incluya sus versiones estén `Completadas`, sin ninguna `Bloqueada`. En modular, comprueba que cada detalle enlazado existe, tiene el T-ID correcto y no hay duplicados ni huérfanos. Las tareas `Obsoletas` se excluyen solo si tienen motivo y reemplazo trazables. Si algo falla, registra el bloqueo y no declares cumplimiento.
3. Inspecciona el estado del repositorio para detectar cambios no previstos y registra el entorno relevante: fecha/hora RFC 3339 con zona, revisión/commit si existe, sistema y versiones de runtime; omite nombres de usuario, host y rutas personales.
4. Confirma que el entorno y los datos sean seguros. No envíes mensajes reales, generes cobros, ejecutes migraciones productivas ni modifiques producción sin autorización explícita para esa acción exacta.

EVIDENCIA DURABLE
Crea o actualiza `validation.md` en la carpeta de la spec. No sobrescribas corridas anteriores: añade una nueva sección fechada con:
1. Versiones exactas de spec, plan y `tasks.md` (que cubre sus detalles enlazados si es modular), entorno y alcance validado.
2. Comandos ejecutados, código de salida y resumen relevante; nunca incluyas secretos ni datos personales.
3. Matriz `H/RF/RNF/CL → plan → T-ID y ruta de detalle si es modular → test o evidencia → resultado`.
4. Verificaciones manuales con precondiciones, pasos, resultado esperado, resultado observado y rol responsable/evidencia; no incluyas identidades ni datos sensibles.
5. Hallazgos numerados `VAL-NNN`, cada uno con severidad, IDs afectados, evidencia y condición concreta de cierre. Los IDs son consecutivos y nunca se reutilizan entre corridas.
6. Estado de cada hallazgo previo abierto: `RESUELTO | NO RESUELTO | NO APLICA`, con evidencia actual. No edites la corrida histórica donde se originó.
7. Veredicto de cumplimiento, evaluación separada de preparación para producción y riesgos pendientes.

VALIDACIÓN
1. Ejecuta los comandos oficiales aplicables de pruebas, análisis estático y build; no reutilices resultados antiguos como si fueran actuales.
2. Recorre individualmente cada RF, RNF y CL de la spec.
3. Para cada ID usa `CUMPLE | NO CUMPLE | BLOQUEADO | PENDIENTE MANUAL` y enlaza la evidencia exacta.
4. No declares una comprobación visual o manual como cumplida hasta realizarla y registrar el resultado observado.
5. Verifica privacidad, seguridad, manejo de errores, regresiones, documentación y todos los criterios de finalización.
6. Recorre por separado los criterios de liberación: despliegue, reversión, compatibilidad, observabilidad, seguridad y aprobaciones aplicables.
7. Comprueba la trazabilidad completa y que el diff no contenga cambios accidentales.

CIERRE
- Clasifica hallazgos por severidad: Bloqueante, Alta, Media o Baja.
- Usa exactamente uno de estos veredictos:
  - `VALIDADA`: todo cumple y no queda verificación manual pendiente.
  - `VALIDACIÓN CONDICIONAL`: lo automatizado cumple, pero queda evidencia manual pendiente.
  - `NO VALIDADA`: existe al menos un incumplimiento reproducible.
  - `BLOQUEADA`: no fue posible obtener evidencia suficiente por una causa externa o de entorno.
- Registra además una conclusión de liberación independiente:
  - `LISTA PARA PRODUCCIÓN`: spec `VALIDADA`, criterios de liberación completos y entorno representativo.
  - `NO LISTA PARA PRODUCCIÓN`: falta al menos un criterio de liberación o existe un riesgo bloqueante.
  - `NO APLICA`: la funcionalidad no tiene liberación independiente, con justificación.
- Después de cualquier corrida, actualiza en la spec “Resultado de validación” y en `MEMORY.md` “Última validación” con ruta, fecha, veredicto y conclusión de liberación; esto no cambia la versión normativa.
- Solo con `VALIDADA`, cambia la spec a `Validada`, completa sus criterios y actualiza su historial.
- Con `NO VALIDADA`, conserva la spec como `Implementada`, registra los hallazgos y señala que `$sdd tareas` debe ejecutarse en modo remediación.
- Con `VALIDACIÓN CONDICIONAL` o `BLOQUEADA`, conserva la spec como `Implementada`, registra exactamente qué evidencia falta y no generes tareas de código salvo que exista un defecto demostrado.
- No afirmes que está lista para producción salvo que coincidan `VALIDADA` y `LISTA PARA PRODUCCIÓN`. Detente sin corregir defectos.
