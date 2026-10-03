# Desglose de tareas

Genera o amplía `tasks.md` para la especificación activa, sin implementar ninguna tarea.

PRECONDICIONES
1. Lee `docs/constitution.md`, `AGENTS.md`, `MEMORY.md`, `spec.md` y `plan.md` completos. Si existe, lee también `tasks.md` y el último resultado de `validation.md`.
2. Modo inicial/evolución: confirma que la spec esté `Aprobada` o `En implementación`, que el plan esté `Aprobado`, cite la versión exacta de la spec y no tenga bloqueos ni autorizaciones pendientes.
3. Modo remediación: solo procede si `validation.md` terminó en `NO VALIDADA`, el contenido normativo de spec/plan sigue vigente y cada hallazgo cabe dentro del alcance aprobado. Si exige cambiar requisitos o arquitectura, detente y devuelve primero el documento afectado a `Borrador`.

METADATOS OBLIGATORIOS DE `tasks.md`
- Estado: `Borrador` hasta aprobación explícita; después `Aprobado`. Si spec o plan dejan de coincidir, `Obsoleto` hasta revisar el impacto.
- Versión propia: `0.1` inicialmente, `1.0` en la primera aprobación y aumento menor en cada reaprobación.
- Rutas relativas y versiones exactas de spec y plan.
- Fecha e historial de cambios.

REGLAS DE DESGLOSE
1. Divide el trabajo en tareas pequeñas de aproximadamente 20–30 minutos. Si una excede ese tamaño, divídela.
2. Ordénalas por dependencias y prioriza “vertical slices” verificables sobre capas enormes aisladas.
3. No agregues archivos, decisiones ni capacidades ausentes del plan aprobado.
4. Incluye pruebas, errores, RNF, seguridad, documentación y verificación cuando sean aplicables.
5. La última tarea debe ejecutar la verificación integrada; no sustituye la validación RF por RF.
6. Si `tasks.md` ya existe, conserva tareas, checkboxes, evidencia e historial. Si cambió spec o plan, marca primero el documento `Obsoleto`, revisa cada tarea y luego pásalo a `Borrador`. Conserva su `Origen`; actualiza `Vigencia` solo después de comparar versiones. Si la evidencia puede haber perdido validez, crea una tarea de reverificación. Marca `Obsoleta` cualquier tarea que ya no aplique y crea tareas explícitas para retirar o adaptar código previo cuando sea necesario. Nunca borres, renumeres, reutilices IDs ni declares completada una tarea obsoleta; agrega IDs nuevos consecutivos.
7. En remediación, cada tarea nueva debe citar además el ID `VAL-NNN` del hallazgo que corrige.
8. Mantén el invariante: `[x]` solo significa `Completada`; `[ ]` solo acompaña `Pendiente`, `Bloqueada` u `Obsoleta`. Ninguna tarea aplicable puede depender de una `Obsoleta`; debe apuntar a su reemplazo o eliminar justificadamente esa dependencia.
9. Toda verificación debe indicar entorno y tipo de datos. Usa datos sintéticos y dobles de prueba; una tarea con efectos reales o sobre producción permanece `Bloqueada` hasta registrar autorización explícita.

FORMATO OBLIGATORIO DE CADA TAREA
- [ ] **T-NNN — [Resultado concreto]** `[RF-x, RNF-x, CL-x, VAL-x si aplica]`
  - **Estado:** [Pendiente | Bloqueada | Completada | Obsoleta]
  - **Origen:** [Spec ruta@versión | Plan ruta@versión; inmutable]
  - **Vigencia:** [Spec ruta@versión | Plan ruta@versión | Obsoleta con motivo/reemplazo]
  - **Tipo:** [Código | Prueba | UI | Datos | Configuración | Documentación | Verificación]
  - **Depende de:** [T-NNN o “Ninguna”]
  - **Archivos previstos:** [Rutas aprobadas en el plan]
  - **Pasos:** [Máximo 3–5 acciones concretas]
  - **Verificación:** [Entorno, datos sintéticos, comando exacto o procedimiento manual reproducible]
  - **Hecho cuando:** [Condición binaria, observable y 100% verificable]

CIERRE
1. Valida cobertura de RF, RNF y CL en modo inicial, o de todos los hallazgos en modo remediación; comprueba estados, origen, vigencia, dependencias, archivos y verificaciones. Las tareas `Obsoletas` no cuentan como cobertura y una tarea solo cubre versiones incluidas en su `Vigencia`.
2. No marques `tasks.md` como `Aprobado` si quedan placeholders, inconsistencias checkbox/estado, dependencias circulares o dependencias hacia tareas obsoletas, ni sin mi aprobación explícita. Cuando la reciba, actualiza versión, estado e historial.
3. Si son tareas de remediación aprobadas, cambia la spec de `Implementada` a `En implementación`; esto no cambia su versión.
4. Actualiza `MEMORY.md` con la ruta/versión de `tasks.md` como “Tareas activas” y la siguiente tarea desbloqueada. Después detente.
