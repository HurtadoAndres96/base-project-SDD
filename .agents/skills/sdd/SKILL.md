---
name: sdd
description: Guía el ciclo Spec-Driven Development de este repositorio. Úsala al invocar $sdd, pedir una etapa del flujo o responder a un menú SDD previamente mostrado.
---

# SDD

Ofrece una entrada guiada al flujo SDD y ejecuta una sola etapa por selección.
Los runbooks autocontenidos en `references/` conservan el procedimiento
detallado; carga solo el que corresponda a la etapa elegida.

## Gate de compatibilidad con el modo del host

Antes de cargar un runbook o ejecutar una etapa, determina el modo solicitado y,
cuando el host exponga modos de colaboración, comprueba que el actual permita el
resultado esperado:

- El menú guiado, `estado` y `ayuda` son de solo lectura y funcionan en cualquier
  modo.
- `inicio`, `nueva`, `revisar`, `foco`, `cancelar`, `plan`, `tareas`, `ejecutar`
  y `validar` requieren un modo con capacidad de escribir o ejecutar, denominado
  **Build/Default** en Codex. `$sdd plan` diseña y persiste `plan.md`; no activa
  Plan Mode del host.
- Si el host informa un modo incompatible, detente antes de leer el runbook,
  modificar archivos o ejecutar comandos. Indica el modo actual, el requerido,
  cómo cambiarlo y el comando SDD que debe repetirse. Declara expresamente que
  no se modificaron archivos.
- Nunca cambies el modo del host por cuenta propia. Si el host no distingue o no
  expone modos, no inventes un bloqueo: continúa bajo sus permisos normales.

## Entrada guiada predeterminada

Cuando la invocación no tenga argumentos, diga `menú` o pregunte qué sigue:

1. Trabaja desde la raíz que contiene `AGENTS.md` y lee completos
   `docs/constitution.md`, `AGENTS.md` y `MEMORY.md`.
2. Inspecciona los metadatos y estados de los artefactos que `MEMORY.md`
   referencia. Si memoria y artefactos discrepan, muestra la discrepancia y usa
   el artefacto como verdad para construir el menú; no corrijas archivos todavía.
3. Determina el primer gate pendiente en este orden:
   configuración completa **y aprobada** → primera spec creada → spec activa →
   spec aprobada → plan aprobado y compatible → tareas aprobadas y compatibles
   → tareas completadas → validación vigente. La ausencia de placeholders no
   demuestra aprobación: comprueba la decisión explícita registrada en
   `MEMORY.md` o recibida en la conversación actual.
4. Muestra entre dos y cinco acciones aplicables. Numéralas dinámicamente, marca
   una como `Recomendada` y explica en una frase el estado o bloqueo relevante.
   Incluye `Ver estado y bloqueos` cuando ayude y `Ayuda completa` como última
   opción. No muestres etapas que el gate actual prohíbe.
5. No cargues ningún runbook ni modifiques archivos al mostrar el menú. Espera
   una respuesta con el número o una intención en lenguaje natural.

Usa estas reglas para recomendar la siguiente acción:

- Si la configuración está incompleta o su aprobación sigue pendiente,
  recomienda `inicio`, incluso si ya no quedan placeholders.
- Si la configuración está aprobada y aún no existe ninguna spec numerada,
  recomienda `inicio` para conducir la primera spec.
- Si ya existen specs numeradas pero no hay una activa, ofrece `foco` para
  seleccionar una existente y `nueva` para un objetivo independiente.
- Si la spec activa está en `Borrador`, recomienda `revisar`.
- Si falta un plan aprobado compatible, recomienda `plan`.
- Si falta un `tasks.md` aprobado compatible, recomienda `tareas`.
- Si existe una tarea elegible pendiente, recomienda `ejecutar`.
- Si todas las tareas vigentes están completas y la spec está `Implementada`,
  recomienda `validar`.
- Si la última corrida fue `NO VALIDADA`, recomienda `tareas` en remediación.
- Si la spec está `Validada`, ofrece `nueva` y `foco`.
- Si está `Cancelada`, ofrece `revisar` para reanudar o `foco`; nunca la reanudes
  automáticamente.

Si el mensaje actual responde al último menú SDD con un número, nombre de opción
o frase inequívoca, usa esa selección sin exigir que se vuelva a escribir
`$sdd`. Si el estado cambió, la opción ya no es válida o la intención es
ambigua, refresca el menú o haz una sola pregunta; no adivines.

Si el mensaje responde inequívocamente a una aprobación solicitada por la etapa
SDD en curso, retoma **esa misma etapa** y verifica el artefacto o la
configuración exactos antes de registrar la decisión. Una aprobación de
configuración pendiente retoma `inicio`; no se interpreta como `nueva` ni abre
el menú. No uses una aprobación para iniciar la etapa siguiente.

## Lenguaje natural

Acepta frases corrientes además de los modos exactos. Por ejemplo:

- “quiero comenzar” o “configura el proyecto” → `inicio`
- “crea la primera spec” → `inicio`; “crea otra spec” o “nueva
  funcionalidad” → `nueva` solo después de completar el primer ciclo de `inicio`
- “corrige/cambia la spec” → `revisar`
- “trabajemos en otra spec” → `foco`
- “haz el plan” → `plan`
- “divide el trabajo” o “genera tareas” → `tareas`
- “implementa/continúa con la siguiente” → `ejecutar` solo si el gate lo permite
- “comprueba/valida la funcionalidad” → `validar`

`continúa` inspecciona el estado y selecciona únicamente la acción recomendada
por el gate actual. Nunca infieras `cancelar` ni otra acción que requiera
confirmación explícita.

## Presentación de la ayuda completa

Cuando se solicite `ayuda` o se elija **Ayuda completa**, presenta los modos por
función y no como una lista plana:

1. **Rutas principales, en orden:**
   - Primer ciclo: `inicio` → `plan` → `tareas` → `ejecutar` (una vez por
     tarea) → `validar`. Aclara que `inicio` configura un proyecto nuevo o
     existente, registra la aprobación de esa configuración y conduce la
     primera spec; no se ejecuta `nueva` inmediatamente después.
   - Ciclos posteriores: `nueva` → `plan` → `tareas` → `ejecutar` (una vez por
     tarea) → `validar`.
2. **Operaciones complementarias sobre una spec:** `revisar`, `foco` y
   `cancelar`. Aclara que no son pasos posteriores a `nueva`: se usan solo
   cuando el estado y la intención lo requieren.
3. **Consulta y orientación:** `estado` y `ayuda`.

Muestra una descripción breve de cada modo y uno o dos ejemplos en lenguaje
natural. Usa `$sdd` en Codex y `/sdd` en Claude Code u OpenCode. Cierra indicando
el modo del host requerido para cada opción, el estado actual, la siguiente
acción segura y que la consulta no modificó archivos.

## Modos explícitos

Los modos siguen disponibles como atajos. Usa la primera palabra reconocida como
modo y el resto como contexto de esa etapa:

| Grupo | Modo | Modo del host | Runbook que debes leer completo | Resultado permitido |
|---|---|---|---|---|
| Preparación | `inicio` | Build/Default | `references/inicio.md` | Configurar o retomar la configuración de un proyecto nuevo o existente y conducir la primera spec |
| Ciclo 1 | `nueva` | Build/Default | `references/spec.md`, modo `NUEVA` | Crear y aprobar una spec nueva posterior |
| Ciclo 2 | `plan` | Build/Default | `references/plan.md` | Diseñar o actualizar el plan técnico |
| Ciclo 3 | `tareas` | Build/Default | `references/tasks.md` | Generar o ampliar las tareas |
| Ciclo 4 | `ejecutar` | Build/Default | `references/execute.md` | Implementar exactamente una tarea elegible |
| Ciclo 5 | `validar` | Build/Default | `references/validate.md` | Producir una corrida de validación durable |
| Spec | `revisar` | Build/Default | `references/spec.md`, modo `REVISIÓN` | Revisar una spec sin ampliar su objetivo |
| Spec | `foco` | Build/Default | `references/spec.md`, modo `CAMBIO DE FOCO` | Seleccionar la spec activa |
| Spec | `cancelar` | Build/Default | `references/spec.md`, modo `CANCELACIÓN` | Cancelar de forma recuperable |
| Consulta | `estado` | Cualquiera | Ninguno | Mostrar estado, gates y bloqueos sin modificar archivos |
| Consulta | `ayuda` | Cualquiera | Ninguno | Mostrar todos los modos y ejemplos sin modificar archivos |

Si un argumento no coincide con un modo ni con una intención inequívoca, muestra
el menú guiado; no modifiques archivos.

## Contrato de ejecución

1. Trabaja desde la raíz que contiene `AGENTS.md`; todas las rutas persistidas
   deben ser relativas a esa raíz.
2. Aplica primero el gate de compatibilidad con el modo del host. Una etapa
   incompatible termina ahí, antes de cargar su runbook o producir efectos.
3. Resuelve las rutas `references/` desde el directorio de esta skill. Lee
   primero el runbook seleccionado y sigue sus lecturas, precondiciones,
   preguntas, autorizaciones, gates y condiciones de parada. No cargues los
   demás runbooks salvo que el seleccionado lo ordene expresamente.
4. Ejecuta solo el modo solicitado. Una invocación nunca aprueba por sí misma
   una spec, un plan o unas tareas, ni autoriza dependencias, producción,
   migraciones, efectos externos o acciones destructivas.
   Antes de `nueva` o cualquier etapa posterior, exige la aprobación explícita
   de la configuración. Si todavía no existe una spec numerada, la primera se
   conduce mediante `inicio`, aunque se haya pedido `nueva`.
5. No encadenes automáticamente la siguiente etapa. Al alcanzar el gate del
   runbook, detente y comunica el próximo comando seguro.
6. Trata los argumentos posteriores al modo como contexto, no como permiso para
   saltar gates. En `ejecutar`, un ID indicado no permite omitir la primera tarea
   elegible definida por el runbook.
7. Si falta un runbook, `docs/constitution.md`, `AGENTS.md` o `MEMORY.md`, detente
   e informa la ruta faltante; no reconstruyas reglas normativas por inferencia.
8. Cierra con un resumen compacto: etapa ejecutada, archivos cambiados, evidencia
   obtenida, gate actual y siguiente comando seguro. No declares éxito sin la
   verificación exigida por el runbook.

## Invocación recomendada

- Codex: `$sdd`
- Claude Code: `/sdd`
- OpenCode: `/sdd`

Los modos explícitos continúan disponibles, por ejemplo `$sdd plan` o
`/sdd plan`.

El contenido canónico de la skill es este archivo. Los adaptadores de cada host
solo deben reenviar aquí el modo y sus argumentos; no deben duplicar el flujo.
