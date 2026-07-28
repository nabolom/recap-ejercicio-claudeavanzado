# Diagnóstico por capas — Facturas de Mariana

## Fallas analizadas

| Falla | Síntoma | Capa | Evidencia |
|---|---|---|---|
| E10 (OC no existe) | El modelo inventa una respuesta en vez de escalar | **Framework** | No es que no entienda — es que la OC no está en los datos. Necesita una herramienta de consulta al ERP |
| Lote incompleto (prework) | Se procesan 8 de 12 facturas, sin saber cuáles faltan | **Harness** | No hay estado en disco: no registra procesadas vs pendientes |
| Factura duplicada no detectada (prework) | Procesa la misma factura dos veces | **Harness** | Sin memoria de folios ya procesados |

## Descartes escritos (E10)

| Capa descartada | Por qué no es esa |
|---|---|
| Contexto | Ya tiene tolerancias, alias y sinónimos. El problema no es que no entienda — es que la OC no está |
| Modelo | Con datos correctos, el modelo clasifica bien (9/10 en los otros) |
| Harness | El problema no es de estado o memoria — es de acceso a datos |

**Veredicto:** E10 → framework (necesita herramienta ERP). Los prework fails → harness (sin estado en disco).

**Decisión estratégica:** atacar harness HOY (archivos planos, resolvible en la sesión); ERP queda para la S4.
