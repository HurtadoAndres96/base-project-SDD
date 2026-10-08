# Ejecución de una tarea

Implementa SOLO la primera tarea `[ ]` con estado `Pendiente`, `Vigencia` compatible con spec/plan activos y dependencias completas del índice global `tasks.md`, sea monolítico o modular. Un archivo de detalle indicado por el usuario no cambia esa selección.

PREPARACIÓN
1. Lee completos `docs/constitution.md`, `AGENTS.md`, `MEMORY.md`, `spec.md`, `plan.md` y `tasks.md`. Si `tasks.md` declara formato modular, comprueba las rutas y los T-ID enlazados sin cargar el cuerpo de todos los detalles; selecciona desde el índice la primera tarea elegible y lee completo solo el detalle enlazado por su ruta relativa a la raíz. Si la tarea cita un `VAL-NNN`, lee también la corrida correspondiente de `validation.md`.
2. Confirma que la spec esté `Aprobada` o `En implementación`, que plan y `tasks.md` estén `Aprobados`, y que los tres documentos citen versiones compatibles.
3. Confirma el invariante checkbox/estado y la `Vigencia` de todas las tareas aplicables desde `tasks.md`; detente si existe una inconsistencia. En modular, detente también ante un detalle faltante, huérfano, duplicado o cuyo encabezado no coincida con el T-ID del índice.
4. Confirma que la tarea pertenezca al alcance aprobado, que sus dependencias estén completas, que todos los archivos previstos en el detalle seleccionado —o en la entrada monolítica— estén autorizados y que cualquier autorización separada esté registrada.
5. Inspecciona el estado del repositorio y conserva cualquier cambio ajeno o no relacionado.
6. Indica qué tarea ejecutarás, qué IDs cubre, qué archivos tocarás y cómo la verificarás.
7. Informa las tareas `Bloqueadas` anteriores a la seleccionada; no las marques completas ni cambies su estado para saltarlas. Una tarea independiente posterior puede ejecutarse si es la primera `Pendiente` elegible según el orden global. Una `Bloqueada` solo vuelve a `Pendiente` con evidencia de que su causa se resolvió; registra esa transición antes de reanudarla.
   Si `tasks.md` es monolítico y supera los umbrales de `AGENTS.md` (por defecto, más de 15 tareas o 300 líneas), informa que `$sdd tareas` lo migrará en otra invocación. Este aviso no invalida la aprobación actual ni cambia la tarea elegible.
8. Antes de ejecutar comandos, confirma entorno y datos. Si pueden enviar mensajes, cobrar, modificar producción o causar otro efecto real, detente salvo que exista autorización explícita para ese efecto y entorno exactos.

EJECUCIÓN
1. Si la tarea cambia comportamiento automatizable, escribe o ajusta primero la prueba mínima, ejecútala y confirma que falla por la razón esperada (ROJO). Si ya pasa, no fabriques un fallo: comprueba si la tarea ya está satisfecha o si la prueba no demuestra el requisito.
2. Implementa el cambio mínimo que satisfaga la tarea, sin refactors ni capacidades adicionales.
3. Ejecuta la prueba específica hasta que pase (VERDE) y luego las verificaciones de regresión aplicables.
4. Para UI visual, documentación o configuración no automatizable, sigue el procedimiento reproducible declarado en la tarea; no simules un ciclo rojo-verde.
5. Si falla cualquier verificación obligatoria o regresión y no puede resolverse dentro del alcance de la tarea, conserva `[ ]`, cambia su estado a `Bloqueada` en `tasks.md`, registra la causa en la entrada monolítica o el detalle seleccionado y detente.

CIERRE
1. Revisa el diff y comprueba alcance, separación de responsabilidades, secretos y cambios accidentales.
2. Marca `[x]` únicamente la tarea ejecutada y cambia su estado a `Completada` en `tasks.md`. Añade `Evidencia: [fecha/hora RFC 3339, comando o procedimiento, código de salida y resultado]` en la entrada monolítica o, en modular, solo en su detalle seleccionado. No cambies el contenido normativo ni la aprobación del conjunto.
3. Si era la primera tarea vigente completada, cambia la spec a `En implementación`. Solo cuando todas las tareas cuya `Vigencia` incluya las versiones activas estén `Completadas` y no exista ninguna `Bloqueada`, cambia la spec a `Implementada`; las tareas `Obsoletas` no bloquean. Registra solo la transición de estado en su historial; no alteres requisitos.
4. Actualiza `MEMORY.md` con el estado material, decisiones o errores a evitar, manteniéndolo breve.
5. Informa resultados rojo-verde o evidencia equivalente, archivos modificados y riesgos pendientes.
6. DETENTE. No empieces, prepares ni marques la siguiente tarea.
