# Auditoría de gobernanza — 4 agosto 2026

> Generada con `/auditar`. Cinco hallazgos, ordenados por impacto.

| # | Hallazgo | Severidad | Dónde duele |
|---|---|---|---|
| 1 | **Sin tope de gasto en la consola** — el loop puede correr indefinidamente si la parada de presupuesto falla | Alta | Factura sorpresa a fin de mes |
| 2 | **Escalamiento a Ana no tiene aprobación antes de mandar** — el Slack se dispara automáticamente | Media | Ana recibe ruido; si el disparador está mal calibrado, fatiga |
| 3 | **No hay Error Trigger** — si la API de Claude cae a las 3 AM, nadie se entera hasta que alguien revisa manualmente | Alta | Fallas invisibles |
| 4 | **Credencial de lectura al directorio de facturas es de escritura** — el agente podría sobreescribir un XML original | Media | Integridad de datos |
| 5 | **Sin registro de costo por corrida** — no se puede auditar gasto real vs presupuesto | Baja | No sabes si el $25 MXN se respeta |

## Plan de cierre

| Hallazgo | Se cierra en | Sección de `gobernanza.md` |
|---|---|---|
| 1 | Paso 2 (tope de gasto) | §2 |
| 2 | Paso 3 (human-in-the-loop) | §3 |
| 3 | Paso 3 (error trigger) | §4 |
| 4 | Paso 2 (permisos mínimos) | §1 |
| 5 | Paso 4 (trazabilidad) | §5 |
