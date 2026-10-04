---
name: sdd
description: Muestra un menú guiado y gestiona una etapa del flujo Spec-Driven Development mediante /sdd.
---

# Adaptador SDD para Claude Code

Lee completo `../../../.agents/skills/sdd/SKILL.md` y síguelo como fuente
canónica. Trata el texto escrito después de `/sdd` como los argumentos de la
invocación. Pasa la invocación a `coordinator` con el contexto de la
conversación, como exige la sección «Orquestación obligatoria»; no ejecutes la
etapa en el agente principal. No dupliques ni sustituyas las reglas del flujo
desde este adaptador.
