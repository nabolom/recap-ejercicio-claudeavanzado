# Gobernanza — Facturas de Mariana

> Cada sección cierra un hallazgo de la auditoría. El límite duro va en el entorno, no en el prompt.

## 1. Permisos mínimos

| Recurso / credencial | Acceso concedido | ¿Solo-lectura? | Por qué es el mínimo |
|---|---|---|---|
| Directorio de facturas XML | Lectura | ✅ Sí (corregido) | Solo necesita leer el XML, nunca modificarlo |
| Directorio de OCs | Lectura | ✅ Sí | Consulta, no escritura |
| `estado/` (procesadas, lote) | Lectura + escritura | ❌ No | Necesita registrar progreso — es el harness |
| API Claude (Sonnet 4) | Chat completions | — | Sin acceso a fine-tuning, sin acceso a otros modelos |

**Regla de la allowlist:**

| Dominio / API | Qué capacidad concede |
|---|---|
| `api.anthropic.com` | Chat completions — puede generar texto, NO puede subir archivos ni crear claves |
| Slack webhook (`#facturas-escalar`) | Enviar mensajes a UN canal — no puede leer ni borrar |
| Directorio local `/facturas/entrantes/` | Leer XMLs — no puede escribir, mover ni borrar |

## 2. Tope de gasto

| Límite | Número | Dónde se enforce | Qué pasa al tocarlo |
|---|---|---|---|
| Gasto mensual | $500 MXN | Consola Anthropic → Usage limits | Se bloquea la API key; alerta a Mariana |
| Tokens por corrida | 50,000 | Script del loop (contador) | Para el loop, registra en traza |
| Iteraciones por corrida | 20 | `loop.md` (parada de presupuesto) | Para el loop con status `presupuesto` |

> Verificado en consola Anthropic el 4 ago 2026: Usage Limit configurado en $500 MXN/mes.

## 3. Human-in-the-loop — solo lo irreversible

| Acción irreversible | ¿Requiere aprobación? | Quién aprueba | Por qué canal | Qué ve antes de aprobar |
|---|---|---|---|---|
| Mandar Slack de escalamiento | ✅ Sí (nuevo) | Mariana | Terminal (prompt y/n) | Folio, monto, motivo, destinatario |
| Aprobar factura > $50,000 | ✅ Sí | Ana | Slack DM con botón | Factura completa + OC + diff |
| Rechazar factura | ❌ No | — | — | Se registra pero no se ejecuta pago |
| Borrar/mover XML original | 🚫 Prohibido | — | — | El agente NO tiene permiso de escritura |

## 4. Error Trigger + observabilidad

| Qué falla | Cómo me entero | Canal | Umbral de alerta |
|---|---|---|---|
| API Claude cae / timeout | Error Trigger en el script | Slack #facturas-ops | 2 timeouts consecutivos |
| JSON fuera de schema (3 seguidos) | Parada de no-progreso | Slack #facturas-ops | 3 iteraciones sin avance |
| Presupuesto tocado | Parada del loop | Slack #facturas-ops + email Mariana | Al tocar el límite |
| Lote no termina en 10 min | Watchdog timer | Slack #facturas-ops | 10 min sin status `éxito` |

## 5. Trazabilidad — qué se registra por corrida

| Campo | Se registra | Dónde queda |
|---|---|---|
| Entrada (facturas del lote) | ✅ | `estado/lote-actual.json` |
| Decisión del modelo + confianza | ✅ | `trazas/<fecha>.md` por iteración |
| Acción tomada (aprobar/rechazar/escalar) | ✅ | `trazas/<fecha>.md` |
| Costo (tokens + $MXN) | ✅ | `trazas/<fecha>.md` campo "Costo" |
| Resultado verificado (schema check) | ✅ | `trazas/<fecha>.md` columna "Evento notable" |

## Hueco → cierre (de la auditoría)

| Hallazgo de `/auditar` | Sección que lo cierra | Estado |
|---|---|---|
| Sin tope de gasto en consola | §2 — $500 MXN/mes verificado | ✅ Cerrado |
| Escalamiento sin aprobación | §3 — prompt y/n antes de Slack | ✅ Cerrado |
| Sin Error Trigger | §4 — 4 disparadores configurados | ✅ Cerrado |
| Credencial de escritura innecesaria | §1 — cambiado a solo-lectura | ✅ Cerrado |
| Sin registro de costo | §5 — campo en cada traza | ✅ Cerrado |
