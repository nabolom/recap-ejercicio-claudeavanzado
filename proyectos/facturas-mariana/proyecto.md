# Proyecto: Validación de facturas de proveedores

## Los cuatro filtros

| Filtro | Respuesta |
|---|---|
| ¿Es frecuente? | Sí — ~120 facturas/semana |
| ¿Es doloroso? | Sí — 4 horas/día de Mariana, errores manuales |
| ¿Tiene reglas? | Sí — tolerancias, catálogo, OC obligatoria |
| ¿El error es tolerable? | Parcialmente — errores < $500 se absorben, > $500 requieren revisión |

## Proceso elegido

Validar cada factura de proveedor contra su orden de compra: montos, conceptos, datos fiscales. Hoy es 100% manual.
