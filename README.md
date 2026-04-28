# Obsidian Claude Agent — Segundo Cerebro con IA

> Sistema de gestión del conocimiento personal potenciado por LLM, construido sobre **Obsidian + Claude Code**. Combina context engineering, memoria persistente entre sesiones y captura automática de información en un vault estructurado.

---

## El problema que resuelve

Los gestores de conocimiento tradicionales (Notion, Obsidian sin IA) son pasivos: vos escribís, ellos guardan. Este sistema invierte la lógica: **el agente entiende quién sos, qué proyectos tenés activos y cómo pensás** — y actúa en consecuencia sin que tengas que re-explicar el contexto cada vez.

---

## Arquitectura

```
┌─────────────────────────────────────────────────────────┐
│                     USUARIO                             │
│            (VS Code + Claude Code extension)            │
└──────────────────────┬──────────────────────────────────┘
                       │ input natural language
                       ▼
┌─────────────────────────────────────────────────────────┐
│                  CLAUDE CODE (LLM)                      │
│                                                         │
│  ┌─────────────────┐    ┌──────────────────────────┐   │
│  │   CLAUDE.md     │    │  .claude/settings.json   │   │
│  │  (system prompt)│    │  (permisos del agente)   │   │
│  │                 │    └──────────────────────────┘   │
│  │ • Perfil usuario│                                    │
│  │ • Proyectos     │    ┌──────────────────────────┐   │
│  │ • Reglas        │    │  memory/ (persistencia)  │   │
│  │ • Convenciones  │    │  • user_*.md             │   │
│  └─────────────────┘    │  • project_*.md          │   │
│                         │  • feedback_*.md         │   │
│                         └──────────────────────────┘   │
└──────────────────────┬──────────────────────────────────┘
                       │ lee / escribe
                       ▼
┌─────────────────────────────────────────────────────────┐
│                  OBSIDIAN VAULT                         │
│                                                         │
│  daily-notes/   proyectos/   personas/   ideas/         │
│  inbox/         research/    resources/  templates/     │
│                                                         │
│  Convenciones: [[wikilinks]], #tags, YYYY-MM-DD         │
└─────────────────────────────────────────────────────────┘
```

---

## Stack tecnológico

| Componente | Tecnología |
|------------|------------|
| Agente LLM | Claude Code (Anthropic) via extensión VS Code |
| Base de conocimiento | Obsidian (markdown + wikilinks) |
| Context engineering | `CLAUDE.md` como system prompt estructurado |
| Memoria persistente | Archivos `.md` con frontmatter YAML |
| Control de acceso | `.claude/settings.json` (allowlist de herramientas) |
| Plantillas | Templater plugin de Obsidian |

---

## Características principales

### 1. Context Engineering via `CLAUDE.md`
El archivo `CLAUDE.md` actúa como system prompt persistente. Define:
- Perfil del usuario (rol, objetivos, forma de trabajar)
- Proyectos activos con estado y prioridades
- Reglas de comportamiento del agente
- Convenciones del vault (estructura, tags, formato de fechas)

No es solo configuración — es un modelo mental del usuario que el agente usa para tomar decisiones sin necesitar instrucciones explícitas cada vez.

### 2. Sistema de memoria entre sesiones
El agente escribe automáticamente en `memory/` archivos categorizados:
- `user_*.md` → preferencias y perfil del usuario
- `project_*.md` → estado de proyectos activos
- `feedback_*.md` → correcciones y patrones de trabajo validados
- `reference_*.md` → punteros a recursos externos

Cada archivo tiene frontmatter con `name`, `description` y `type`. El índice `MEMORY.md` se carga en cada sesión.

### 3. Captura automática
Regla central del sistema: **todo lo que el usuario menciona que quiere hacer, una idea o un plan → se captura inmediatamente** en `inbox/` o `ideas/` sin esperar instrucción explícita.

### 4. Control de permisos del agente
`.claude/settings.json` define qué herramientas puede ejecutar el agente (allowlist). Permite operar con seguridad en un vault personal sin riesgo de acciones no deseadas.

---

## Decisiones de diseño

**¿Por qué archivos markdown y no una base de datos?**
La memoria en archivos planos es inspeccionable, editable manualmente, compatible con git y no requiere infraestructura. El usuario mantiene control total sobre lo que el agente recuerda.

**¿Por qué `CLAUDE.md` y no el system prompt de la API?**
Claude Code inyecta automáticamente el `CLAUDE.md` del directorio de trabajo en cada sesión. Esto permite versionar el contexto junto con el vault y modificarlo sin tocar código.

**¿Por qué Obsidian y no Notion/Roam?**
Obsidian trabaja con archivos locales en markdown plano. El agente puede leer y escribir directamente con herramientas de filesystem, sin necesidad de APIs externas ni autenticación.

---

## Cómo configurar el tuyo

### Requisitos
- [Obsidian](https://obsidian.md/) con plugin Templater instalado
- [VS Code](https://code.visualstudio.com/) con extensión Claude Code
- Cuenta en [Anthropic](https://www.anthropic.com/) (Claude Code)

### Pasos

1. **Clonar este repo** dentro de tu vault de Obsidian (o como vault nuevo)
   ```bash
   git clone https://github.com/tu-usuario/obsidian-claude-agent
   ```

2. **Editar `CLAUDE.md`** con tu información personal, proyectos y reglas de comportamiento. Es el paso más importante — el agente es tan útil como el contexto que le des.

3. **Configurar permisos** en `.claude/settings.json` según las herramientas que quieras permitir.

4. **Abrir el vault en VS Code** y activar la extensión Claude Code.

5. **Primera sesión**: pedirle al agente que lea tu `CLAUDE.md` y que cree tu primera nota diaria.

---

## Estructura del vault

```
vault/
├── CLAUDE.md                  ← system prompt del agente
├── .claude/
│   └── settings.json          ← permisos del agente
├── memory/
│   ├── MEMORY.md              ← índice de memorias
│   └── *.md                   ← memorias por categoría
├── daily-notes/               ← una nota por día
├── proyectos/                 ← un .md por proyecto activo
├── personas/                  ← contactos relevantes
├── ideas/                     ← ideas sueltas
├── inbox/                     ← pendiente de procesar
├── research/                  ← investigaciones
├── resources/                 ← material de referencia
└── templates/                 ← plantillas base
    ├── daily-note.md
    ├── proyecto.md
    ├── persona.md
    └── research.md
```

---

## Tags del sistema

| Tag | Uso |
|-----|-----|
| `#daily` | Notas diarias |
| `#proyecto` | Proyectos |
| `#research` | Investigaciones |
| `#persona` | Contactos |
| `#idea` | Ideas sueltas |
| `#inbox` | Pendiente de procesar |
| `#estado/activo` | Proyecto en curso |
| `#estado/pausado` | Proyecto en pausa |
| `#estado/completado` | Proyecto terminado |
| `#prioridad/alta` | Urgente e importante |
| `#prioridad/media` | Importante, no urgente |
| `#prioridad/baja` | Para cuando haya tiempo |

---

## Flujo de trabajo

```
Captura → inbox/
    │
    ▼
Procesa → carpeta correcta + plantilla aplicada
    │
    ▼
Conecta → [[wikilinks]] a proyectos, personas, ideas
    │
    ▼
Revisa → daily note como punto de partida diario
```

---

## Por qué esto es relevante para Data Science

Este proyecto aplica conceptos directamente usados en sistemas LLM de producción:

- **Prompt engineering**: `CLAUDE.md` es un system prompt de producción real — no un ejemplo de tutorial
- **Agente con memoria**: implementación manual del patrón Read/Write memory en agentes LLM
- **Gestión de contexto**: diseño de qué información incluir en el contexto limitado de un LLM
- **Knowledge graph**: el vault con wikilinks forma un grafo de conocimiento navegable por el agente
- **Control de acceso**: allowlist de herramientas para operación segura en entorno personal

---

## Autor

Desarrollado por [Kevin Larguía](https://www.linkedin.com/in/kevin-larguia-42641638b/) — Ing. Informática (FICH UNL) + Diplomatura en Datos (Coderhouse).

---

## Licencia

MIT — usalo, modificalo, mejoralo.
