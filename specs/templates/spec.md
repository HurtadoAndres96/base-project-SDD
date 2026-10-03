# Spec [NNN] — [Nombre descriptivo de la funcionalidad]

## Metadatos

- **Estado:** Borrador
- **Versión:** 0.1
- **Rol aprobador:** [Rol; no incluir datos personales]
- **Creada:** [AAAA-MM-DD]
- **Última actualización:** [AAAA-MM-DD]
- **Dependencias:** [Specs, servicios o decisiones previas; “Ninguna” si no aplica]
- **Entorno objetivo:** [Local, desarrollo, staging, producción o equivalente; sin credenciales]

Estados permitidos: `Borrador` → `Aprobada` → `En implementación` → `Implementada` → `Validada`. Cualquier estado anterior a `Validada` puede pasar a `Cancelada` con confirmación explícita y motivo. Reanudar una spec cancelada requiere revisión normativa, nueva versión y retorno a `Borrador`. Una funcionalidad validada se retira mediante otra spec, no cancelando su evidencia histórica. Una validación fallida puede iniciar el ciclo `Implementada` → tareas de remediación aprobadas → `En implementación`. Si cambia el alcance o contenido normativo después de aprobarse, vuelve a `Borrador`, se reabren los criterios de finalización y el resultado de la nueva versión queda `Pendiente`; la evidencia histórica no se elimina. Una transición de estado no cuenta como cambio de alcance.

Versionado: usar `0.x` mientras sea el borrador inicial y `1.0` en la primera aprobación. Para revisar contenido normativo ya aprobado sin cambiar el objetivo de la funcionalidad, incrementar primero la versión menor (`1.0` → `1.1`), cambiar a `Borrador` y registrar la versión base; la reaprobación conserva esa versión candidata. Una capacidad independiente o un cambio material de objetivo requiere una spec nueva. Cambios editoriales y transiciones de estado no cambian la versión.

Antes de aprobar, eliminar todas las instrucciones y placeholders. Si una sección no aplica, escribir `No aplica` y justificarlo brevemente; no dejar campos vacíos.

Después de la primera aprobación, los IDs nunca se renumeran ni reutilizan. Si un elemento deja de aplicar, conservarlo como `[RETIRADO en vX.Y]` con motivo y reemplazo, si existe.

## Contexto y objetivo

[Qué se construirá, qué problema resuelve y qué resultado observable se espera.]

## Glosario

- **[Término de dominio]:** [Definición precisa usada por esta spec.]

## Alcance

- ✅ [Capacidad incluida]
- ✅ [Capacidad incluida]

## Usuarios / actores

- **[Actor]:** [Necesidad, permisos o responsabilidad relevante]

## Historias de usuario

- **H-1:** Como [rol], quiero [acción] para [beneficio o valor].

## Requisitos funcionales — EARS

Cada requisito debe ser atómico, observable y verificable. Usar únicamente las formas EARS necesarias y eliminar los ejemplos que no apliquen.

- **RF-1 — [Nombre]:** CUANDO [evento], EL SISTEMA DEBE [respuesta observable].
- **RF-2 — [Nombre]:** SI [condición], ENTONCES EL SISTEMA DEBE [respuesta observable].
- **RF-3 — [Nombre]:** MIENTRAS [estado], EL SISTEMA DEBE [comportamiento continuo].
- **RF-4 — [Nombre]:** EL SISTEMA DEBE [comportamiento permanente o regla general].
- **RF-5 — [Nombre]:** DONDE [función opcional esté habilitada], EL SISTEMA DEBE [comportamiento condicionado].

## Requisitos no funcionales

Definir una métrica o procedimiento de verificación; evitar términos como “rápido”, “seguro” o “intuitivo” sin criterio medible.

- **RNF-1 — [Nombre]:** [Umbral o comportamiento medible]. **Verificación:** [Prueba, comando o inspección].

## Datos, privacidad y seguridad

- **Datos tratados:** [Categorías y clasificación; nunca incluir valores o identidades reales. “Ninguno” si no aplica]
- **Persistencia y retención:** [Dónde, cuánto tiempo y eliminación]
- **Permisos y acceso:** [Quién puede hacer qué]
- **Riesgos y mitigaciones:** [Validación, secretos, abuso, filtración, etc.]

## Casos límite y manejo de errores

- **CL-1 — [Caso límite]:** [Condición excepcional o fallo].
  - **Resultado esperado:** [Respuesta observable, recuperación y mensaje si aplica].

## Fuera de alcance

- 🚫 [Capacidad excluida explícitamente para evitar expansión del alcance.]

## Supuestos y dependencias

- **S-1:** [Supuesto que, si resulta falso, obliga a revisar la spec.]
- **D-1:** [Dependencia técnica, de negocio o externa.]

## Matriz de trazabilidad

| Historia | RF / RNF / CL relacionados | Evidencia esperada |
|---|---|---|
| H-1 | RF-1, RNF-1, CL-1 | [Test, métrica o verificación manual] |

## Criterios de finalización

- [ ] Todos los RF, RNF y casos límite tienen evidencia satisfactoria.
- [ ] Pruebas, análisis estático y build aplicables pasan.
- [ ] No quedan dudas bloqueantes ni cambios accidentales.
- [ ] La documentación y `MEMORY.md` reflejan las decisiones vigentes.

## Criterios de liberación

- [ ] Despliegue, compatibilidad y reversión aplicables fueron comprobados.
- [ ] Observabilidad, seguridad, privacidad y operación aplicables fueron comprobadas.
- [ ] Aprobaciones manuales o regulatorias aplicables están registradas.

Si la funcionalidad no se libera de forma independiente, escribir `No aplica` y justificarlo.

## Dudas abiertas

- [ ] **DA-1 [BLOQUEANTE|NO BLOQUEANTE]:** [Pregunta concreta y rol responsable de resolverla.]

Una duda bloqueante impide aprobar la spec. Si no existen dudas, escribir `Ninguna`.

## Resultado de validación

- **Evidencia:** `validation.md` — Pendiente
- **Último veredicto:** Pendiente
- **Preparación para producción:** Pendiente

## Historial de cambios

| Versión | Fecha | Cambio | Motivo | Rol aprobador |
|---|---|---|---|---|
| 0.1 | [AAAA-MM-DD] | Borrador inicial | Nueva funcionalidad | Pendiente |
