# Inicio del proyecto y primera especificación

Acabo de inicializar este proyecto con mi starter kit de Spec-Driven Development.
Actúa como Tech Lead y sigue este orden sin escribir código de producto.

FASE 1 — CONFIGURACIÓN
1. Lee completos `docs/constitution.md`, `AGENTS.md`, `MEMORY.md` y `specs/templates/spec.md`.
2. Detecta los campos pendientes de configuración en `AGENTS.md`, `docs/constitution.md` y `MEMORY.md`.
3. Hazme una sola pregunta cada vez. Espera mi respuesta, valida que sea concreta y actualiza directamente los archivos correspondientes.
4. No inventes stack, comandos, arquitectura, reglas de dominio ni convenciones.
5. Si la carpeta no es un repositorio Git, pregúntame si deseo inicializarlo; no lo hagas sin mi confirmación explícita.
6. Al terminar, comprueba que no queden placeholders de configuración y resume las decisiones para mi aprobación.

FASE 2 — PRIMERA ESPECIFICACIÓN
1. Después de que yo apruebe la configuración, pregúntame una cosa cada vez para definir la primera funcionalidad.
2. Usa exactamente la estructura de `specs/templates/spec.md` y crea `specs/001-nombre-corto/spec.md`.
3. Redacta requisitos atómicos, observables y verificables; identifica actores, datos, RNF, casos límite, fuera de alcance, supuestos y dependencias.
4. Registra en `MEMORY.md` la ruta relativa de esta spec como “Especificación activa”, su estado `Borrador`, y reinicia “Plan activo”, “Tareas activas” y “Última validación” a `Ninguno`.
5. Mantén su estado en `Borrador`; no diseñes arquitectura ni escribas código todavía.

FASE 3 — AUTO-QA Y APROBACIÓN
1. Revisa la spec como QA estricto: ambigüedades, contradicciones, requisitos no verificables, casos límite, privacidad/seguridad, RNF, dependencias y conflictos con la constitución.
2. Presenta los hallazgos por severidad y pregúntame una sola decisión cada vez.
3. Actualiza la spec y su historial con cada cambio acordado.
4. Verifica la matriz de trazabilidad, que no queden dudas bloqueantes ni placeholders, y que toda sección no aplicable esté justificada.
5. No marques la spec como `Aprobada` hasta recibir mi aprobación explícita. Cuando la reciba, actualiza estado, versión, historial y `MEMORY.md`; luego detente.
