# AGENTS.md — {{NOMBRE_PROYECTO}}

> **Objetivo:** [Una o dos frases: qué es, para quién y cuál es el objetivo principal del sistema.]

## Stack y estructura

- **Tecnologías:** [Ej: NestJS / Flutter / React Native, PostgreSQL, etc.]
- **Versiones y toolchain:** [Runtime, SDK, compilador y gestor de paquetes con versiones soportadas.]
- **Entornos soportados:** [SO, navegador, dispositivo o plataforma; “No aplica” si corresponde.]
- **Arquitectura:** [Ej: Feature-First, Clean Architecture, MVC.]
- **Carpetas clave:** [Dónde viven dominio, aplicación, infraestructura, UI y pruebas.]
- **Configuración:** [Nombres de variables y archivo de ejemplo; nunca valores reales ni secretos.]

## Comandos oficiales

- **Instalar:** [Comando reproducible para preparar el entorno.]
- **Ejecutar:** [Comando para levantar el proyecto.]
- **Pruebas:** [Comando principal de tests.]
- **Análisis estático:** [Comando de lint/typecheck o “No aplica”.]
- **Build:** [Comando de compilación o empaquetado.]

No inventar comandos. Si falta uno, registrarlo como bloqueo o pedirlo.

## Convenciones

- Código, nombres y arquitectura en [idioma definido en la constitución].
- Documentación, interfaz y mensajes de error en [idioma definido en la constitución].
- Estilo y nombres: [Convenciones concretas del stack.]
- Patrones obligatorios: [Patrón y archivo de referencia, si existe.]
- Mantener la separación entre dominio, infraestructura e interfaz.
- Guardar rutas de proyecto en formato relativo a la raíz; no persistir rutas absolutas, nombres de usuario ni detalles de la máquina.

## Reglas de dominio y trampas conocidas

- [Regla que no pueda deducirse fácilmente del código.]
- [Ej: Las fechas se almacenan en UTC y solo se convierten en la interfaz.]

## Severidad común

- **Bloqueante:** Impide aprobar, ejecutar o validar; o implica riesgo de seguridad, privacidad, corrupción o pérdida de datos.
- **Alta:** Incumple un RF/RNF importante sin alternativa aceptable.
- **Media:** Degradación parcial con alternativa temporal y sin riesgo crítico.
- **Baja:** Defecto menor, editorial o de mantenibilidad sin impacto funcional inmediato.

## Entrada SDD portable

- La entrada canónica del flujo es `.agents/skills/sdd/SKILL.md`; sus runbooks
  detallados viven en `.agents/skills/sdd/references/`.
- Invocar `$sdd` en Codex o `/sdd` en Claude Code y OpenCode para obtener un
  menú contextual con la siguiente acción segura. La entrada pasa siempre por
  `coordinator`, que delega cada etapa en el agente especializado; si los
  agentes no están disponibles, el flujo se detiene sin ejecución directa.
- Los modos explícitos siguen disponibles como atajos: `$sdd plan` o `/sdd plan`.
- Primer ciclo: `inicio` → `plan` → `tareas` → `ejecutar` (una vez por tarea)
  → `validar`; `inicio` configura el proyecto y conduce la primera spec.
- Ciclos posteriores: `nueva` → `plan` → `tareas` → `ejecutar` (una vez por
  tarea) → `validar`.
- Operaciones complementarias sobre una spec: `revisar`, `foco` y `cancelar`;
  no son etapas consecutivas del flujo.
- Modos de consulta: `estado` y `ayuda`.
- Si el host distingue Plan de Build/Default, el menú, `estado` y `ayuda`
  funcionan en cualquier modo; las demás opciones requieren Build/Default
  porque escriben artefactos o ejecutan trabajo.
- Ante un modo incompatible, detenerse antes de cargar el runbook o producir
  efectos, indicar cómo cambiar de modo y nunca cambiarlo automáticamente.
- Cada invocación ejecuta una sola etapa y se detiene en su gate. No encadenar
  aprobación, planificación, tareas, implementación o validación automáticamente.

## Protocolo obligatorio antes de trabajar

1. Leer `docs/constitution.md` completo.
2. Leer `MEMORY.md` y obtener de allí la ruta relativa de la especificación activa. Comparar estado y versiones reflejados en memoria con los artefactos; si difieren, los artefactos prevalecen y se corrige la memoria.
3. Para planificar o implementar, leer completos `spec.md`, `plan.md` y `tasks.md` aplicables; leer también `validation.md` al validar o remediar.
4. Comprobar el estado de aprobación requerido y si existen dudas bloqueantes.
5. Inspeccionar el estado del repositorio y preservar cambios ajenos o no relacionados. Si aún no existe un repositorio, indicarlo sin inicializarlo por cuenta propia.
6. Explicar brevemente el enfoque antes de modificar archivos.

Si no hay spec activa o su ruta es ambigua, detener el trabajo de implementación y pedir que se seleccione una.

## Forma de trabajar

- Seguir la jerarquía y las puertas de aprobación de la constitución.
- Realizar cambios pequeños, incrementales y fáciles de revertir.
- Implementar únicamente la primera tarea `[ ]` con estado `Pendiente`, versiones activas compatibles y dependencias completas.
- Usar rojo-verde para comportamiento automatizable. Para otros tipos de tarea, ejecutar la verificación declarada en `tasks.md`.
- No modificar una prueba solo para ocultar un defecto. Si la spec, el plan y el test discrepan, detenerse y resolver la contradicción.
- No afirmar que algo funciona sin ejecutar la verificación correspondiente y registrar su resultado.
- No mezclar refactors oportunistas con la tarea activa.
- Mantener el invariante de tareas: `[x]` solo con estado `Completada`; `[ ]` con `Pendiente`, `Bloqueada` u `Obsoleta`. Si checkbox y estado discrepan, detenerse y corregir la inconsistencia antes de continuar.
- No renumerar ni reutilizar identificadores aprobados. Conservar elementos retirados u obsoletos con motivo y versión para mantener trazabilidad histórica.

## Memoria

- Actualizar `MEMORY.md` después de cambios materiales: implementación, decisiones aprobadas, bloqueos o validación.
- No actualizarla por inspecciones o consultas que no cambien el estado del proyecto.
- Mantenerla por debajo de ~50 líneas, eliminando información obsoleta.
- Registrar decisiones duraderas con su motivo y errores que no deben repetirse.
- Mover reglas permanentes a este archivo o a la constitución; `MEMORY.md` no es normativa.
- Nunca guardar claves, tokens, datos personales ni otros datos sensibles.

## Límites

- 📜 **Siempre:** Respetar `docs/constitution.md` y el alcance de la spec activa.
- ✅ **Siempre:** Conservar trazabilidad entre requisito, tarea y evidencia.
- ⚠️ **Requiere autorización explícita separada:** Instalar dependencias, modificar esquemas o migraciones de datos, integrar servicios externos, actuar sobre producción, enviar mensajes reales, generar cobros o realizar acciones destructivas, aunque aparezcan en un borrador técnico.
- ✅ **Se considera autorizado por el flujo:** Crear los artefactos SDD solicitados por la skill `sdd` y sus runbooks en `references/`, incluido el puente mínimo `CLAUDE.md` solo si `inicio` demuestra que hace falta, y crear o modificar archivos de implementación enumerados en el plan y `tasks.md` aprobados.
- ⚠️ **Cualquier otro archivo o carpeta nueva:** Requiere aprobación antes de crearse.
- 🚫 **Nunca:** Exponer secretos, borrar trabajo ajeno, romper la separación de responsabilidades o refactorizar fuera de alcance.

## Verificación de cierre

- Ejecutar pruebas, análisis estático y build aplicables.
- Confirmar que la verificación usa el entorno previsto y datos sintéticos; no producir efectos externos reales sin autorización explícita.
- Verificar RF, RNF y casos límite afectados.
- Revisar el diff para detectar cambios accidentales, secretos o archivos generados.
- Marcar una tarea como `Completada` y `[x]` solo cuando su “Hecho cuando” tenga evidencia.
- Si una verificación no puede ejecutarse, conservar `[ ]`, marcar la tarea `Bloqueada` y documentar la causa. Para reanudarla, registrar la resolución y devolverla a `Pendiente`.
