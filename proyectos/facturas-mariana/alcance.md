# Alcance — Facturas de proveedores

## Qué SÍ hace el sistema
- Valida conceptos de factura contra OC (matching semántico)
- Decide aprobar/rechazar/escalar según reglas + contexto

## Qué NO hace
- No busca OCs en el ERP (requiere integración — S4)
- No procesa notas de crédito
- No modifica montos ni aprueba pagos

## Definición de terminado
Una factura está "procesada" cuando tiene un veredicto (`aprobada` | `rechazada` | `escalada`) con justificación escrita y trazable al input.

## Condición de éxito verificable
`len(facturas_con_veredicto) == len(facturas_del_lote)` — sin preguntarle al modelo.
