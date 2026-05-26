# Changelog — System Mapper

## v1.0.0 — 2026-05-26

### Lanzamiento inicial

- Análisis completo de codebase con generación de 5 archivos `.sqa/`
- Modos: sin flag (completo), `--quick`, `--diff [SHA]`, `--risk`, `--api`, `--deps`, `--update`
- Checklists: `architecture-patterns.md`, `risk-indicators.md`, `entry-points-patterns.md`
- Soporte para: Next.js, Laravel, EspoCRM, Django, Rails, Express/Fastify, Spring Boot, Go
- Análisis adaptativo: full para proyectos Small/Medium, muestreado para Large/Enterprise
- Modo `--diff` con Impact Analysis Report (veredicto: SEGURO / REVISAR / BLOQUEAR)
- Declaración explícita de limitaciones para evitar alucinaciones
- Integración complementaria con SQA Agent

### Frameworks probados en producción
- Next.js 16 (App Router) — Zeni Portal
- EspoCRM — CotrafaSocial
