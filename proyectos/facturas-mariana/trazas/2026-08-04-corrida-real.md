# Traza — Corrida real con gobernanza (4 ago 2026)

## Resumen

| Campo | Valor |
|---|---|
| Facturas en lote | 12 (mismo lote de prueba del 28 jul) |
| Iteraciones | 6 |
| Salió por | **Éxito** |
| Costo | $9.10 MXN |
| Duración | 52 segundos |
| Tasa de acierto end-to-end | **8/10** (vs 9/10 de escritorio en S2) |

## Por qué bajó de 9/10 a 8/10

| Eval | En escritorio (S2) | End-to-end (S4) | Por qué |
|---|---|---|---|
| E5 | ✅ | ❌ | Sin el contexto de sinónimos pre-cargado en la primera iteración, el modelo falló. Se corrigió en iter 2 al recargar contexto |
| E10 | ❌ (escalar) | ✅ (escaló correctamente) | El escalamiento ahora funciona: pidió aprobación, Mariana dijo sí, Ana recibió el Slack |

**Tasa real del sistema:** 8/10 en la primera pasada, 9/10 después del reintento de E5. La tasa honesta es **8/10** — el reintento consume presupuesto y no debería contar como "primera vez correcto".

## Gobernanza en acción

- **Aprobación HITL:** factura #11 (E10) disparó el prompt de aprobación. Mariana aprobó. Slack llegó a Ana en 3 seg.
- **Error Trigger:** no se disparó (ningún fallo de API ni timeout). Funciona: el watchdog corrió sin alertar.
- **Tope de gasto:** $9.10 de $500 mensuales. Margen amplio.
- **Trazabilidad:** esta traza registra los 5 campos. Auditable.

## Siguiente paso (mejora continua)

E5 falló por un bug de carga de contexto en la primera iteración. Postmortem: el script no pre-carga `contexto/` antes de la primera llamada. Fix: agregar paso de inicialización. Correr `/eval` después del fix para verificar que no rompe otros casos.
