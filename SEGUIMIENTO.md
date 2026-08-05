# Seguimiento por stage — Qué correr para ponerse al día

Copia y pega el bloque que corresponda al alumno según dónde se quedó.
Cada bloque es autoservicio: lo corren en su terminal + Claude Code y llegan al estado mínimo para la siguiente sesión.

---

## Te quedaste en la S1 (tienes repo pero no hiciste evals)

**Lo que necesitas para entrar a la S4:** `evals.md`, `prompts.md`, `loop.md` y al menos una traza.

**Tiempo estimado de recuperación:** 45-60 min con el agente.

### En tu terminal:

```bash
cd <tu-carpeta-del-repo>

# Trae los comandos y plantillas que te faltan
curl -o .claude/commands/eval.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/.claude/commands/eval.md
curl -o .claude/commands/loop.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/.claude/commands/loop.md
curl -o proyectos/PLANTILLA-evals.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-evals.md
curl -o proyectos/GUIA-escribir-evals.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/GUIA-escribir-evals.md
curl -o proyectos/PLANTILLA-loop.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-loop.md
curl -o proyectos/PLANTILLA-diagnostico.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-diagnostico.md
curl -o proyectos/PLANTILLA-harness.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-harness.md
curl -o proyectos/PLANTILLA-gobernanza.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-gobernanza.md

git add -A && git commit -m "sync: traigo comandos y plantillas de S2, S3 y S4" && git push
```

### En Claude Code:

```
Necesito ponerme al día. Tengo mi proyecto de la S1 (flujo.md, alcance.md, decisiones.md) pero no hice evals ni loop.

1. Ayúdame a escribir mi prompt v0 a partir de lo que ya tenemos en decisiones.md y reglas (si existen). Escríbelo en prompts.md.
2. Luego vamos caso por caso con los evals — proponme el input, yo te digo la salida esperada. Empecemos con 6 casos (4 típicos, 1 límite, 1 adversarial).
3. Cuando tenga los 6, corre /eval para la línea base.
4. Después hacemos el diagnóstico por capas y el loop.
```

**El mínimo para llegar a la S4:** tener `evals.md` con una tasa (aunque sea 4/10), `prompts.md` con al menos v0, y `loop.md` con las 4 paradas especificadas. No necesitas la traza perfecta — necesitas el esqueleto.

---

## Te quedaste en la S2 (tienes evals pero no hiciste loop)

**Lo que necesitas para entrar a la S4:** `diagnostico.md`, `harness.md`, `loop.md` y al menos una traza.

**Tiempo estimado de recuperación:** 30-40 min con el agente.

### En tu terminal:

```bash
cd <tu-carpeta-del-repo>

# Trae lo que te falta
curl -o .claude/commands/loop.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/.claude/commands/loop.md
curl -o proyectos/PLANTILLA-loop.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-loop.md
curl -o proyectos/PLANTILLA-diagnostico.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-diagnostico.md
curl -o proyectos/PLANTILLA-harness.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-harness.md
curl -o proyectos/PLANTILLA-gobernanza.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-gobernanza.md

git add -A && git commit -m "sync: traigo comandos y plantillas de S3 y S4" && git push
```

### En Claude Code:

```
Tengo mis evals (X/10) y mi prompt en v1. Necesito construir el loop.

1. Primero hagamos el diagnóstico: toma mis fallos de evals.md y clasifícalos por capa (contexto / modelo / framework / harness). Escríbelo en diagnostico.md.
2. Luego el inventario de capas en harness.md — qué tengo, qué falta.
3. Después corre /loop para especificar las 4 paradas con mis números reales.
```

**El mínimo para llegar a la S4:** tener `loop.md` con las 4 paradas llenas (con números, no frases). La traza es ideal pero no bloqueante — en la S4 la corrida end-to-end genera una traza nueva.

---

## Te quedaste en la S3 (tienes loop pero no corriste /auditar)

**Lo que necesitas para la S4:** nada extra que descargar. Ya tienes todo.

**Tiempo estimado de recuperación:** 0 min de setup, 20 min de taller.

### En tu terminal:

```bash
cd <tu-carpeta-del-repo>

# Solo verifica que tienes la plantilla de gobernanza
curl -o proyectos/PLANTILLA-gobernanza.md https://raw.githubusercontent.com/nabolom/ai-automation-expert/main/proyectos/PLANTILLA-gobernanza.md

git add -A && git commit -m "sync: plantilla de gobernanza para S4" && git push
```

### En Claude Code:

```
/auditar
```

Eso es todo. El agente revisa tu sistema completo y te dice qué parte es teatro. Después llenas `gobernanza.md` con la plantilla.

**Estás listo para la S4.** Solo necesitas correr `/auditar` al inicio del taller.

---

## Vas al día (terminaste la S3 con traza)

**Felicidades — eres el caso ideal.**

### Antes de la S4, solo verifica:

```bash
cd <tu-carpeta-del-repo>
ls .claude/commands/          # debe tener: arrancar, auditar, verificar, eval, loop
ls proyectos/<tu-proyecto>/   # debe tener: flujo, alcance, evals, prompts, loop, trazas/
```

Si todo está, llegas a la S4 y arrancas directo con `/auditar`.

---

## Caso extremo: no tengo repo (no vine a la S1)

**Tiempo estimado:** 10 min de setup + 60-90 min con el agente para llegar al estado mínimo.

### En tu terminal:

```bash
# 1. Ve a github.com/nabolom/ai-automation-expert
# 2. Click "Use this template" → "Create a new repository" → Private
# 3. Clona TU repo:
git clone https://github.com/<tu-usuario>/<tu-nombre-de-repo>.git
cd <tu-nombre-de-repo>
claude
```

### En Claude Code:

```
/arrancar
```

El agente te lleva por todo el flujo de la S1. Después sigue los bloques de arriba para S2 y S3.

**Realidad:** si alguien llega a la S4 sin haber hecho nada, no va a alcanzar a ponerse al día en el taller. La recomendación honesta es: haz la S1 y S2 como tarea antes de la sesión (1.5-2 horas), y en el taller arrancas desde la S3 o directamente con `/auditar` sobre lo que tengas.

---

## Resumen rápido para copiar-pegar en Slack

| ¿Dónde te quedaste? | Qué hacer | Tiempo |
|---|---|---|
| **S1 hecha, sin evals** | Baja comandos + corre el bloque de "ponerse al día" en Claude Code | 45-60 min |
| **S2 hecha, sin loop** | Baja `/loop` + plantillas + corre diagnóstico y loop | 30-40 min |
| **S3 hecha, sin auditar** | Solo baja la plantilla de gobernanza. Estás listo | 5 min |
| **Todo al día** | Verifica con `ls` y llega | 0 min |
| **Sin repo** | Crea desde template + `/arrancar` + sigue los bloques | 90+ min |

