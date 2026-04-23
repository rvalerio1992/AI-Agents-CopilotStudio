# 🏗️ Arquitectura de Agentes — Copilot Studio

> Blueprint del sistema de agentes de IA para desarrollo de copilots conversacionales en Microsoft Copilot Studio.
> Este documento es el **contrato arquitectónico** que guía la implementación.

---

## 🎯 Principios fundamentales

1. **Bajo acoplamiento** — agentes se comunican vía archivos en `outputs/`, no llamadas directas
2. **Trazabilidad total** — cada paso deja JSON/CSV/MD intermedios auditables
3. **Humano en el medio** — agentes de escritura siempre operan en DRY-RUN primero
4. **Idempotencia** — correr un agente 2 veces con el mismo input produce el mismo output
5. **Read-only por defecto** — escribir requiere flag explícito del usuario
6. **Separación de responsabilidades** — un agente = un propósito claro

---

## 🏛️ Arquitectura en 4 capas

Este repo sigue el **mismo patrón que los demás repos del ecosistema**:

```
┌─────────────────────────────────────────────────────────────┐
│ CAPA 1: OBSERVACIÓN (read-only)                             │
│ Agentes que generan contexto para que los demás lo consuman.│
│ Sin esta capa, cada agente tiene que reinventar el parseo.  │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ CAPA 2: AUDITORÍA (read-only)                               │
│ Detectan problemas y generan findings estructurados.        │
│ Output: reportes + JSON para CI/CD.                         │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ CAPA 3: ENRIQUECIMIENTO (read-write seguro)                 │
│ Aplican mejoras sin cambiar lógica fundamental.             │
│ Siempre operan DRY-RUN → diff → confirmación.               │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ CAPA 4: PRODUCCIÓN                                          │
│ Generan artefactos NUEVOS. No modifican existentes.         │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎚️ Niveles de autonomía por capa

| Capa | Autonomía | Razón |
|---|---|---|
| 1 — Observación | 🟢 **Alta** | Solo lee, no hay riesgo |
| 2 — Auditoría | 🟢 **Alta** | Solo genera reportes |
| 3 — Enriquecimiento | 🟡 **Media** | DRY-RUN por defecto, aplica con confirmación |
| 4 — Producción | 🟡 **Media** | Genera artefactos, humano decide si los usa |

---

## 🗺️ Roadmap de agentes

**Este repo está en fase de inicialización.** Los agentes específicos se
diseñan e implementan a medida que surgen casos de uso reales en el equipo.

### Fase 1 — Fundación (próximo)

| Agente | Capa | Estado |
|---|---|---|
| `explore-explorer` | 1 — Observación | ⏳ Pendiente de definir |

El primer agente de observación es **siempre la prioridad #1** en cada repo
porque es la base que los demás consumen.

### Fases 2-4 — Se definen con casos de uso reales

No inventamos agentes sin un caso de uso concreto. Cuando el equipo identifique
una tarea repetitiva que valga la pena automatizar, diseñamos el agente con:

1. **Propósito único y claro**
2. **Input/output bien definidos** (idealmente consumiendo `outputs/context/`)
3. **Guardrails de seguridad** (qué NO debe hacer)
4. **Scoring o métricas** si aplica
5. **Confianza etiquetada** en outputs (alta/media/baja/TODO) cuando hay inferencia

---

## 📁 Estructura de outputs

```
outputs/
├── context/             ← Producido por Capa 1
│   └── <entity>_context.json
├── audit/               ← Producido por Capa 2
│   ├── <entity>_<auditor>_findings.json
│   └── <entity>_<auditor>_review.md
├── <enriched>/          ← Producido por Capa 3
│   ├── _summary.md
│   └── <file>.<ext>.diff
└── <generated>/         ← Producido por Capa 4
```

Cada agente documenta sus propios outputs.

---

## 🛡️ Guardrails universales

Todos los agentes de este repo deben cumplir:

1. **Nunca modificar código fuente sin confirmación explícita**
2. **Generar backup** antes de cualquier escritura
3. **Generar diffs legibles** antes de aplicar cambios
4. **NO hacer commits git automáticos**
5. **NO inventar metadata** — si no está en los datos, no completarla
6. **NUNCA exponer credenciales** en outputs (filtrar passwords, keys, tokens)
7. **Distinguir niveles de output**:
   - *Hallazgo* — algo que existe y requiere atención
   - *Recomendación* — sugerencia de mejora
   - *Cambio automatizable* — acción ejecutable (con aprobación)

---

## 🔗 Ecosistema

Este repo es parte de un ecosistema de 4 repos con arquitectura idéntica:

| Repo | Dominio | Estado |
|---|---|---|
| [AI-Agents-BI](https://github.com/rvalerio1992/AI-Agents-BI) | Power BI y reportes | 🟢 Fase 2 en curso |
| [AI-Agents-DataScience](https://github.com/rvalerio1992/AI-Agents-DataScience) | Ciencia de datos | 🟡 MVP estructural |
| [AI-Agents-DataEngineering](https://github.com/rvalerio1992/AI-Agents-DataEngineering) | Pipelines y warehouse | 🟡 MVP estructural |
| [AI-Agents-CopilotStudio](https://github.com/rvalerio1992/AI-Agents-CopilotStudio) | Copilots conversacionales | 🟡 MVP estructural |

El repo `AI-Agents-BI` es el más maduro y sirve como **referencia
arquitectónica** para los demás.

---

## 📚 Referencias

- [VS Code Agent Skills documentation](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [GitHub Copilot — customization](https://docs.github.com/en/copilot/customizing-copilot)
- [data-goblin/power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) — inspiración inicial

---

**Mantenedor:** Equipo de Data Science — Banco Promerica Costa Rica
**Última actualización:** 2026-04-23
