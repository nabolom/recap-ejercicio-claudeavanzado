# El caso completo — Mariana y las facturas, de la idea al sistema en producción

Este repo es el **caso de referencia del curso completo (S1–S4)**: el proyecto de
Mariana (validar facturas de proveedores contra su orden de compra) llevado paso a
paso por las cuatro sesiones. Todo lo que ves aquí salió del mismo método que tú
aplicas en tu propio repo.

**Para qué sirve:** para que compares tu proyecto contra un caso terminado. Si en
tu repo falta alguno de los archivos de la tabla de abajo, ahí está tu pendiente.

---

## El arco completo en una tabla

| Sesión | La pregunta que contesta | El número que deja | Los outcomes (haz clic para ver el de Mariana) | El agente te lo da con |
|---|---|---|---|---|
| **S1 — Diseño** | ¿Qué parte de mi proceso es `if`, qué es IA, qué es humano? | **0.95^n** — cada paso de IA multiplica el error; recorta pasos | [`proyecto.md`](proyectos/facturas-mariana/proyecto.md) · [`flujo.md`](proyectos/facturas-mariana/flujo.md) · [`alcance.md`](proyectos/facturas-mariana/alcance.md) · [`decisiones.md`](proyectos/facturas-mariana/decisiones.md) | `/arrancar` — te conduce por filtros → flujo → recorte → alcance, y escribe los 4 archivos |
| **S2 — Evals** | La IA decide en un paso... ¿pero decide BIEN? | **6/10 → 9/10** — la mejora se mide, no se siente | [`evals.md`](proyectos/facturas-mariana/evals.md) · [`prompts.md`](proyectos/facturas-mariana/prompts.md) · [`contexto/`](proyectos/facturas-mariana/contexto) | `/eval` — corre la suite, escribe la tasa; los casos los fijas TÚ |
| **S3 — Loops** | ¿Y quién corre el sistema? Hasta hoy, el bucle eras tú | **4 paradas** — éxito, presupuesto, no-progreso, escalamiento | [`diagnostico.md`](proyectos/facturas-mariana/diagnostico.md) · [`harness.md`](proyectos/facturas-mariana/harness.md) · [`loop.md`](proyectos/facturas-mariana/loop.md) · [`trazas/`](proyectos/facturas-mariana/trazas) | `/loop` — exige diagnóstico primero, te hace especificar las 4 paradas con números |
| **S4 — Gobernanza** | ¿Qué pasa cuando se rompe a las 3 AM? | **Tasa real: 8/10** — el número de escritorio baja en producción | [`auditoria-2026-08-04.md`](proyectos/facturas-mariana/auditoria-2026-08-04.md) · [`gobernanza.md`](proyectos/facturas-mariana/gobernanza.md) · [`trazas/2026-08-04-corrida-real.md`](proyectos/facturas-mariana/trazas/2026-08-04-corrida-real.md) | `/auditar` — te dice qué parte de tu sistema es teatro; la plantilla cierra cada hallazgo |

> **Cómo trabaja el agente:** tú no llenas plantillas a mano. Corres el comando en
> Claude Code, el agente te entrevista, y los archivos quedan escritos en disco y
> commiteados. La división nunca cambia: **el agente escribe, tú decides.** Las
> salidas esperadas de los evals, los números de las paradas y los umbrales de
> escalamiento son tuyos — un número copiado es una parada que no para.

---

## Sesión 1 — Diseño del sistema

**Para qué sirve:** evitar el error #1 de la automatización con IA: meterle IA a
todo. La aritmética manda — si tu flujo tiene 6 pasos de IA encadenados y cada uno
acierta el 95%, el sistema completo acierta 0.95⁶ ≈ **73%**. Uno de cada cuatro
casos sale mal.

**El paso a paso que siguió Mariana:**

1. Eligió UN proceso real y frecuente (facturas de proveedores) y lo pasó por los
   cuatro filtros: ¿es frecuente?, ¿es doloroso?, ¿tiene reglas?, ¿el error es tolerable?
2. Mapeó el flujo actual paso por paso y clasificó cada uno: `if` / IA / humano.
   Salieron 6 pasos marcados como IA.
3. Aplicó la aritmética: 0.95⁶ = 73.5%. Inaceptable. Revisó cada paso de IA y
   descubrió que 4 eran **`if` disfrazados** (reglas deterministas vestidas de
   criterio). Recortó: 6 → 2 pasos de IA → 0.95² = **90.3%**.
4. Firmó el contrato de alcance: qué hace el sistema, qué NO hace, y qué es
   "terminado" en términos verificables.
5. Dejó por escrito las 3 decisiones difíciles — las cajas donde de verdad se
   necesita criterio — como puente a la S2.

**Lo que deberías tener al terminar la S1** (en `proyectos/<tu-proyecto>/`):

| Archivo | Qué contiene |
|---|---|
| [`proyecto.md`](proyectos/facturas-mariana/proyecto.md) | El proceso elegido y los cuatro filtros contestados |
| [`flujo.md`](proyectos/facturas-mariana/flujo.md) | El flujo mapeado con cada paso clasificado (`if` / IA / humano) y el recorte documentado |
| [`alcance.md`](proyectos/facturas-mariana/alcance.md) | El contrato: qué sí, qué no, y la definición de terminado |
| [`decisiones.md`](proyectos/facturas-mariana/decisiones.md) | Las decisiones difíciles que quedaron en manos de la IA |

---

## Sesión 2 — Evals: medir la decisión

**Para qué sirve:** pasar de "creo que funciona" a "funciona el X% de las veces".
Sin evals, cada cambio de prompt es una apuesta a ciegas: puedes arreglar un caso
y romper tres sin enterarte.

**El paso a paso que siguió Mariana:**

1. **Escribió 10 casos ANTES de correr nada** — 5 típicos, 3 límite, 2 adversariales —
   con la respuesta correcta fijada por un humano ([`evals.md`](proyectos/facturas-mariana/evals.md)). La verdad se fija
   antes de medir; si escribes el prompt primero, escribes casos que tu prompt ya pasa.
2. **Escribió el prompt v0 pelón** y corrió la suite: **6/10**. El número feo se
   commiteó sin maquillar — es la línea base.
3. **Diagnosticó cada fallo con la escalera:** ¿regla ambigua? ¿falta un dato?
   ¿era un `if`? ¿el modelo? E6 era una tolerancia (un `if` disfrazado), E7 un alias
   de proveedor, E8 un sinónimo de catálogo.
4. **Agregó SOLO el contexto que los fallos pidieron** — tres archivos en
   [`contexto/`](proyectos/facturas-mariana/contexto), ni un documento más — y sacó el v1.
5. **Volvió a correr: 9/10.** El diff de [`evals.md`](proyectos/facturas-mariana/evals.md) entre v0 y v1 es la evidencia:
   un número que sube con un motivo escrito al lado.
6. **E10 no se arregló con contexto** — la factura referencia una OC que no está en
   los datos. Eso necesita una *herramienta* (ir a buscarla al ERP), no un dato.
   Quedó como puente a la S3.

**Lo que deberías tener al terminar la S2:**

| Archivo | Qué contiene |
|---|---|
| [`evals.md`](proyectos/facturas-mariana/evals.md) | Los 10 casos (5/3/2) con esperado vs obtenido, tasa v0 y tasa v1 |
| [`prompts.md`](proyectos/facturas-mariana/prompts.md) | El prompt versionado: v0 pelón y v1 con contexto — cada versión con su tasa |
| [`contexto/`](proyectos/facturas-mariana/contexto) | Solo los archivos que los evals reprobados pidieron |

---

## Sesión 3 — Loops: el sistema corre solo y para solo

**Para qué sirve:** hasta la S2, el bucle eras tú — tú pegabas el input, tú leías
el output, tú decidías si repetir. Un loop invierte eso: el sistema trabaja, se
verifica y se detiene solo. Pero un loop sin condiciones de parada no es
automatización — es una fuga de presupuesto.

**El paso a paso que siguió Mariana (los commits de este repo):**

1. **Diagnosticó ANTES de construir** ([`diagnostico.md`](proyectos/facturas-mariana/diagnostico.md)). Tomó los fallos reales
   (E10 + los del prework) y los clasificó por capa con evidencia escrita:
   - E10 → **framework**: no es que no entienda la OC, es que no la TIENE. Necesita
     una herramienta. Nadie escribe "modelo" sin descartar las otras tres capas.
   - El lote que se quedó a medias y la factura duplicada → **harness**: sin estado
     en disco, sin memoria de procesados.
2. **Inventarió sus cuatro capas** ([`harness.md`](proyectos/facturas-mariana/harness.md)): qué existe, dónde vive, qué
   falta. Cada capa con al menos un hueco nombrado. Y eligió el hueco a atacar
   primero: harness (estado en disco), porque se resuelve HOY con archivos planos;
   el ERP queda para la S4.
3. **Especificó el loop** ([`loop.md`](proyectos/facturas-mariana/loop.md)): ciclo de trabajo / verificación / decisión —
   donde **el paso que verifica no es el que trabaja** — y las cuatro paradas con
   números, no frases:

   | Parada | El número de Mariana |
   |---|---|
   | Éxito | `len(procesadas) == len(lote)` — verificable con un `if`, sin preguntarle al modelo |
   | Presupuesto | 20 iteraciones / $25 MXN por lote |
   | No-progreso | 3 vueltas sin que suba el conteo de procesadas |
   | Escalamiento | OC faltante o monto > $50,000 → Ana, con canal definido |

4. **Corrió y leyó la traza** ([`trazas/2026-07-28-lote-prueba.md`](proyectos/facturas-mariana/trazas/2026-07-28-lote-prueba.md)): salió por
   **éxito** en 5 iteraciones. La traza registra por cuál parada salió, cuánto
   costó, y un detalle clave: el schema check cachó un JSON malformado en la
   iteración 3 y el loop reintentó solo — la prueba viva de por qué la verificación
   va separada del trabajo.

**Lo que deberías tener al terminar la S3:**

| Archivo | Qué contiene |
|---|---|
| [`diagnostico.md`](proyectos/facturas-mariana/diagnostico.md) | Tus fallos clasificados por capa, con evidencia — no sensaciones |
| [`harness.md`](proyectos/facturas-mariana/harness.md) | El inventario de tus cuatro capas y el hueco que atacas primero |
| [`loop.md`](proyectos/facturas-mariana/loop.md) | Tu loop especificado: ciclo, cuatro paradas con números, aritmética |
| [`trazas/`](proyectos/facturas-mariana/trazas) | Al menos una corrida real: por cuál parada salió y qué aprendiste |

---

## Sesión 4 — Gobernanza: qué pasa cuando se rompe a las 3 AM

**Para qué sirve:** hasta la S3, tu sistema funciona en tu máquina, con tus ojos
encima. La gobernanza es lo que falta para que corra sin ti: quién lo frena, quién
paga, quién se entera si falla. Sin gobernanza, un loop autónomo es un riesgo
abierto — no una automatización.

**El paso a paso que siguió Mariana:**

1. **Corrió `/auditar`** ([`auditoria-2026-08-04.md`](proyectos/facturas-mariana/auditoria-2026-08-04.md)) — el agente revisó su
   sistema completo (loop, permisos, escalamiento, trazas) y encontró **5 huecos**,
   ordenados por impacto:
   - Sin tope de gasto en la consola (el loop puede gastar sin límite)
   - Escalamiento a Ana se dispara sin aprobación previa
   - Sin Error Trigger (si la API cae a las 3 AM, nadie se entera)
   - Credencial de escritura innecesaria (el agente podría sobreescribir XMLs)
   - Sin registro de costo por corrida

2. **Llenó la plantilla de gobernanza** ([`gobernanza.md`](proyectos/facturas-mariana/gobernanza.md)) — cinco secciones,
   cada una cierra un hallazgo:
   - **§1 Permisos mínimos:** allowlist de dominios + credenciales a solo-lectura
   - **§2 Tope de gasto:** $500 MXN/mes en consola Anthropic (verificado)
   - **§3 Human-in-the-loop:** aprobación antes de Slack, solo para lo irreversible
   - **§4 Error Trigger:** 4 disparadores con canal y umbral definido
   - **§5 Trazabilidad:** 5 campos por corrida, auditables

3. **Corrió el sistema end-to-end con gobernanza activa** ([`trazas/2026-08-04-corrida-real.md`](proyectos/facturas-mariana/trazas/2026-08-04-corrida-real.md)):
   - Tasa real: **8/10** (vs 9/10 en escritorio). ¿Por qué bajó? E5 falló por un
     bug de carga de contexto en la primera iteración — el sistema real no es el
     sistema de escritorio.
   - E10 escaló correctamente: pidió aprobación, Mariana dijo sí, Ana recibió el
     Slack en 3 segundos.
   - Costo: $9.10 MXN de $500 de presupuesto mensual.

4. **Identificó el siguiente paso:** fix del bug de carga (E5) + correr `/eval`
   para verificar que no rompe otros casos. El ciclo de mejora continua ya tiene
   infraestructura.

**Lo que deberías tener al terminar la S4:**

| Archivo | Qué contiene |
|---|---|
| [`auditoria-2026-08-04.md`](proyectos/facturas-mariana/auditoria-2026-08-04.md) | Los hallazgos de `/auditar` — qué parte de tu sistema es teatro |
| [`gobernanza.md`](proyectos/facturas-mariana/gobernanza.md) | Permisos, tope, HITL, error trigger y trazabilidad — cada hallazgo cerrado |
| [`trazas/2026-08-04-corrida-real.md`](proyectos/facturas-mariana/trazas/2026-08-04-corrida-real.md) | La corrida real con gobernanza activa — la tasa honesta del sistema |

> **El número que importa:** tu tasa de escritorio (S2) NO es tu tasa real. La S4
> te da el número honesto — el que pasa cuando el sistema corre solo, con sus
> permisos reales, sus límites reales y sus fallas reales. Si la tasa baja, no es
> un retroceso: es la verdad que antes no veías.

---

## Cómo leer este repo

La historia está en los commits — recórrela con:

```bash
git log --oneline
```

| # | Commit | Qué muestra |
|---|---|---|
| 0 | `baseline: estado al final de la S2 (9/10, E10 pendiente)` | El punto de partida |
| 1 | `diagnostico: E10 es framework, no contexto — evidencia escrita` | La tabla de fallas con evidencia |
| 2 | `harness: inventario de capas — el hueco es estado en disco` | Qué existe, qué falta, qué atacar primero |
| 3 | `loop: 4 paradas, verificación separada, aritmética` | Las cuatro paradas con números |
| 4 | `traza: primera corrida — salió por éxito en 5 iteraciones` | Por cuál parada salió y qué significa |
| 5 | `auditoria: 5 huecos de gobernanza` | Qué parte del sistema es teatro |
| 6 | `gobernanza: permisos, tope, HITL, error trigger y trazabilidad` | **El commit que importa de la S4** — 5/5 cerrados |
| 7 | `tasa real: 8/10 end-to-end` | La verdad: el número de escritorio baja en producción |

El detalle de cada commit y las preguntas para discusión están en
[`HISTORIA-DE-COMMITS.md`](HISTORIA-DE-COMMITS.md).

---

## El checklist completo (S1 + S2 + S3 + S4)

Si tu repo tiene todo esto, vas al día:

```
proyectos/<tu-proyecto>/
├── proyecto.md              ← S1: el proceso y los cuatro filtros
├── flujo.md                 ← S1: pasos clasificados if/IA/humano + recorte
├── alcance.md               ← S1: el contrato y la definición de terminado
├── decisiones.md            ← S1→S2: la decisión que se evalúa
├── evals.md                 ← S2: 10 casos, tasa v0 y v1
├── prompts.md               ← S2: prompt versionado con su tasa
├── contexto/                ← S2: solo lo que los fallos pidieron
├── diagnostico.md           ← S3: fallos clasificados por capa
├── harness.md               ← S3: inventario de capas y el hueco prioritario
├── loop.md                  ← S3: ciclo + cuatro paradas + aritmética
├── trazas/                  ← S3+S4: cada corrida, por cuál parada salió
├── auditoria-<fecha>.md     ← S4: hallazgos de /auditar
└── gobernanza.md            ← S4: permisos, tope, HITL, error trigger, trazabilidad

.claude/commands/
├── arrancar.md  auditar.md  verificar.md   ← S1
├── eval.md                                  ← S2
└── loop.md                                  ← S3
```

Lo que falte, está cubierto en el README del repo del curso
([ai-automation-expert](https://github.com/nabolom/ai-automation-expert)) con el
paso a paso y los `curl` para traer lo que no tengas.
