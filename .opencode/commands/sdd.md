---
description: Abre el menú guiado o ejecuta una etapa del flujo SDD
agent: coordinator
subtask: false
---

Usa la skill de proyecto `sdd` definida en `.agents/skills/sdd/SKILL.md`.
Ejecuta exactamente esta invocación y conserva sus gates y autorizaciones:

`$ARGUMENTS`

Si no se proporcionaron argumentos, abre la entrada guiada de la skill: revisa
el estado, muestra únicamente opciones válidas y espera la selección sin
modificar archivos.
