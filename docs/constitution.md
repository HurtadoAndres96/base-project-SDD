# Constitución del Proyecto: {{NOMBRE_PROYECTO}}

> **Nivel de mantenimiento:** {{PERFIL_MANTENEDOR - Ej: Principiante / Full-stack / Producción}}
> **Versión de la constitución:** 1.6

## Jerarquía de autoridad

Cuando dos documentos entren en conflicto, se respeta este orden:

1. Esta constitución.
2. La especificación activa cuya ruta figura en `MEMORY.md`; su contenido normativo solo gobierna después de ser aprobado.
3. El `plan.md` aprobado de esa especificación.
4. Su `tasks.md` aprobado.
5. `validation.md`, que conserva evidencia pero no crea requisitos.
6. `MEMORY.md`, que conserva contexto pero no crea requisitos.

La ambigüedad no se resuelve suponiendo: se registra como duda y se consulta al responsable.

## Principios innegociables

1. **Spec como fuente de verdad:** Cada funcionalidad vive en `specs/NNN-nombre/spec.md`. Ningún comportamiento se implementa sin un RF/RNF aprobado que lo justifique.
2. **Puertas de aprobación:** Una spec debe estar `Aprobada` antes del plan; el plan debe estar `Aprobado` antes de generar tareas; `tasks.md` debe estar `Aprobado` antes de ejecutar. Una duda abierta bloqueante impide avanzar.
3. **Trazabilidad completa:** Todo cambio debe poder recorrerse en ambos sentidos: historia → requisito → decisión del plan → tarea → prueba o evidencia → resultado de validación.
4. **Simplicidad del stack:** Usar la menor cantidad de abstracciones razonable. Antes de agregar una dependencia, demostrar que el entorno no resuelve el problema de forma suficientemente simple y obtener aprobación.
5. **Separación de responsabilidades:** La interfaz debe permanecer pasiva. Reglas de negocio, validaciones y estado viven desacoplados de UI, transporte y persistencia.
6. **Verificación proporcional al riesgo:** El comportamiento automatizable se desarrolla con ciclo rojo-verde. UI visual, documentación y configuración usan una verificación explícita y reproducible. Nunca se declara éxito sin evidencia.
7. **Privacidad y seguridad por defecto:** No incluir valores reales de secretos, tokens, datos personales o contenido sensible en código, specs, memoria, fixtures o evidencia. El producto solo trata categorías de datos autorizadas por la spec, nunca registra valores sensibles en logs y no los envía a terceros sin autorización explícita. Validar entradas y aplicar mínimo privilegio.
8. **Cambios pequeños y enfocados:** Implementar una sola tarea aprobada a la vez. No refactorizar fuera del alcance ni introducir capacidades “por si acaso”.
9. **Control de cambios:** Si cambia el alcance o contenido normativo de una spec aprobada, vuelve a `Borrador`, reabre sus criterios de finalización y deja pendiente la validación de la nueva versión; la evidencia anterior se conserva como historial. Plan y tareas quedan `Obsoletos` hasta revisar el impacto. Si cambia contenido normativo de un plan o `tasks.md` aprobados, el documento afectado vuelve a `Borrador` y requiere nueva aprobación. Transiciones de estado, checkboxes y evidencia no alteran la aprobación.
10. **Convención de idioma:** Código, arquitectura, ramas y variables en **{{IDIOMA_CODIGO - Ej: Inglés}}**; documentación, interfaz y mensajes de error en **{{IDIOMA_UI - Ej: Español}}**.
11. **Evidencia durable:** Los resultados de validación se guardan en `validation.md`; un resultado mostrado únicamente en el chat no cuenta como evidencia persistente.
12. **Portabilidad del contexto:** Rutas guardadas en specs, planes, tareas, validaciones y memoria son relativas a la raíz del proyecto, nunca absolutas ni dependientes de una máquina.
13. **Foco activo explícito:** Solo una spec figura como activa en `MEMORY.md`. Cambiar el foco requiere una instrucción explícita y no elimina ni altera los artefactos de la spec anterior.
14. **Identificadores inmutables:** Después de la primera aprobación, los IDs H, RF, RNF, CL, DA, T y VAL nunca se renumeran ni reutilizan. Un elemento retirado conserva su ID, estado y motivo para no romper referencias históricas.
15. **Verificación segura:** Pruebas y validaciones usan datos sintéticos y entornos aislados por defecto. Enviar mensajes reales, generar cobros, tocar producción o ejecutar otro efecto externo irreversible exige autorización explícita y evidencia del entorno seleccionado.
16. **Cancelación recuperable:** Cancelar una spec no validada exige confirmación explícita y motivo, conserva todos sus artefactos y vuelve obsoletos plan/tareas. No elimina ni revierte código o despliegues. Reanudarla requiere revisión normativa, nueva versión y las puertas de aprobación normales. Retirar una funcionalidad ya validada requiere una spec nueva de desactivación o migración.

## Definición mínima de calidad

Una funcionalidad solo puede declararse `Validada` cuando todos sus RF, RNF, casos límite y criterios de finalización tienen evidencia satisfactoria en `validation.md`; las pruebas, análisis estático y build aplicables pasan; no quedan dudas bloqueantes ni cambios accidentales; y no queda verificación manual pendiente.

`Validada` no significa automáticamente `Lista para producción`. La liberación requiere además que todos los criterios de liberación aplicables estén comprobados, que el entorno sea representativo y que `validation.md` registre explícitamente `LISTA PARA PRODUCCIÓN`.

## Gobernanza de esta constitución

Cualquier cambio requiere aprobación explícita, incremento de versión y registro resumido en `MEMORY.md`. Después del cambio se revisan la spec, el plan y las tareas activas para detectar incompatibilidades. Nunca se relajan estas reglas silenciosamente para completar una tarea.
