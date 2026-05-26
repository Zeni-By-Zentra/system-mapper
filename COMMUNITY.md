# Community — System Mapper

## Contribuir

¿Usaste system-mapper en un proyecto y quieres mejorar los checklists?

### Cómo agregar soporte para un framework nuevo

1. Fork del repo
2. Agrega el framework en `checklists/architecture-patterns.md` con:
   - Entry points típicos
   - Zonas de riesgo específicas
   - Comando de rebuild/restart
   - Zonas seguras para modificar
3. Prueba con un proyecto real de ese framework
4. PR con el nombre del framework y un ejemplo de output

### Reportar un hallazgo incorrecto

Si el skill clasificó un archivo como 🟢 cuando era 🔴, o viceversa:

- Abre un issue con: framework, archivo, clasificación recibida, clasificación correcta, por qué
- Actualiza `checklists/risk-indicators.md` con la señal que faltaba

### Casos de uso que agradeceríamos documentar

- Proyectos legacy sin documentación (el caso de uso más valioso)
- Monorepos grandes (+50 packages)
- Sistemas con arquitectura de microservicios
- Proyectos con múltiples lenguajes

## Autores

**Jhonatan Ortega** — [webzentra.com](https://webzentra.com)  
Founder de Zentra · Arquitecto del SQA Agent y System Mapper

## Licencia

MIT © 2026 Zentra · Jhonatan Ortega · webzentra.com  
Uso libre con atribución.
