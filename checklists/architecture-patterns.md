# Architecture Patterns — System Mapper Checklist

## Indicadores por patrón de arquitectura

### Next.js App Router (TypeScript)
- Entrada: `app/` directory con `page.tsx`, `layout.tsx`, `route.ts`
- API: `app/api/**/route.ts` — cada archivo es un endpoint
- Middleware: `middleware.ts` en raíz — 🔴 CRÍTICO
- DB: `lib/db.ts` o `lib/prisma.ts` — 🔴 CRÍTICO
- Auth: `lib/auth.ts`, `middleware.ts` — 🔴 CRÍTICO
- Config: `next.config.ts`, `.env*` — 🔴 CRÍTICO
- Safe zones: `components/`, `styles/`, `public/`
- Zonas de riesgo medio: `app/api/` (cualquier handler), `lib/`
- Comando reinicio: `pm2 restart [nombre]` o `npm run dev`

### Laravel (PHP)
- Entrada: `routes/web.php`, `routes/api.php` — 🟡 Medio
- Controllers: `app/Http/Controllers/` — 🟡 Medio
- Models: `app/Models/` — 🟡 Medio
- Middleware: `app/Http/Middleware/` — 🔴 CRÍTICO
- Service Container: `app/Providers/` — 🔴 CRÍTICO
- Bootstrap: `bootstrap/app.php` — 🔴 CRÍTICO
- Config: `config/` — 🟡 Medio
- Safe zones: `resources/views/`, `tests/`, `lang/`
- Migraciones: `database/migrations/` — 🟡 Medio (nunca modificar migración existente, solo crear nuevas)

### EspoCRM (PHP)
- Core: `application/` — 🔴 CRÍTICO (NUNCA modificar)
- Custom: `custom/` — 🟢 Zona segura para todo
- Metadata: `custom/Espo/Custom/Resources/metadata/` — 🟢 Seguro
- Rebuild: `php command.php rebuild` — obligatorio después de cambios
- Config: `data/config.php` — 🔴 CRÍTICO
- Safe zones: `custom/`, `client/custom/`

### Django (Python)
- Entrada: `urls.py` en cada app — 🟡 Medio
- Settings: `settings.py` o `settings/` — 🔴 CRÍTICO
- Models: `models.py` — 🟡 Medio (migrations necesarias)
- Middleware: `MIDDLEWARE` en settings — 🔴 CRÍTICO
- WSGI/ASGI: `wsgi.py`, `asgi.py` — 🔴 CRÍTICO
- Safe zones: `templates/`, `static/`, `tests/`
- Migraciones: `python manage.py makemigrations && migrate` — siempre

### Rails (Ruby)
- Entrada: `config/routes.rb` — 🟡 Medio
- Controllers: `app/controllers/` — 🟡 Medio
- Models: `app/models/` — 🟡 Medio
- Initializers: `config/initializers/` — 🔴 CRÍTICO
- Application: `config/application.rb` — 🔴 CRÍTICO
- Safe zones: `app/views/`, `spec/`, `test/`

### Express / Fastify (Node.js)
- Entrada: `index.ts`, `server.ts`, `app.ts` — 🔴 CRÍTICO
- Routes: `routes/` o `src/routes/` — 🟡 Medio
- Middleware: `middleware/` o registrado en app.ts — 🔴 CRÍTICO
- Config: `config/`, `.env` — 🔴 CRÍTICO
- Safe zones: `tests/`, `__tests__/`

### Spring Boot (Java)
- Entrada: `*Application.java` (main class) — 🔴 CRÍTICO
- Controllers: `@RestController` classes — 🟡 Medio
- Services: `@Service` classes — 🟡 Medio
- Config: `application.properties` / `application.yml` — 🔴 CRÍTICO
- Security: `SecurityConfig.java` — 🔴 CRÍTICO
- Safe zones: `src/test/`

### Go (net/http, Gin, Echo)
- Entrada: `main.go` — 🔴 CRÍTICO
- Routers: `router.go`, `routes.go` — 🟡 Medio
- Middleware: `middleware/` — 🔴 CRÍTICO
- Config: `config.go`, `.env`, `config.yaml` — 🔴 CRÍTICO
- Safe zones: `*_test.go` files

---

## Señales de arquitecturas especiales

### Monorepo
- Detectar: `packages/`, `apps/`, `libs/`, `workspace` en package.json / pnpm-workspace.yaml
- Acción: generar `.sqa-[subproyecto]/` para cada app/package principal
- Mapear dependencias cruzadas entre packages

### Microservicios
- Detectar: múltiples `docker-compose.yml`, múltiples `package.json` en subdirs
- Acción: mapear cada servicio por separado, documentar la comunicación entre ellos
- Entry points incluyen: colas (Redis, RabbitMQ, SQS), eventos, REST entre servicios

### BFF (Backend for Frontend)
- Detectar: `api/` dentro de un proyecto frontend (Next.js route handlers, Nuxt server routes)
- Acción: separar el mapa de rutas del frontend del de la API

---

## Señales de riesgo universal (cualquier stack)

| Señal | Riesgo |
|-------|--------|
| Archivo con >500 líneas | 🟡 — probablemente núcleo del sistema |
| Archivo importado por >10 módulos | 🔴 — cambio en cascada |
| Archivo de configuración de DB | 🔴 — siempre crítico |
| Archivo con "middleware", "auth", "security" en nombre | 🔴 — crítico |
| Archivo con "bootstrap", "init", "startup" en nombre | 🔴 — crítico |
| Archivo modificado hace más de 1 año sin cambios | 🟡 — estable pero frágil |
| Archivo sin tests asociados + 🔴 riesgo | 🔴 — máxima precaución |
