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
- Orquestación automática: la entrada SDD pasa por `coordinator`, que delega
  en `planner`, `implementer` o `reviewer` según la etapa.
- Seguridad por defecto: datos sintéticos, mínimo privilegio y autorización
  separada para dependencias, migraciones, producción o efectos externos.

## Flujo SDD

Primer ciclo del proyecto:

```text
inicio → plan → tareas → ejecutar × N → validar
```

`inicio` configura un proyecto nuevo o existente, espera la aprobación explícita
de esa configuración y conduce la primera spec. Si la configuración ya quedó
documentada, retoma desde el gate pendiente. No se ejecuta `nueva`
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

También puedes adoptar el kit en un proyecto existente: incorpora la skill,
sus adaptadores y los agentes; integra `AGENTS.md`, `docs/constitution.md`,
`MEMORY.md` y la plantilla de spec con los documentos reales, conservando las
reglas y cambios previos. Revisa cualquier conflicto antes de reemplazar un
archivo. No necesitas instalar dependencias para usar el kit: las herramientas
y comandos del producto se registran durante la configuración inicial.

### 2. Abrirlo con tu agente

| Host | Entrada única | Cómo llega al coordinador |
|---|---|---|
| Codex | `$sdd` | La skill delega en el agente personalizado `coordinator`. |
| Claude Code | `/sdd` | El adaptador carga la skill y delega en `coordinator`. |
| OpenCode | `/sdd` | El comando selecciona `coordinator` directamente. |

El starter ya no incluye `CLAUDE.md`: era un puente que solo importaba
`AGENTS.md`. Desde Claude Code 2.1.277, con la opción predeterminada de
**Project instructions** y sin otro `CLAUDE.md` de proyecto, Claude Code lee
`AGENTS.md` directamente. Así se evita mantener un archivo puente específico
para el mismo contexto. Esta lectura nativa no estaba disponible en Bedrock,
Vertex ni Foundry al publicarse 2.1.277; si usas uno de esos proveedores o una
versión anterior, `inicio` comprobará si necesitas el puente y creará en la
raíz `CLAUDE.md` con este contenido exacto:

```markdown
# Compatibilidad con Claude Code

@AGENTS.md
```

También aplica si **Project instructions** está configurado para leer solo
`CLAUDE.md`. Sin evidencia de esa necesidad, no se crea; si el proyecto ya
tiene uno, `inicio` lo conserva y revisa su relación con `AGENTS.md`.

También puedes indicar la etapa, por ejemplo `$sdd plan` o `/sdd plan`. No
necesitas mencionar manualmente a ningún agente. Si el host no puede invocar a
`coordinator` o al especialista requerido, el flujo se detiene en vez de
ejecutar la etapa sin delegación.

### 3. Configurar el proyecto

En Codex:

```text
$sdd inicio
```

En Claude Code u OpenCode:

```text
/sdd inicio
```

También puedes añadir contexto en lenguaje natural a la entrada:

```text
$sdd inicio Acabo de crear este proyecto.
```

Para un producto existente cuya configuración ya está documentada:

```text
$sdd inicio Retoma desde la aprobación pendiente de la configuración SDD.
```

La etapa `inicio` inspecciona el contexto disponible y pregunta una decisión
cada vez cuando falte información para completar:

- nombre y objetivo del proyecto;
- stack, versiones y entornos soportados;
- arquitectura y carpetas clave;
- comandos oficiales de instalación, ejecución, pruebas, análisis y build;
- idiomas y convenciones;
- reglas de dominio y riesgos conocidos;
- primer cambio que se convertirá en `specs/NNN-.../spec.md` (001 si aún no hay
  specs numeradas).

Si el proyecto ya existe, la primera spec abarca solo el cambio elegido; por
ejemplo, mejorar una vista o un recorrido de usuario. La aprobación de la
configuración se registra en `MEMORY.md` antes de crearla. El agente no inventará
respuestas ni avanzará sin las aprobaciones requeridas.

## Uso del equipo de agentes

| Agente | Responsabilidad |
|---|---|
| `coordinator` | Habla con el usuario, conserva los gates y delega cada etapa. No modifica archivos ni implementa código. |
| `planner` | Configura el starter y mantiene specs, planes y tareas. No implementa código de producto. |
| `implementer` | Ejecuta exactamente una tarea elegible, con rojo-verde o evidencia equivalente. |
| `reviewer` | Revisa specs como QA y valida la implementación requisito por requisito sin corregir defectos. |

Ejemplo en Codex:

```text
$sdd nueva Quiero que los usuarios puedan archivar tareas completadas.
```

El coordinador utiliza la skill SDD, transmite el contexto al agente apropiado y
te devuelve las preguntas, aprobaciones o resultados. `nueva` corresponde a
ciclos posteriores; durante el primer ciclo usa `inicio`. No necesitas invocar
manualmente a ninguno de los cuatro agentes.

## Modelos opcionales por proyecto

El starter **no cambia modelos por defecto**. Si quieres que `coordinator`
elija un perfil de capacidad según el riesgo y la complejidad de cada encargo,
copia `SDD_MODELS.example.md` como `SDD_MODELS.md` junto a `AGENTS.md` y
`MEMORY.md`, y adapta el mapeo a los modelos disponibles en tu host.

Solo se lee `SDD_MODELS.md` cuando existe y antes de delegar una etapa a
`planner`, `implementer` o `reviewer`. Si falta, el comportamiento actual se
mantiene. Si un host no permite elegir modelo por delegación o una opción no
está disponible, se usa la herencia existente y se informa; nunca se omiten
gates o verificaciones para ahorrar tokens. El archivo de ejemplo no activa la
política por sí mismo. Cuando la política está activa, el coordinador informa
el perfil, modelo y esfuerzo solicitados en cada delegación y aclara si el
modelo efectivo no puede verificarse desde el host.

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
├── SDD_MODELS.example.md           # Ejemplo inactivo de política opcional de modelos
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
