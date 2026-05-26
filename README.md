# System Mapper v1.0 — Software Architecture Mapper

> Skill para Claude Code que genera un mapa completo, persistente y a prueba de fallos de cualquier sistema de software. Reverse engineering para cualquier lenguaje o framework.

Desarrollado por [Zeni by Zentra](https://github.com/Zeni-By-Zentra) · Jhonatan Ortega · [webzentra.com](https://webzentra.com)

**MIT License** © 2026 Zentra · Jhonatan Ortega · webzentra.com

---

## Instalación

```bash
git clone https://github.com/Zeni-By-Zentra/system-mapper ~/.claude/skills/system-mapper
```

## Uso

```
/system-mapper [ruta | URL-GitHub | flags]
```

---

## Modos disponibles

| Flag | Qué hace |
|------|----------|
| *(sin flag)* | Análisis completo + genera los 5 archivos `.sqa/` |
| `--quick` | Resumen ejecutivo en < 3 min: top 10 archivos clave, arquitectura, zonas de riesgo |
| `--diff [SHA]` | Análisis de impacto: qué módulos cambiaron, qué dependencias se rompieron |
| `--risk` | Solo zonas de alto riesgo y guía de modificación segura |
| `--api` | Solo mapeo de endpoints HTTP |
| `--deps` | Solo grafo de dependencias en formato Mermaid |
| `--update` | Actualiza `.sqa/` existente con cambios recientes |

---

## Archivos que genera

```
.sqa/
├── SYSTEM_MAP.md          # Mapa de arquitectura legible para humanos
├── MODULE_INDEX.json      # Índice estructurado para máquinas
├── MEMORY.md              # Memoria de sesión — cárgala al inicio de cada sesión
├── DEPENDENCY_GRAPH.md    # Grafo de dependencias (formato Mermaid)
└── ENTRY_POINTS.json      # Todos los puntos de entrada del sistema
```

---

## Ejemplos

```bash
# Mapear el proyecto actual
/system-mapper

# Mapear rápido antes de una PR review
/system-mapper --quick

# Ver impacto de cambios antes de merge
/system-mapper --diff main

# Mapear proyecto externo
/system-mapper /opt/espocrm

# Solo ver endpoints de la API
/system-mapper --api src/app/api/

# Actualizar mapa después de un sprint
/system-mapper --update
```

---

## Proyectos compatibles

| Framework | Lenguaje | Estado |
|-----------|---------|--------|
| Next.js (App Router) | TypeScript | ✅ Probado |
| Laravel | PHP | ✅ Probado |
| EspoCRM | PHP | ✅ Probado |
| Django | Python | ✅ Compatible |
| Rails | Ruby | ✅ Compatible |
| Express / Fastify | Node.js | ✅ Compatible |
| Spring Boot | Java | ✅ Compatible |
| Go (net/http, Gin, Echo) | Go | ✅ Compatible |
| Monorepos | Cualquiera | ✅ Compatible (genera `.sqa-[subproyecto]/`) |

---

## Honestidad sobre limitaciones

- Para codebases con **+500 archivos**, el análisis es **muestreado**, no exhaustivo
- `safe_to_modify` es una **inferencia del AI**, no una garantía mecánica
- Dependencias en runtime (inyección dinámica) pueden no aparecer en el grafo

---

## Checklists de soporte

El skill carga dinámicamente desde GitHub:

| Checklist | URL |
|-----------|-----|
| Patrones de arquitectura | [architecture-patterns.md](checklists/architecture-patterns.md) |
| Indicadores de riesgo | [risk-indicators.md](checklists/risk-indicators.md) |
| Patrones de entry points | [entry-points-patterns.md](checklists/entry-points-patterns.md) |

---

## Complemento con SQA Agent

Este skill funciona perfectamente en conjunto con [sqa-agent](https://github.com/Zeni-By-Zentra/sqa-agent):

1. `/system-mapper` → entiende el sistema antes de tocarlo
2. Haces cambios
3. `/sqa-agent --diff` → audita la calidad de los cambios

---

## Licencia

MIT © 2026 Zentra · Jhonatan Ortega · webzentra.com  
Uso comercial, personal, educativo y open-source: completamente libre. Solo mantén la atribución.
