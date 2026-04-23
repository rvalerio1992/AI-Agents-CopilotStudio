# AI-Agents-CopilotStudio

> Sistema de agentes de IA para **desarrollo de copilots conversacionales en Microsoft Copilot Studio** — Banco Promerica CR

Parte del ecosistema de agentes de IA de Banco Promerica, siguiendo el patrón
arquitectónico de **4 capas** (Observación → Auditoría → Enriquecimiento → Producción).

## 🎯 Propósito

Automatizar auditoría de topics, validación de flujos conversacionales, revisión de knowledge sources, generación de documentación de copilots, detección de inconsistencias entre copilots del banco.

## 🏗️ Arquitectura

Ver [ARCHITECTURE.md](./ARCHITECTURE.md) para el blueprint completo.

```
📦 AI-Agents-CopilotStudio
├── .github/
│   ├── agents/          ← Definiciones de agentes (.agent.md)
│   ├── prompts/         ← Prompts invocables (/nombre-prompt)
│   ├── skills/          ← Skills reutilizables (patterns, templates)
│   ├── instructions/    ← Reglas de estilo (applyTo patterns)
│   └── copilot-instructions.md
├── .vscode/
│   └── settings.json    ← Config para Agent Skills
├── outputs/             ← Artefactos generados por agentes
└── ARCHITECTURE.md      ← Blueprint del sistema
```

## 🚀 Uso rápido

### Prerrequisitos

- **VS Code 1.108+** con [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)
- Setting `chat.useAgentSkills: true` activado

### Flujo típico

```
1. Ubicá tu copilot exportado (`.zip` o carpeta) en `copilot-project/`
2. Ejecutar /explore-copilot para generar contexto base
3. Ejecutar agentes de auditoría según lo que necesites
4. Revisar reportes en outputs/
5. (Opcional) Aplicar mejoras con agentes de enriquecimiento
```

## 🗺️ Roadmap

### ✅ Fase 0 — MVP estructural (actual)
- Estructura base del repo
- Instrucciones de Copilot
- Arquitectura documentada
- Listo para sumar agentes específicos del dominio

### 🚧 Fase 1 — Fundación (próximo)
- `{domain}-explorer` — agente observador que genera contexto base
- Primer agente de auditoría específico del dominio

### 🔜 Fases siguientes
Se definen según casos de uso reales del equipo. Ver ARCHITECTURE.md.

## 🔗 Ecosistema

| Repo | Dominio |
|---|---|
| [AI-Agents-BI](https://github.com/rvalerio1992/AI-Agents-BI) | Power BI y reportes |
| [AI-Agents-DataScience](https://github.com/rvalerio1992/AI-Agents-DataScience) | Ciencia de datos |
| [AI-Agents-DataEngineering](https://github.com/rvalerio1992/AI-Agents-DataEngineering) | Pipelines y warehouse |
| [AI-Agents-CopilotStudio](https://github.com/rvalerio1992/AI-Agents-CopilotStudio) | Copilots conversacionales |

## 👥 Mantenedor

**Equipo de Data Science — Banco Promerica Costa Rica**
Roberto Valerio, Head of Data Science

## 📄 Estado

🟢 **Inicialización** — estructura base lista, agentes específicos pendientes de desarrollo según casos de uso reales.
