# Risk Indicators — System Mapper Checklist

## Criterios de clasificación de riesgo

### 🔴 Crítico — Indicadores automáticos

Un archivo es automáticamente 🔴 Crítico si cumple UNO O MÁS de estos criterios:

**Por nombre:**
- Contiene: `middleware`, `auth`, `security`, `bootstrap`, `kernel`, `container`, `registry`, `provider`, `startup`, `init`, `main`, `server`, `app` (como archivo raíz)
- Es: `index.ts` / `index.php` / `index.py` en un directorio de librería (no en app)
- Es: `docker-compose.yml`, `.env` (real, no `.example`), `database.yml`, `settings.py`

**Por posición:**
- Está en la raíz del proyecto (no en un subdirectorio de features)
- Es el único archivo de su tipo en el proyecto (ej: único `router.ts`)

**Por acoplamiento:**
- Es importado por más de 10 archivos distintos
- Es la única interfaz con la base de datos (único ORM config)
- Es el único archivo de configuración de autenticación

**Por contenido:**
- Contiene credenciales, connection strings o API keys (aunque sea en variables de env)
- Define la inyección de dependencias del sistema completo
- Registra todos los routes/endpoints en un solo lugar

---

### 🟡 Medio — Indicadores

Un archivo es 🟡 Medio si:
- Es un Controller / Handler de una ruta que maneja auth o pagos
- Es un Model / Entity de dominio central (User, Subscription, Payment)
- Es importado por 4-10 archivos distintos
- Contiene lógica de negocio crítica (facturación, validación de permisos)
- Es un archivo de migración de DB (🟡 porque no se puede revertir fácilmente)
- Es middleware de un dominio específico (no global)

---

### 🟢 Bajo — Zona segura

Un archivo es 🟢 Bajo si:
- Es una View / Template / Componente de UI
- Es un archivo de test
- Es un archivo de i18n / traducciones / strings
- Es un script de utilidad o CLI helper
- Es un nuevo módulo de feature sin dependencias hacia otros módulos
- Es un Controller de un dominio secundario (ej: preferencias de usuario)
- Es documentación (README, CHANGELOG)

---

## Análisis de acoplamiento (para MODULE_INDEX.json)

Para determinar `safe_to_modify` en el JSON, usa esta heurística:

```
safe_to_modify = true  si:
  - risk_level = "low"
  - O risk_level = "medium" AND depended_by.length < 3 AND hay tests que lo cubren

safe_to_modify = false si:
  - risk_level = "critical"
  - O risk_level = "high"
  - O depended_by.length > 5 (independiente del nivel de riesgo)
  - O es el único archivo de su tipo crítico
```

---

## Señales de deuda técnica que aumentan el riesgo

| Señal | Impacto en riesgo |
|-------|-----------------|
| Archivo sin tests en zona 🟡+ | +1 nivel de riesgo |
| TODO/FIXME en archivo crítico | +1 nivel de riesgo |
| Último cambio hace >2 años | Estable pero frágil — documentar como "legacy crítico" |
| Mezcla de lógica de negocio y presentación | 🟡 mínimo |
| Más de 3 responsabilidades en un archivo (God Object) | 🟡 — difícil de predecir impacto |
| Dependencias circulares detectadas | 🔴 — alto riesgo de side effects |

---

## Cómo reportar zonas de riesgo en SYSTEM_MAP.md

Formato estándar para cada zona de riesgo:

```
| archivo/directorio | Razón del riesgo | Consecuencia si se rompe |
|--------------------|-----------------|--------------------------|
| middleware.ts | Intercept ALL requests | Sistema completo inaccesible |
| lib/db.ts | Única interfaz con DB | Pérdida total de datos/sesiones |
| app/api/auth/ | Auth logic for all users | Acceso no autorizado |
```

## Qué incluir en la "Guía de Modificación Segura"

Para proyectos en producción, siempre incluir en SYSTEM_MAP.md:

1. **Cómo añadir un endpoint nuevo** (sin tocar el router global)
2. **Cómo añadir un campo a la DB** (con migración, no modificando schema directamente)
3. **Cómo añadir middleware** (local vs global — la diferencia)
4. **Cómo hacer rollback** si algo sale mal

El ejemplo debe ser específico al framework detectado, no genérico.
