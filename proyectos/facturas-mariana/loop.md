# Loop — Validación de facturas (lote)

## Ciclo

| Fase | Qué hace | Quién |
|---|---|---|
| **Trabajo** | Toma la siguiente factura pendiente del lote, la valida contra OC | El modelo (prompt v1) |
| **Verificación** | Valida que el JSON de salida tenga schema correcto y decisión en {aprobar, rechazar, escalar} | Un `if` — schema check, sin modelo |
| **Decisión** | ¿Sigo, paro, o escalo? Evalúa las 4 paradas | Lógica determinista |

## Las cuatro paradas

| Parada | Condición exacta | Verificable con |
|---|---|---|
| **Éxito** | `len(procesadas) == len(lote)` | Un `if` — sin preguntarle al modelo |
| **Presupuesto** | > 20 iteraciones O > $25 MXN en tokens por lote | Contador + API usage |
| **No-progreso** | 3 vueltas consecutivas sin que suba `len(procesadas)` | Comparación de contadores |
| **Escalamiento** | OC no encontrada (E10) O monto > $50,000 O confianza < 0.6 | Reglas deterministas sobre el output |

## Escalamiento

| Disparador | A quién | Por qué canal | Qué ve antes de decidir |
|---|---|---|---|
| OC no encontrada | Ana (supervisora) | Slack #facturas-escalar | Folio, monto, proveedor, motivo |
| Monto > $50,000 | Ana | Slack #facturas-escalar | Factura completa + OC + diff de monto |
| 3 fallos de schema seguidos | Mariana | Slack #facturas-ops | Log de los 3 intentos fallidos |

## Aritmética

- 2 pasos de IA por factura × 0.95 cada uno = 0.90 por factura
- Lote de 12 facturas: el loop reintenta las que fallan schema check
- Presupuesto de 20 iteraciones cubre el lote + reintentos razonables
