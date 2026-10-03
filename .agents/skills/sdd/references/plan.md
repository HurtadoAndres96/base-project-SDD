# Plan técnico

Vamos a diseñar el plan técnico de la especificación activa. No escribas código de producto.

PRECONDICIONES
1. Lee `docs/constitution.md`, `AGENTS.md` y `MEMORY.md` completos.
2. Obtén de `MEMORY.md` la ruta exacta de la spec activa y léela completa.
3. Confirma que la spec esté `Aprobada`, `En implementación` o `Implementada`, sin dudas bloqueantes. `En implementación` permite revisar el plan técnico sin cambiar requisitos; `Implementada` solo permite atender hallazgos de validación dentro del alcance. Si está `Borrador`, `Validada` o `Cancelada`, detente y explica qué flujo corresponde.

GENERA O ACTUALIZA `plan.md` EN LA MISMA CARPETA CON ESTA ESTRUCTURA
1. Metadatos: estado `Borrador`, versión propia `0.1`, ruta y versión exacta de la spec usada, fecha e historial de cambios. Estados permitidos del plan: `Borrador` → `Aprobado`; si la versión de la spec deja de coincidir, `Obsoleto` → `Borrador` tras revisar el impacto.
2. Resumen técnico y límites del cambio.
3. Archivos: archivos creados o modificados y responsabilidad estricta de cada uno.
4. Lógica y datos: funciones, clases, contratos, modelos y migraciones; marca cualquier acción que requiera autorización.
5. Flujo principal: pseudocódigo o secuencia, incluyendo fallos y recuperación.
6. UI/API/Integraciones: estados, contratos, accesibilidad y errores visibles.
7. Seguridad y privacidad: amenazas relevantes y mitigaciones.
8. Decisiones técnicas: alternativa elegida, alternativas descartadas y consecuencias.
9. Estrategia de pruebas: prueba o evidencia para cada RF, RNF y caso límite; entorno aislado, datos sintéticos y dobles de prueba cuando correspondan.
10. Efectos externos: mensajes, cobros, integraciones, migraciones o acciones sobre producción; aislamiento, autorizaciones y método seguro de prueba.
11. Observabilidad y operación: logs no sensibles, métricas, compatibilidad, despliegue, reversión y criterios de liberación; indicar “No aplica” con motivo cuando corresponda.
12. Matriz de cobertura: requisito/caso límite → sección del plan → evidencia prevista.
13. Riesgos y dudas del plan con ID, severidad, carácter bloqueante, mitigación y rol responsable.

REGLAS
- Cita explícitamente los IDs RF, RNF y CL cubiertos en cada sección.
- No agregues requisitos ni capacidades que no estén en la spec.
- No ocultes decisiones irreversibles o cambios de datos.
- Enumera como “Autorizaciones pendientes” dependencias nuevas, esquemas/migraciones, servicios externos, mensajes o cobros reales, acciones sobre producción y operaciones destructivas. Obtén y registra una decisión explícita para cada una antes de aprobar el plan.
- Si ya existe `plan.md`, no lo sobrescribas: conserva su historial y compara la versión de la spec. Si no coincide, márcalo primero `Obsoleto`, revisa el impacto y pásalo a `Borrador` antes de modificarlo. Cualquier cambio normativo exige nueva aprobación.
- Haz una auto-revisión de consistencia, simplicidad, alcance, testabilidad y reversibilidad.
- No marques el plan como `Aprobado` con riesgos/dudas bloqueantes, placeholders o autorizaciones pendientes, ni sin mi aprobación explícita. En la primera aprobación cambia su versión a `1.0`; en aprobaciones posteriores incrementa la versión menor. Actualiza estado e historial, registra en `MEMORY.md` su ruta relativa y versión como “Plan activo”, marca el `tasks.md` anterior `Obsoleto` hasta revisar cada tarea y considera las validaciones anteriores históricas para las versiones nuevas; detente y no generes tareas.
