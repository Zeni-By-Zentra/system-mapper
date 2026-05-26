# Entry Points Patterns — System Mapper Checklist

## Tipos de entry points por categoría

### HTTP / REST
```json
{
  "type": "http",
  "method": "GET | POST | PUT | PATCH | DELETE | OPTIONS",
  "path": "/api/v1/...",
  "handler": "src/app/api/.../route.ts",
  "auth_required": true,
  "auth_type": "session | jwt | api_key | basic | none",
  "rate_limited": true,
  "description": "..."
}
```

### WebSocket
```json
{
  "type": "websocket",
  "path": "/ws",
  "handler": "src/ws/server.ts",
  "auth_required": true,
  "auth_type": "token",
  "description": "Realtime connection for live updates"
}
```

### CLI / Commands
```json
{
  "type": "cli",
  "command": "php command.php rebuild | npm run migrate | python manage.py ...",
  "handler": "scripts/... | commands/...",
  "auth_required": false,
  "description": "..."
}
```

### Cron Jobs
```json
{
  "type": "cron",
  "schedule": "0 3 * * *",
  "handler": "src/app/api/cron/...",
  "auth_type": "secret_header | cron_service",
  "description": "Daily backup job"
}
```

### Webhooks (externos → sistema)
```json
{
  "type": "webhook",
  "method": "POST",
  "path": "/api/webhooks/stripe",
  "handler": "src/app/api/webhooks/stripe/route.ts",
  "auth_type": "hmac_signature",
  "external_source": "Stripe",
  "description": "Payment events from Stripe"
}
```

### Queue Workers / Jobs
```json
{
  "type": "queue",
  "queue_name": "emails | notifications | processing",
  "handler": "src/jobs/EmailJob.ts",
  "trigger": "enqueued by [quién]",
  "auth_required": false,
  "description": "..."
}
```

### Agent Bot / AI Webhooks
```json
{
  "type": "webhook",
  "method": "POST",
  "path": "/api/bot/webhook",
  "handler": "src/app/api/bot/webhook/route.ts",
  "auth_type": "token_param",
  "external_source": "Chatwoot AgentBot",
  "description": "Receives incoming messages from Chatwoot for AI processing"
}
```

---

## Cómo detectar entry points por framework

### Next.js App Router
```bash
# HTTP routes
find app/api -name "route.ts" -o -name "route.tsx" | sort

# Pages (also entry points for SSR)
find app -name "page.tsx" | sort

# Middleware (intercepts ALL)
ls middleware.ts 2>/dev/null

# Cron routes
find app/api -path "*/cron/*" -name "route.ts"
```

### Laravel
```bash
cat routes/web.php routes/api.php | grep -E "Route::(get|post|put|patch|delete|any)"
php artisan route:list 2>/dev/null
```

### Django
```bash
find . -name "urls.py" | xargs grep -h "path\|re_path\|url" 2>/dev/null | head -50
```

### Rails
```bash
cat config/routes.rb
rails routes 2>/dev/null | head -50
```

### Express
```bash
find . -name "*.ts" -o -name "*.js" | xargs grep -h "router\.\(get\|post\|put\|delete\|patch\)" 2>/dev/null | head -50
```

### EspoCRM
```bash
find custom/Espo/Custom -name "*.php" | xargs grep -l "Action" 2>/dev/null
ls application/Espo/Controllers/ 2>/dev/null | head -20
```

---

## Clasificación de criticidad de entry points

| Tipo | Criticidad por defecto | Razón |
|------|----------------------|-------|
| Auth endpoints (`/login`, `/auth/*`) | 🔴 | Acceso a todo el sistema |
| Payment endpoints (`/payment/*`, `/webhook/stripe`) | 🔴 | Dinero real |
| Admin endpoints (`/admin/*`, `/api/admin/*`) | 🔴 | Privilegios elevados |
| Webhook externos sin auth verificada | 🔴 | Vector de ataque |
| Cron jobs sin auth de header | 🟡 | Puede ejecutarse externamente |
| Public API endpoints sin rate limit | 🟡 | Vector de abuso |
| CRUD estándar con auth verificada | 🟢 | Normal |
| Webhooks con HMAC verification | 🟢 | Seguros |
| CLI commands (solo local) | 🟢 | No expuestos externamente |
