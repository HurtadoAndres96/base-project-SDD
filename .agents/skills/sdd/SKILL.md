---
name: sdd
description: Guía el ciclo Spec-Driven Development de este repositorio. Úsala al invocar $sdd, pedir una etapa del flujo o responder a un menú SDD previamente mostrado.
---

# SDD

Ofrece una entrada guiada al flujo SDD y ejecuta una sola etapa por selección.
Los runbooks autocontenidos en `references/` conservan el procedimiento
detallado; carga solo el que corresponda a la etapa elegida.

## Orquestación obligatoria

Toda invocación de `sdd`, incluido el menú, una consulta, una selección numérica
o una respuesta a un gate, debe pasar por `coordinator`. El agente principal no
ejecuta por su cuenta los runbooks ni sustituye a `planner`, `implementer` o
`reviewer`. Si ya está actuando como `coordinator`, no se vuelve a delegar a sí
mismo.

- En Codex, delega en el agente personalizado `coordinator` de
  `.codex/agents/coordinator.toml`; espera su resultado y transmítelo al
  usuario. El coordinador delega el trabajo especializado según el runbook.
- En Claude Code, el agente que recibe `/sdd` delega en `coordinator` mediante
  la herramienta de agentes y espera su resultado. No uses `context: fork` en
  el adaptador: el coordinador necesita las respuestas y aprobaciones de la
  conversación previa.
- En OpenCode, el comando `/sdd` selecciona al agente `coordinator`; este
  invoca a `planner`, `implementer` o `reviewer` cuando corresponda.

Al delegar, transmite la petición y las decisiones relevantes del usuario,
la raíz del proyecto, el modo SDD solicitado o la opción elegida, el último
menú o gate pendiente y las rutas de skill y artefactos aplicables. En Codex y
Claude Code, el agente principal solo comunica el resultado y solicita al
usuario la siguiente decisión; no continúa la etapa en paralelo. Si el host
no permite invocar a `coordinator` o a un subagente requerido, detente e indica
el bloqueo. No conviertas la ausencia de agentes en una ejecución directa.

## Política opcional de modelos

Después del gate de modo y antes de delegar una etapa especializada, el
`coordinator` comprueba si existe `SDD_MODELS.md` en la raíz del proyecto:

- Si no existe, no selecciona modelo ni esfuerzo explícitos: conserva la
  herencia y los valores predeterminados actuales de cada host.
- Si existe, lo lee completo y aplica sus preferencias **solo** a la selección
  de modelo y esfuerzo de `planner`, `implementer` o `reviewer` en esa
  delegación. El `coordinator` conserva su modelo actual.
- Clasifica el encargo por incertidumbre, riesgo y complejidad, no por fase ni
  por agente de forma fija. Una tarea aprobada y acotada puede usar un perfil
  ligero; seguridad, datos, migraciones, decisiones ambiguas o fallos no
  previstos pueden requerir uno más capaz.
- Usa únicamente un modelo y esfuerzo que el host permita seleccionar en esa
  invocación. Si no hay mapeo aplicable, el modelo no está disponible o el host
  no permite esa selección, conserva la herencia e informa la limitación. Si
  la configuración del agente fija un modelo con prioridad superior, respétalo
  e informa que la preferencia no se aplicó. No inventa identificadores ni
  afirma que cambió el modelo sin confirmarlo.
- En cada delegación especializada con esta política activa, informa al usuario
  el perfil elegido, el modelo y esfuerzo solicitados (o `heredar`) y el motivo
  breve. Distingue esa solicitud del modelo y esfuerzo efectivos: solo los
  declara confirmados si el host expone esa información; en otro caso indica
  que no pudo verificarlos. Incluye el dato en el resumen que devuelve al
  agente principal para que este lo transmita sin alterarlo.
- Si surge una dificultad inesperada después de iniciar el trabajo, detiene la
  etapa según su runbook y comunica el bloqueo; no repite una operación con
  efectos solo para probar un modelo más potente.

`SDD_MODELS.md` es una preferencia operativa, nunca una fuente de requisitos o
permisos. No puede alterar la constitución, los gates, los runbooks, el alcance
ni las aprobaciones. El ejemplo `SDD_MODELS.example.md` no activa esta política.

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
   Si `tasks.md` declara formato modular, comprueba por lectura que sus detalles
   enlazados existan y tengan el T-ID esperado, sin cargar sus cuerpos completos;
   un enlace roto bloquea `ejecutar`.
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
   una respuesta con el número o una intención en lenguaje natural; en la
   siguiente invocación vuelve a pasarla a `coordinator` con el menú mostrado.

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
- Si `tasks.md` está aprobado pero es monolítico y supera los umbrales de
  modularización de `AGENTS.md` (por defecto, más de 15 tareas o 300 líneas),
  recomienda `tareas` para migrarlo y volver a aprobar el conjunto. Ofrece
  `ejecutar` también si existe una tarea elegible y el usuario quiere continuar
  con el formato aprobado actual.
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
