# Configuración SDD y primera especificación

Aplica este inicio tanto a un proyecto nuevo como a uno existente. Actúa como
Tech Lead y sigue este orden sin escribir código de producto. En un proyecto
existente, la primera spec describe solo el cambio elegido; no reespecifica
todo el producto ni altera funcionalidades anteriores por adopción del kit.

FASE 1 — CONFIGURACIÓN
1. Lee completos `docs/constitution.md`, `AGENTS.md`, `MEMORY.md` y `specs/templates/spec.md`. Inspecciona el estado Git y las specs numeradas existentes; conserva archivos y cambios ajenos.
2. Detecta los campos pendientes. Si el proyecto ya funciona, contrasta stack, comandos, arquitectura y reglas con manifiestos, CI, código y documentación pertinentes. Registra incertidumbres o deriva documental; no presentes una inferencia como hecho verificado ni sobrescribas instrucciones existentes sin conciliarlas.
   Si el proyecto usará Claude Code, comprueba con evidencia del host o del
   usuario si carga `AGENTS.md` como instrucciones del proyecto. No crees
   `CLAUDE.md` por defecto. Cuando conste que no lo carga (por ejemplo, una
   versión anterior a 2.1.277, un proveedor sin soporte en la versión usada o
   **Project instructions** configurado para leer solo `CLAUDE.md`), y no
   exista ya un `CLAUDE.md` de proyecto, crea en la raíz exactamente:

   ```markdown
   # Compatibilidad con Claude Code

   @AGENTS.md
   ```

   Si ya existe, consérvalo y comprueba si importa `AGENTS.md`; no lo
   sobrescribas ni dupliques las reglas. Si no puedes determinar si el puente
   es necesario, pregunta antes de crearlo. Registra en `MEMORY.md` el motivo
   de crearlo o de conservar el existente.
3. Pregúntame de una en una solo las decisiones que no puedan resolverse con esa evidencia. Valida cada respuesta y actualiza los archivos correspondientes sin modificar código de producto. No inventes stack, comandos, arquitectura, reglas de dominio ni convenciones.
4. Si la carpeta no es un repositorio Git, pregúntame si deseo inicializarlo; no lo hagas sin mi confirmación explícita.
5. Si la configuración ya está documentada pero figura pendiente de aprobación, no la rehagas: comprueba que no queden placeholders y resume las decisiones. Solicita mi aprobación explícita solo si no la he dado para esta configuración en el mensaje actual. La ausencia de placeholders no equivale a aprobación.
6. Cuando apruebe la configuración actual, registra esa decisión en `MEMORY.md`: `**Estado:** Configuración aprobada; primera spec pendiente` y “Siguiente acción: Definir la primera spec con `inicio`”. Si ya consta esa aprobación, continúa sin solicitarla de nuevo. Detente aquí si falta la aprobación.

FASE 2 — PRIMERA ESPECIFICACIÓN
1. Con la configuración aprobada, comprueba si la primera spec ya se creó durante `inicio`. Si está en `Borrador` y esta conversación retoma su revisión o aprobación, continúa en FASE 3 sin crearla de nuevo. Si hay otra spec activa, informa su estado y dirige a `revisar` o a la etapa que corresponda. Si existen specs numeradas pero ninguna está activa, detente y ofrece `foco` o `nueva` según la intención.
2. Si aún no hay primera spec, pregúntame una cosa cada vez para definir el primer cambio que gestionará SDD. Puede ser una funcionalidad, una mejora de interfaz o experiencia, o una corrección acotada de un producto existente. Usa exactamente la estructura de `specs/templates/spec.md` y crea `specs/NNN-nombre-corto/spec.md`, donde `NNN` es el siguiente número monotónico disponible (001 si no hay specs numeradas). No sobrescribas ninguna existente.
3. Redacta requisitos atómicos, observables y verificables; identifica actores, datos, RNF, casos límite, fuera de alcance, supuestos y dependencias.
4. Registra en `MEMORY.md` la ruta relativa de esta spec como “Especificación activa”, su estado `Borrador`, y reinicia “Plan activo”, “Tareas activas” y “Última validación” a `Ninguno`.
5. Mantén su estado en `Borrador`; no diseñes arquitectura ni escribas código todavía.

FASE 3 — AUTO-QA Y APROBACIÓN
1. Revisa la spec como QA estricto: ambigüedades, contradicciones, requisitos no verificables, casos límite, privacidad/seguridad, RNF, dependencias y conflictos con la constitución.
2. Presenta los hallazgos por severidad y pregúntame una sola decisión cada vez.
3. Actualiza la spec y su historial con cada cambio acordado.
4. Verifica la matriz de trazabilidad, que no queden dudas bloqueantes ni placeholders, y que toda sección no aplicable esté justificada.
5. No marques la spec como `Aprobada` hasta recibir mi aprobación explícita. Cuando la reciba, actualiza estado, versión, historial y `MEMORY.md`; luego detente.
