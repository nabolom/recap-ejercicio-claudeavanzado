# Inventario de capas — Facturas de Mariana

| Capa | Qué existe | Dónde vive | Qué falta |
|---|---|---|---|
| **Contexto** | Tolerancias, alias, sinónimos | `contexto/` (3 archivos) | Nada por ahora — cubre los 9/10 |
| **Modelo** | Claude Sonnet 4, prompt v1 | `prompts.md` | Nada — 9/10 es suficiente para los casos con datos |
| **Framework** | — | — | Herramienta de consulta al ERP para OCs faltantes |
| **Harness** | — | — | Estado en disco: registro de procesadas, folios vistos, lote pendiente |

## El hueco que desbloquea (hoy)

**Harness:** implementar estado en disco con archivos planos:
- `estado/procesadas.json` — lista de folios ya procesados
- `estado/lote-actual.json` — facturas del lote con status (pendiente/procesada/escalada)
- Check de duplicados antes de procesar

**Por qué este primero:** se resuelve con archivos planos en una sesión. El ERP requiere integración real (API, credenciales, manejo de errores) — eso es S4.

## Lo que NO se toca hoy

- Framework (ERP) → S4, cuando tengamos la integración
- Modelo → no hay evidencia de que sea el cuello de botella
