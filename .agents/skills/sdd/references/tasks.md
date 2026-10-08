# Desglose de tareas

Genera o amplía el conjunto de tareas de la especificación activa, sin implementar ninguna tarea. `tasks.md` puede ser monolítico o un índice modular con un detalle por tarea en `specs/NNN-nombre/tasks/T-NNN.md`. No crees un segundo checklist.

PRECONDICIONES
1. Lee `docs/constitution.md`, `AGENTS.md`, `MEMORY.md`, `spec.md` y `plan.md` completos. Si existe, lee también `tasks.md`, todos los detalles que referencia cuando es modular y el último resultado de `validation.md`.
2. Modo inicial/evolución: confirma que la spec esté `Aprobada` o `En implementación`, que el plan esté `Aprobado`, cite la versión exacta de la spec y no tenga bloqueos ni autorizaciones pendientes.
3. Modo remediación: solo procede si `validation.md` terminó en `NO VALIDADA`, el contenido normativo de spec/plan sigue vigente y cada hallazgo cabe dentro del alcance aprobado. Si exige cambiar requisitos o arquitectura, detente y devuelve primero el documento afectado a `Borrador`.

METADATOS OBLIGATORIOS DE `tasks.md`
- Estado: `Borrador` hasta aprobación explícita; después `Aprobado`. Si spec o plan dejan de coincidir, `Obsoleto` hasta revisar el impacto.
- Versión propia: `0.1` inicialmente, `1.0` en la primera aprobación y aumento menor en cada reaprobación.
- Rutas relativas y versiones exactas de spec y plan.
- Formato: `monolítico` o `modular`; si un documento anterior no lo declara, trátalo como `monolítico`.
- Fecha e historial de cambios.

ELECCIÓN Y CONTRATO DEL FORMATO
1. Aplica los umbrales explícitos de `AGENTS.md` o, si no los hay, los predeterminados: más de 15 tareas o más de 300 líneas de `tasks.md` monolítico. Cuenta las entradas únicas de tarea, no las menciones históricas a T-ID. Para un conjunto nuevo, estima las líneas monolíticas antes de redactar el detalle; si supera cualquiera de los umbrales, crea el formato modular directamente. Para uno existente, mide su tamaño al entrar en `tareas`: si supera un umbral, migra el conjunto monolítico a modular durante esta etapa, incluso si estaba aprobado, y deja `tasks.md` en `Borrador` para la aprobación del conjunto exacto. Si no supera los umbrales, conserva su formato. No conviertas formatos durante `ejecutar`.
2. En formato modular, `tasks.md` es el único índice canónico: cada T-ID aparece una vez, en orden global, con checkbox, resultado, IDs cubiertos, `Estado`, `Origen`, `Vigencia`, `Tipo`, `Depende de` y ruta `Detalle: specs/NNN-nombre/tasks/T-NNN.md` relativa a la raíz del proyecto. Usa una entrada compacta por tarea; las rutas y versiones de spec/plan en metadatos permiten abreviar sus referencias por fila sin perder la versión exacta. No dupliques estos campos en el detalle, salvo el T-ID de su encabezado.
3. Cada detalle contiene el mismo T-ID en su encabezado y solamente `Archivos previstos`, `Pasos`, `Verificación`, `Hecho cuando` y, tras ejecutarse, `Evidencia` o causa de bloqueo. Mantén un archivo por tarea para que la ejecución cargue solo el necesario. La aprobación y versión de `tasks.md` cubren también todos los detalles enlazados; estos no tienen aprobación ni versión independientes.
4. Comprueba que cada ruta esté dentro de la carpeta de la spec, que cada enlace apunte a un archivo existente con el T-ID correcto y que no haya T-ID, enlaces o detalles huérfanos o duplicados. Conserva detalles de tareas obsoletas enlazados desde su entrada histórica del índice. Una falta o discrepancia bloquea la aprobación y la ejecución.
5. Cualquier cambio normativo en el índice o en un detalle aprobado devuelve `tasks.md` a `Borrador` y exige nueva aprobación del conjunto. Cambiar solo estado, checkbox o evidencia no modifica su aprobación. Al migrar un conjunto aprobado por umbral o petición, conserva IDs, orden, contenido, evidencia e historial, deja el conjunto en `Borrador` y solicita aprobación explícita antes de reanudarlo.

REGLAS DE DESGLOSE
1. Divide el trabajo en tareas pequeñas de aproximadamente 20–30 minutos. Si una excede ese tamaño, divídela.
2. Ordénalas por dependencias y prioriza “vertical slices” verificables sobre capas enormes aisladas.
3. No agregues archivos, decisiones ni capacidades ausentes del plan aprobado.
4. Incluye pruebas, errores, RNF, seguridad, documentación y verificación cuando sean aplicables.
5. La última tarea debe ejecutar la verificación integrada; no sustituye la validación RF por RF.
6. Si `tasks.md` ya existe, conserva tareas, checkboxes, evidencia e historial, incluidos los detalles modulares. Si cambió spec o plan, marca primero el conjunto `Obsoleto`, revisa cada tarea y luego pasa el índice a `Borrador`. Conserva su `Origen`; actualiza `Vigencia` solo después de comparar versiones. Si la evidencia puede haber perdido validez, crea una tarea de reverificación. Marca `Obsoleta` cualquier tarea que ya no aplique y crea tareas explícitas para retirar o adaptar código previo cuando sea necesario. Nunca borres, renumeres, reutilices IDs ni declares completada una tarea obsoleta; agrega IDs nuevos consecutivos.
7. En remediación, cada tarea nueva debe citar además el ID `VAL-NNN` del hallazgo que corrige.
8. Mantén el invariante: `[x]` solo significa `Completada`; `[ ]` solo acompaña `Pendiente`, `Bloqueada` u `Obsoleta`. Ninguna tarea aplicable puede depender de una `Obsoleta`; debe apuntar a su reemplazo o eliminar justificadamente esa dependencia.
9. Toda verificación debe indicar entorno y tipo de datos. Usa datos sintéticos y dobles de prueba; una tarea con efectos reales o sobre producción permanece `Bloqueada` hasta registrar autorización explícita.

FORMATO MONOLÍTICO OBLIGATORIO DE CADA TAREA
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

FORMATO MODULAR OBLIGATORIO
- En `tasks.md`, una entrada compacta por tarea con todos los campos canónicos del punto 2 de «Elección y contrato del formato». La tarea conserva su checkbox y el orden global; el índice no duplica pasos ni evidencia.
- Ejemplo de entrada: `- [ ] **T-001 — Resultado** [RF-1] · Estado: Pendiente · Origen: spec@1.0/plan@1.0 · Vigencia: spec@1.0/plan@1.0 · Tipo: Código · Depende de: Ninguna · Detalle: specs/001-ejemplo/tasks/T-001.md` (las rutas exactas de spec y plan constan en los metadatos del índice).
- En el archivo de detalle, encabezado `# T-NNN` y los campos de detalle del punto 3. No repitas checkbox, resultado, estado, vigencia, dependencias, cobertura ni aprobaciones.
- Si el mismo campo aparece en índice y detalle o falta una parte obligatoria, detente y corrige la inconsistencia antes de pedir aprobación.

CIERRE
1. Valida cobertura de RF, RNF y CL en modo inicial, o de todos los hallazgos en modo remediación; comprueba estados, origen, vigencia, dependencias, archivos y verificaciones. En modular, recorre todas las entradas y detalles vigentes, no solo el índice. Las tareas `Obsoletas` no cuentan como cobertura y una tarea solo cubre versiones incluidas en su `Vigencia`.
2. No marques `tasks.md` como `Aprobado` si quedan placeholders, detalles ausentes o inconsistentes, inconsistencias checkbox/estado, dependencias circulares o dependencias hacia tareas obsoletas, ni sin mi aprobación explícita para el conjunto exacto. Cuando la reciba, actualiza versión, estado e historial del índice.
3. Si son tareas de remediación aprobadas, cambia la spec de `Implementada` a `En implementación`; esto no cambia su versión.
4. Actualiza `MEMORY.md` con la ruta/versión de `tasks.md` como “Tareas activas” y la siguiente tarea desbloqueada. En modular, no registres cada detalle en memoria. Después detente.
