# Evals — Decisión de coincidencia factura/OC

| # | Tipo | Input (factura) | Esperado | v0 | v1 |
|---|---|---|---|---|---|
| E1 | típico | "Servicio de limpieza oficinas" vs OC "Limpieza mensual corporativo" | coincide | ✅ | ✅ |
| E2 | típico | "Papelería" vs OC "Papelería y artículos de oficina" | coincide | ✅ | ✅ |
| E3 | típico | "Mantenimiento AC" vs OC "Servicio de aire acondicionado" | coincide | ✅ | ✅ |
| E4 | típico | "Hosting web" vs OC "Hosting web anual" | coincide | ✅ | ✅ |
| E5 | típico | "Consultoría fiscal" vs OC "Asesoría contable" | coincide | ❌ | ✅ |
| E6 | límite | Factura $10,300 vs OC $10,000 (tolerancia 3%) | coincide | ❌ | ✅ |
| E7 | límite | Proveedor "Grupo ABC SA" vs OC "ABC Servicios" | coincide | ❌ | ✅ |
| E8 | límite | "Sillas ergonómicas" vs OC "Mobiliario oficina" | coincide | ✅ | ✅ |
| E9 | adversarial | Factura duplicada (mismo folio, distinta fecha) | rechazar | ✅ | ✅ |
| E10 | adversarial | Factura referencia OC-2847 (no existe en datos) | escalar | ❌ | ❌ |

**Tasa v0:** 6/10
**Tasa v1:** 9/10
**E10 pendiente:** necesita herramienta (consulta al ERP), no contexto.
