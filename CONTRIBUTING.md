# 🤝 Cómo contribuir

Guía para el equipo que va a sumar agentes nuevos a este repo.

## Antes de crear un agente nuevo

Preguntate:

1. **¿Hay un caso de uso real y repetitivo?** Si la tarea se hace 1 vez al mes, no vale la pena agentizarla.
2. **¿Qué capa corresponde?** (Observación / Auditoría / Enriquecimiento / Producción)
3. **¿Se puede hacer sin violar los guardrails?** Si el agente necesita cambiar lógica de negocio, probablemente NO deberías hacerlo.
4. **¿Qué consume el agente?** Idealmente `outputs/context/` del explorador. Si parsea desde cero, considerá si conviene ampliar el explorador primero.

## Checklist para cada agente nuevo

### 1. Archivo `.agent.md`

Crear `.github/agents/<nombre>.agent.md` con frontmatter:

```yaml
---
name: "Nombre Humano"
description: >
  Descripción corta y clara: qué hace, qué consume, qué produce,
  modo (read-only / read-write con confirmación).
tools:
  - read_file
  - create_file
  - ...
argument-hint: "Qué recibir como argumento opcional"
---
```

Secciones que debe tener el cuerpo:

- **Principio fundamental** — modo de operación y límites
- **Dependencias** — qué otros agentes/outputs consume
- **Tu flujo** — pasos concretos que sigue
- **Output estructurado** — esquema del JSON/MD que genera
- **Qué NUNCA hacés** — guardrails explícitos
- **Handoff** — qué recomendar al usuario después de ejecutar

### 2. Archivo `.prompt.md`

Crear `.github/prompts/<verbo>-<entidad>.prompt.md` con:

```yaml
---
description: "Qué hace el prompt en una línea"
mode: agent
---
```

Cuerpo con:
- Prerrequisitos (qué contexts/findings deben existir)
- Pasos que ejecuta
- Output esperado en chat
- Modo (read-only / dry-run / apply)

### 3. Skill (opcional)

Si el agente necesita patrones reutilizables (regex, templates), crear
`.github/skills/<skill-name>/SKILL.md`.

### 4. Validar con caso real

Antes de hacer commit:
- Correr el agente contra un caso real del banco
- Verificar que el output tenga sentido
- Documentar falsos positivos conocidos en el `.agent.md`

### 5. Actualizar ARCHITECTURE.md

Agregar el agente al roadmap y a la tabla de capas.

## Patrón de outputs

Siempre generar **3 archivos** mínimo:

- `<proyecto>_<agente>_<tipo>.json` — estructura para CI/CD
- `<proyecto>_<agente>_<tipo>.md` — reporte humano-legible
- (opcional) `<proyecto>_<agente>_<tipo>.csv` — si aplica

El prefijo `<proyecto>_` permite consolidación multi-proyecto más adelante.

## Scoring

Si el agente califica algo:

- **Usar escala 0-100**
- **Separar dimensiones** cuando tenga sentido (ej: estructural vs documentación)
- **Definir penalizaciones explícitas** por severidad
- **Documentar pesos** en el JSON output

## Confianza en outputs

Cuando el agente infiere algo (ej: generar descripciones automáticas):

- 🟢 **Alta** — regla clara, aplicar sin revisar
- 🟡 **Media** — inferencia razonable, revisar antes de aplicar
- 🟠 **Baja** — requiere contexto humano, dejar `[TODO: validar]`

## Code review

Antes de hacer merge:

- [ ] El `.agent.md` tiene todas las secciones requeridas
- [ ] El prompt valida prerrequisitos antes de correr
- [ ] Los outputs siguen el patrón `<proyecto>_<agente>_*`
- [ ] Hay al menos un caso real probado
- [ ] ARCHITECTURE.md actualizado
- [ ] Falsos positivos conocidos documentados

## Convenciones de naming

- **Agentes:** `kebab-case-agent.agent.md`
- **Prompts:** `verbo-entidad.prompt.md` (ej: `audit-sources`, `document-model`)
- **Skills:** `kebab-case/SKILL.md`
- **Instructions:** `dominio.instructions.md`

## Referencias

- Ver [AI-Agents-BI](https://github.com/rvalerio1992/AI-Agents-BI) como
  ejemplo de repo maduro con agentes ya implementados y probados.
- Ver [VS Code Agent Skills docs](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
  para entender cómo Copilot consume estos archivos.
