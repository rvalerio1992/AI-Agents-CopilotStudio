# Copilot Instructions — Copilot Studio

Este repo contiene agentes de IA para automatizar tareas de **desarrollo de copilots conversacionales en Microsoft Copilot Studio**
en Banco Promerica Costa Rica.

## Equipo

- **Líder:** Roberto Valerio, Head of Data Science
- **Banco:** Banco Promerica Costa Rica
- **Estructura organizacional:** Banca Personas, Banca Empresas, Banca Privada
- **Stack principal:** Python, PySpark, Azure Synapse, Azure Fabric, Power BI
- **Metodologías:** CRISP-DM para DS, GTD para priorización

## Arquitectura de agentes

Este repo sigue el **patrón de 4 capas** establecido en el ecosistema:

1. **🔍 Observación (read-only)** — generan contexto para los demás agentes
2. **🧪 Auditoría (read-only)** — detectan problemas y producen findings
3. **✨ Enriquecimiento (read-write seguro)** — aplican mejoras con DRY-RUN
4. **🚀 Producción** — generan artefactos nuevos

Ver [ARCHITECTURE.md](./ARCHITECTURE.md) para detalles.

## Repos hermanos del ecosistema

Todos siguen el mismo patrón arquitectónico:

- [AI-Agents-BI](https://github.com/rvalerio1992/AI-Agents-BI) — Power BI y reportes
- [AI-Agents-DataScience](https://github.com/rvalerio1992/AI-Agents-DataScience) — Proyectos de ciencia de datos
- [AI-Agents-DataEngineering](https://github.com/rvalerio1992/AI-Agents-DataEngineering) — Pipelines y warehouse
- [AI-Agents-CopilotStudio](https://github.com/rvalerio1992/AI-Agents-CopilotStudio) — Copilots conversacionales

## Principios fundamentales

1. **Bajo acoplamiento** — los agentes se comunican por archivos en `outputs/`
2. **Trazabilidad** — cada paso deja un JSON/MD auditable
3. **Humano en el medio** — agentes de escritura siempre DRY-RUN primero
4. **Idempotencia** — correr un agente 2 veces produce el mismo output
5. **Read-only por defecto** — escribir requiere flag explícito
6. **Un agente = un propósito** — separación clara de responsabilidades

## Qué NO hacer

- ❌ Inventar metadata de negocio (mejor dejar `[TODO: validar]`)
- ❌ Modificar código existente sin confirmación explícita del usuario
- ❌ Hacer commits git automáticos
- ❌ Exponer credenciales en outputs

## Estilo de comunicación

- **Idioma:** Español (código y comentarios pueden ser inglés si es convención del framework)
- **Tono:** Técnico, directo, sin florituras
- **Decisiones:** Cuando haya opciones, dar 2-4 alternativas con trade-offs claros
- **Honestidad:** Si algo tiene riesgo, decirlo explícitamente
