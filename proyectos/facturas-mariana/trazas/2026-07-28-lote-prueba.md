# Traza — Lote de prueba (28 jul 2026)

## Resumen

| Campo | Valor |
|---|---|
| Facturas en lote | 12 |
| Iteraciones | 5 |
| Salió por | **Éxito** |
| Costo | $8.40 MXN (~1,200 tokens promedio × 12 facturas) |
| Duración | 47 segundos |

## Detalle por iteración

| Iter | Facturas procesadas | Evento notable |
|---|---|---|
| 1 | 4 | — |
| 2 | 7 | — |
| 3 | 7 | ⚠️ JSON malformado en factura #8 — schema check lo cachó, reintento |
| 4 | 10 | Factura #8 procesada en reintento. #11 escalada (OC no encontrada → Ana) |
| 5 | 12 | Todas con veredicto (10 aprobadas, 1 rechazada, 1 escalada) |

## Decisiones del loop

- **Iter 3:** schema check detectó `{"decision": "aprobar", "justificacion": }` (JSON inválido). No contó como procesada. El loop reintentó — la verificación separada del trabajo funcionó.
- **Iter 4:** factura #11 referencia OC-2847 (no existe en datos). Disparó escalamiento a Ana por Slack. Contó como "procesada" con status `escalada`.

## Siguiente paso

E10/factura #11 salió por escalamiento como se esperaba. Para resolverlo sin escalar: necesita herramienta de consulta al ERP (S4).
