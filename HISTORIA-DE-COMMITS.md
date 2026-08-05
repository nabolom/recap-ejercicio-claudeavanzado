# Historia de commits — recap-ejercicio-claudeavanzado

Cada commit es un paso del taller. El facilitador puede recorrerlos con
`git log --oneline` y mostrar el diff entre cualquier par.

| # | Mensaje de commit | Qué cambia | Qué muestra en clase |
|---|---|---|---|
| 0 | `baseline: estado al final de la S2 (9/10, E10 pendiente)` | Archivos de S1+S2 tal cual | El punto de partida: un sistema que acierta 9/10 pero donde el bucle sigue siendo el humano |
| 1 | `diagnostico: E10 es framework, no contexto — evidencia escrita` | `diagnostico.md` | La tabla de fallas con la columna de evidencia. Nadie escribe "modelo" sin descartar las otras tres |
| 2 | `harness: inventario de capas — el hueco es estado en disco` | `harness.md` | Qué existe, dónde vive, qué falta. Cada capa con al menos un hueco nombrado |
| 3 | `loop: 4 paradas, verificación separada, aritmética` | `loop.md` | Las cuatro paradas con números. El paso de verificación separado del de trabajo. La aritmética 0.95² = 0.90 |
| 4 | `traza: primera corrida — salió por éxito en 5 iteraciones` | `trazas/2026-07-28-lote-prueba.md` | Por cuál parada salió, cuánto costó, qué cambió entre iteraciones |
| 5 | `auditoria: 5 huecos de gobernanza — el tope de gasto y el error trigger son los criticos` | `auditoria-2026-08-04.md` | Los 5 huecos ordenados por impacto. Pregunta: "¿cuál cerrarías primero y por qué?" |
| 6 | `gobernanza: permisos, tope, HITL, error trigger y trazabilidad — 5/5 cerrados` | `gobernanza.md` | Cada sección cierra un hallazgo. El límite duro va en el entorno, no en el prompt |
| 7 | `tasa real: 8/10 end-to-end — E5 falla por bug de carga, E10 escala correctamente` | `trazas/2026-08-04-corrida-real.md` | La tasa de escritorio (9/10) baja en producción (8/10). La verdad que antes no veías |

## Qué mostrar en clase

### Sesión 3 (commits 1-4)

- **El diff del commit 1** — la tabla de fallas con evidencia. Pregunta al grupo: "¿por qué E10 no es contexto?"
- **El commit 3 completo** — las cuatro paradas. Pregunta: "¿qué pasa si le quito la de no-progreso?"
- **El commit 4** — la traza. Pregunta: "¿salió por donde esperaban?"

### Sesión 4 (commits 5-7)

- **El commit 5** — la auditoría. Pregunta: "¿cuál de los 5 huecos es el más peligroso a las 3 AM?"
- **El diff entre commit 4 y 6** — de "funciona en mi máquina" a "funciona con guardrails". Pregunta: "¿qué cambió en la OPERACIÓN, no en el código?"
- **El commit 7** — la tasa real. Pregunta: "¿por qué bajó de 9/10 a 8/10? ¿Es un retroceso o es honestidad?" (Respuesta: es honestidad — antes no veías el bug de carga porque corrías en escritorio con todo pre-cargado)
