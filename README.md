# Base Project SDD

Starter kit portable para trabajar con **Spec-Driven Development (SDD)** en
Codex, Claude Code y OpenCode. Convierte una petición de producto en una cadena
trazable de especificación, plan, tareas, implementación y validación, con
aprobaciones explícitas entre etapas.

El repositorio no impone un lenguaje, framework ni arquitectura. Su primer paso
es configurar esas decisiones para el proyecto real mediante `inicio`.

## Qué aporta

- Una Constitución con principios, jerarquía documental y gates de aprobación.
- Una skill SDD canónica compartida por los tres hosts.
- Runbooks separados para especificar, planificar, ejecutar y validar.
- Artefactos persistentes y versionados dentro de `specs/`.
- Evidencia durable en `validation.md`; un resultado mostrado solo en el chat no
  cuenta como validación.
- Cuatro agentes especializados: `coordinator`, `planner`, `implementer` y
  `reviewer`.
- Seguridad por defecto: datos sintéticos, mínimo privilegio y autorización
  separada para dependencias, migraciones, producción o efectos externos.

## Flujo SDD

Primer ciclo del proyecto:

```text
inicio → plan → tareas → ejecutar × N → validar
```

`inicio` configura el starter y conduce la primera spec. No se ejecuta `nueva`
inmediatamente después.

Ciclos posteriores:

```text
nueva → plan → tareas → ejecutar × N → validar
```

Operaciones complementarias sobre una spec:

- `revisar`: corrige o cambia una spec existente sin ampliar silenciosamente su
  objetivo.
- `foco`: selecciona explícitamente la spec activa.
- `cancelar`: cancela una spec de forma recuperable y conserva su historial.

Consultas de solo lectura:

- `estado`: muestra el gate actual y los bloqueos.
- `ayuda`: explica las rutas disponibles y ejemplos de uso.

Cada invocación ejecuta una sola etapa y se detiene en su gate. Una tarea de
implementación requiere una invocación de `ejecutar`; nunca se completan todas
las tareas automáticamente.

## Inicio rápido

### 1. Obtener el starter

```bash
git clone git@github.com:HurtadoAndres96/base-project-SDD.git mi-proyecto
cd mi-proyecto
```

También puedes copiar su contenido sobre un proyecto nuevo. No necesitas
instalar dependencias para usar el kit: las herramientas y comandos del producto
se definen durante la configuración inicial.

### 2. Abrirlo con tu agente

| Host | Entrada SDD directa | Entrada multiagente |
|---|---|---|
| Codex | `$sdd` | Menciona `@coordinator` y describe la intención |
| Claude Code | `/sdd` | Usa `@coordinator` o inicia `claude --agent coordinator` |
| OpenCode | `/sdd` | Selecciona `coordinator` como agente primario con Tab |

La entrada directa aplica la skill desde el agente principal. La entrada
multiagente pone a `coordinator` al frente para delegar el trabajo especializado.

### 3. Configurar el proyecto

En Codex:

```text
$sdd inicio
```

En Claude Code u OpenCode:

```text
/sdd inicio
```

También puedes usar lenguaje natural con el coordinador:

```text
@coordinator Acabo de crear este proyecto. Inicia el flujo SDD.
```

La etapa `inicio` preguntará una decisión cada vez para completar:

- nombre y objetivo del proyecto;
- stack, versiones y entornos soportados;
- arquitectura y carpetas clave;
- comandos oficiales de instalación, ejecución, pruebas, análisis y build;
- idiomas y convenciones;
- reglas de dominio y riesgos conocidos;
- primera funcionalidad que se convertirá en `specs/001-.../spec.md`.

El agente no inventará respuestas ni avanzará sin las aprobaciones requeridas.

## Uso del equipo de agentes

| Agente | Responsabilidad |
|---|---|
| `coordinator` | Habla con el usuario, conserva los gates y delega cada etapa. No modifica archivos ni implementa código. |
| `planner` | Configura el starter y mantiene specs, planes y tareas. No implementa código de producto. |
| `implementer` | Ejecuta exactamente una tarea elegible, con rojo-verde o evidencia equivalente. |
| `reviewer` | Revisa specs como QA y valida la implementación requisito por requisito sin corregir defectos. |

Ejemplo en Codex:

```text
@coordinator Quiero que los usuarios puedan archivar tareas completadas.
```

El coordinador utiliza la skill SDD, transmite el contexto al agente apropiado y
te devuelve las preguntas, aprobaciones o resultados. No necesitas invocar
manualmente a los otros tres agentes.

## Gates y estados

La jerarquía de autoridad es:

```text
Constitución
  → spec aprobada
    → plan aprobado
      → tasks aprobadas
        → evidencia de validación
          → memoria operativa
```

Estados principales de una spec:

```text
Borrador → Aprobada → En implementación → Implementada → Validada
```

Una validación concluye con uno de estos veredictos:

- `VALIDADA`
- `VALIDACIÓN CONDICIONAL`
- `NO VALIDADA`
- `BLOQUEADA`

La preparación para producción se evalúa por separado. `Validada` no significa
automáticamente `LISTA PARA PRODUCCIÓN`.

En Codex, las etapas que escriben o ejecutan trabajo requieren modo
**Build/Default**. El menú, `estado` y `ayuda` funcionan también en modo de solo
lectura. El kit nunca cambia el modo del host por cuenta propia.

## Estructura

```text
.
├── AGENTS.md                       # Reglas permanentes del proyecto
├── MEMORY.md                       # Contexto operativo breve entre sesiones
├── docs/constitution.md            # Constitución y jerarquía normativa
├── specs/templates/spec.md         # Plantilla canónica de especificación
├── .agents/skills/sdd/             # Skill SDD y runbooks canónicos
├── .codex/agents/                  # Agentes para Codex
├── .claude/agents/                 # Agentes para Claude Code
├── .claude/skills/sdd/             # Adaptador de la skill para Claude Code
└── .opencode/
    ├── agents/                     # Agentes para OpenCode
    └── commands/sdd.md             # Comando /sdd
```

Los adaptadores no duplican el flujo. La fuente canónica es
`.agents/skills/sdd/SKILL.md` y cada etapa carga únicamente el runbook que le
corresponde.

## Principios esenciales

- Ningún comportamiento se implementa sin un requisito aprobado.
- Spec, plan y tareas requieren aprobaciones independientes.
- Los IDs aprobados nunca se renumeran ni reutilizan.
- Solo se implementa una tarea pequeña y elegible por vez.
- No se declara éxito sin verificación reproducible.
- Una validación se conserva en `validation.md` y no corrige defectos.
- Cambios de alcance invalidan de forma explícita los artefactos dependientes.
- No se almacenan secretos, datos personales ni evidencia sensible.

## Estado inicial del starter

Los placeholders de `AGENTS.md`, `docs/constitution.md` y `MEMORY.md` son
intencionales. Indican que el starter todavía no pertenece a un producto
concreto. Ejecuta `inicio` para reemplazarlos mediante decisiones explícitas.

## Licencia

Distribuido bajo la [licencia MIT](LICENSE).
